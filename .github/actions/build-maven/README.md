# build-maven

Kjører Maven-kommandoer med standardiserte flagg og konfigurasjon. Denne actionen wrapper Maven-kall med fornuftige defaults for CI/CD-miljøer.

Actionen kjører Maven med følgende standardflagg:
- `--settings maven-settings.xml` - bruker settings generert av setup-java-maven
- `--errors` - viser feilmeldinger
- `--batch-mode` - ikke-interaktiv modus
- `--no-transfer-progress` - skjuler nedlastingsfremdrift
- `--quiet` - reduserer output

Ved testfeil kjøres testen automatisk på nytt opptil 2 ganger (med mindre tester er skippet).

## Forutsetninger

- `setup-java-maven` må ha kjørt først for å generere `maven-settings.xml`

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: navikt/familie-baks-gha-workflows/.github/actions/setup-java-maven@main
    with:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      java-version: '21'
  - uses: navikt/familie-baks-gha-workflows/.github/actions/build-maven@main
```

## Eksempel med verify og coverage

```yaml
steps:
  - uses: navikt/familie-baks-gha-workflows/.github/actions/build-maven@main
    with:
      maven-command: 'verify'
      profile: 'enhetstest'
      with-coverage: true
```

## Eksempel uten tester

```yaml
steps:
  - uses: navikt/familie-baks-gha-workflows/.github/actions/build-maven@main
    with:
      maven-command: 'package'
      skip-tests: true
```

