# build-yarn-app

Bygger en Yarn/Node-applikasjon og oppretter et Docker-image. Denne workflowen håndterer hele byggeprosessen inkludert validering, testing og containerisering.

Workflowen installerer avhengigheter, kjører `yarn build`, og kan valgfritt kjøre validering, tester og Cypress-tester. Den støtter også build-profiler for applikasjoner som har flere byggkonfigurasjoner (f.eks. `yarn build:frontend` og `yarn build:backend`).

## Forutsetninger

- Prosjektet må ha en `Dockerfile` i rot
- `package.json` med yarn-scripts: `build`, `validate`, `test`, `cypress`

## Eksempel

```yaml
build:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/build-yarn-app.yaml@main
  with:
    node-version: '22'
    push-image: true
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
```

## Eksempel med build-profil

For applikasjoner med flere bygg-targets:

```yaml
build-frontend:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/build-yarn-app.yaml@main
  with:
    node-version: '22'
    build-profile: 'frontend'
    push-image: true
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
```

## Eksempel med Sentry

```yaml
build:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/build-yarn-app.yaml@main
  with:
    node-version: '22'
    push-image: true
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
    SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
```

