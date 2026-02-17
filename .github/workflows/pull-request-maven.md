# pull-request-maven

Validerer Maven pull requests ved å kjøre `mvn verify`. Denne workflowen er ment å kjøres på alle pull requests for å sikre at koden kompilerer og testene passerer før merge.

Workflowen støtter også kodedekningsrapporter via JaCoCo. Når `with-coverage` er aktivert, genereres det dekningsrapporter som lastes opp som artifacts. Disse kan brukes videre av Sonar-workflowen for analyse.

## Forutsetninger

- Maven-prosjekt med `pom.xml`
- For coverage: JaCoCo må være konfigurert i prosjektet

## Eksempel

```yaml
pull-request:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-maven.yaml@main
  with:
    java-version: '21'
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Eksempel med coverage

```yaml
pull-request:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/pull-request-maven.yaml@main
  with:
    java-version: '21'
    with-coverage: true
    profile: 'enhetstest'
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Eksempel med flere test-profiler

For prosjekter som kjører enhetstester og integrasjonstester separat:

```yaml
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
```

