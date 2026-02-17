# pull-request-yarn

Validerer Yarn pull requests ved å bygge applikasjonen og kjøre tester. Denne workflowen er ment å kjøres på alle pull requests for å sikre at koden kompilerer, validerer og testene passerer før merge.

Workflowen kjører `yarn build`, `yarn validate`, `yarn test` og valgfritt `yarn cypress`. Den støtter også Sentry for feilsporing under bygging.

## Forutsetninger

- `package.json` med yarn-scripts: `build`, `validate`, `test`
- Valgfritt: `cypress` script for E2E-tester

## Eksempel

```yaml
pull-request:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-yarn.yaml@main
  with:
    node-version: '22'
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
```

## Eksempel med Cypress

```yaml
pull-request:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-yarn.yaml@main
  with:
    node-version: '22'
    skip-cypress: false
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
```

## Eksempel med Sentry

```yaml
pull-request:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-yarn.yaml@main
  with:
    node-version: '22'
  secrets:
    READER_TOKEN: ${{ secrets.READER_TOKEN }}
    SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
```

