# scan-vulnerabilities-maven

Kjører sikkerhetsscanning på Maven-prosjekter ved hjelp av CodeQL og Trivy. Denne workflowen bør kjøres regelmessig (f.eks. daglig eller ukentlig) for å oppdage sårbarheter i koden og avhengigheter.

Workflowen utfører to typer scanning:
- **CodeQL**: Statisk kodeanalyse som finner sikkerhetsproblemer i Java/Kotlin-kode og GitHub Actions
- **Trivy**: Container-scanning som finner sårbarheter i Docker-imaget og dets avhengigheter

Resultatene lastes opp til GitHub Security-fanen der de kan gjennomgås og håndteres.

## Forutsetninger

- Maven-prosjekt med `pom.xml`
- `Dockerfile` i rot

## Eksempel

```yaml
security-scan:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/scan-vulnerabilities-maven.yaml@main
  with:
    java-version: '21'
  secrets:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Eksempel med scheduled trigger

For å kjøre scanning automatisk hver natt:

```yaml
name: Security scan

on:
  schedule:
    - cron: '0 6 * * *'  # Hver dag kl. 06:00 UTC
  workflow_dispatch:      # Mulighet for manuell kjøring

jobs:
  scan:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/scan-vulnerabilities-maven.yaml@main
    with:
      java-version: '21'
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

