# trivy

Kjører [Trivy](https://trivy.dev/) sårbarhetsskanning på et Docker-image og laster opp resultatene til GitHub Security.

Trivy skanner Docker-imaget for kjente sårbarheter (CVE-er) i operativsystem-pakker og applikasjonsavhengigheter. Kun sårbarheter med alvorlighetsgrad HIGH eller CRITICAL rapporteres.

Resultatene vises i GitHub Security-fanen under "Code scanning alerts".

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Build docker image
    id: build
    uses: navikt/familie-baks-gha-workflows/.github/actions/build-docker-image@main
  - name: Scan image
    uses: navikt/familie-baks-gha-workflows/.github/actions/trivy@main
    with:
      image: ${{ steps.build.outputs.image-ref }}
```

## Forutsetninger

- Workflow må ha `security-events: write` permission for å laste opp SARIF-resultater

