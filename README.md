# familie-baks-gha-workflows

Felles GitHub Actions workflows og actions for Team BAKS sine applikasjoner.

## Oversikt

Dette repoet inneholder gjenbrukbare workflows og composite actions som brukes på tvers av Team BAKS sine applikasjoner. Målet er å standardisere CI/CD-pipelines, redusere duplisering og gjøre det enklere å vedlikeholde bygg- og deploy-prosesser.

## Struktur

```
.github/
├── actions/          # Composite actions (gjenbrukbare steg)
│   ├── build-docker-image/
│   ├── build-maven/
│   ├── build-push-docker-image/
│   ├── deploy/
│   ├── setup-java-maven/
│   ├── setup-node-yarn/
│   └── trivy/
└── workflows/        # Reusable workflows (komplette pipelines)
    ├── build-maven-app.yaml
    ├── build-yarn-app.yaml
    ├── deploy.yaml
    ├── deploy-with-tag.yaml
    ├── ktlint.yaml
    ├── notify-slack.yaml
    ├── pull-request-maven.yaml
    ├── pull-request-yarn.yaml
    ├── scan-vulnerabilities-maven.yaml
    ├── scan-vulnerabilities-yarn.yaml
    └── sonar.yaml
```

## Konsepter

### Reusable Workflows vs Composite Actions

- **Reusable Workflows** (`workflows/`): Komplette pipelines som kan kalles fra andre repositories. Disse definerer hele jobber med permissions, runners og steg.
- **Composite Actions** (`actions/`): Gjenbrukbare steg som kan brukes inne i workflows. Disse er mer granulære og fleksible.

### Secrets

Workflows krever at secrets sendes eksplisitt fra kallende workflow. `GITHUB_TOKEN` er automatisk tilgjengelig i GitHub Actions, men må fortsatt sendes videre til reusable workflows.

```yaml
secrets:
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Versjoner

Alle eksterne actions er pinnet til spesifikke SHA-er med ratchet-kommentarer for sikkerhet og reproduserbarhet. Dependabot er konfigurert til å holde disse oppdatert.

## Bruk

### Kalle en workflow

```yaml
jobs:
  build:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/build-maven-app.yaml@main
    with:
      java-version: '21'
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Bruke en action

```yaml
steps:
  - uses: navikt/familie-baks-gha-workflows/.github/actions/setup-java-maven@main
    with:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      java-version: '21'
```

## Dokumentasjon

- [Workflows dokumentasjon](.github/workflows/README.md)
- [Actions dokumentasjon](.github/actions/README.md)

## Kontaktinformasjon

For NAV-interne kan henvendelser rettes til #team-familie på Slack. Ellers kan man opprette et issue her på GitHub.
