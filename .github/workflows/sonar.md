# sonar

Kjører [SonarCloud](https://sonarcloud.io/) analyse for kodekvalitet og testdekning. Denne workflowen er ment å kjøres etter at tester med coverage er fullført, typisk som en del av pull request-validering.

Workflowen laster ned coverage-rapporter fra tidligere jobber og sender dem til SonarCloud sammen med kodeanalysen. Dette gir innsikt i kodekvalitet, teknisk gjeld, duplisering og testdekning.

**Merk:** Workflowen kjører ikke for Dependabot PRs eller merge queue events.

## Forutsetninger

- Prosjektet må være konfigurert i SonarCloud
- JaCoCo coverage-rapporter må være generert og lastet opp som artifacts
- `SONAR_TOKEN` secret må være konfigurert

## Eksempel

```yaml
sonar:
  needs: [enhetstest, integrasjonstest]
  uses: navikt/familie-baks-gha-workflows/.github/workflows/sonar.yaml@main
  with:
    java-version: '21'
  secrets:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Eksempel med komplett PR-workflow

```yaml
name: Pull request

on:
  pull_request:

jobs:
  enhetstest:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-maven.yaml@main
    with:
      java-version: '21'
      with-coverage: true
      profile: 'enhetstest'
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  integrasjonstest:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-maven.yaml@main
    with:
      java-version: '21'
      with-coverage: true
      profile: 'integrasjonstest'
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  sonar:
    needs: [enhetstest, integrasjonstest]
    uses: navikt/familie-baks-gha-workflows/.github/workflows/sonar.yaml@main
    with:
      java-version: '21'
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

