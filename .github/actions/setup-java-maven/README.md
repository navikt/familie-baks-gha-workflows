# setup-java-maven

Setter opp Java JDK og Maven for bygging av Maven-prosjekter. Denne actionen konfigurerer også tilgang til GitHub Packages for nedlasting av interne avhengigheter.

Actionen utfører følgende:
1. Genererer `maven-settings.xml` med tilgang til `maven.pkg.github.com/navikt/familie-felles`
2. Installerer Temurin JDK med angitt versjon
3. Konfigurerer Maven-cache for raskere bygg
4. Installerer Maven 3.9.5

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: navikt/familie-baks-gha-workflows/.github/actions/setup-java-maven@main
    with:
      github-token: ${{ secrets.GITHUB_TOKEN }}
      java-version: '21'
  - run: mvn --version
```

