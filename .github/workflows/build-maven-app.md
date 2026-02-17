# build-maven-app

Bygger en Maven-applikasjon og oppretter et Docker-image. Denne workflowen håndterer hele byggeprosessen fra kompilering til containerisering.

Workflowen kjører `mvn install`, og kan valgfritt bygge og pushe Docker-image til Google Artifact Registry (GAR). Den støtter også egendefinert SBOM (Software Bill of Materials) for bedre sårbarhetsanalyse av nestede Maven-avhengigheter.

## Forutsetninger

- Prosjektet må ha en `Dockerfile` i rot
- Maven-prosjekt med `pom.xml`

## Eksempel

```yaml
build:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/build-maven-app.yaml@main
  with:
    java-version: '21'
    push-image: true
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Eksempel med SBOM

For bedre sårbarhetsanalyse kan du generere egen SBOM:

```yaml
build:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/build-maven-app.yaml@main
  with:
    java-version: '21'
    push-image: true
    byosbom: 'target/bom.json'
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

