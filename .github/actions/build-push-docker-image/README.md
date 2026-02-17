# build-push-docker-image

Bygger og pusher Docker-image til Google Artifact Registry (GAR) via NAIS sin docker-build-push action. Denne actionen brukes for å lage produksjonsklare images som kan deployes til NAIS.

Actionen genererer automatisk image-tags basert på tidsstempel og commit SHA. Den støtter også egendefinert SBOM (Software Bill of Materials) for bedre sårbarhetsanalyse.

Etter vellykket bygg vises image-navnet i GitHub Actions Summary.

## Forutsetninger

- `Dockerfile` i rot av prosjektet
- Workflow må ha `id-token: write` permission for autentisering mot GAR

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Build and push
    id: build-push
    uses: navikt/familie-baks-gha-workflows/.github/actions/build-push-docker-image@main
    with:
      push-image: true
  - name: Use image
    run: echo "Image: ${{ steps.build-push.outputs.image }}"
```

## Eksempel med image-suffix

For applikasjoner med flere images (f.eks. frontend og backend):

```yaml
steps:
  - uses: navikt/familie-baks-gha-workflows/.github/actions/build-push-docker-image@main
    with:
      push-image: true
      image-suffix: 'frontend'
```

## Eksempel med egendefinert SBOM

```yaml
steps:
  - uses: navikt/familie-baks-gha-workflows/.github/actions/build-push-docker-image@main
    with:
      push-image: true
      byosbom: 'target/bom.json'
```

