# ktlint

Kjører [ktlint](https://ktlint.github.io/) statisk kodeanalyse på Kotlin-kode ved hjelp av Maven sin antrun-plugin.

ktlint er et verktøy som sjekker at Kotlin-koden følger en konsistent kodestil. Det hjelper med å holde kodebasen ryddig og lesbar ved å håndheve standardiserte formateringsregler. Denne workflowen er ment å kjøres på pull requests for å fange opp stilfeil før koden merges.

## Forutsetninger

Prosjektet må ha ktlint konfigurert i `pom.xml` med antrun-plugin:

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-antrun-plugin</artifactId>
  <executions>
    <execution>
      <id>ktlint</id>
      <!-- ktlint-konfigurasjon -->
    </execution>
  </executions>
</plugin>
```

## Eksempel

```yaml
ktlint:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/ktlint.yaml@main
  with:
    java-version: '21'
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

