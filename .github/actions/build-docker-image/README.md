# build-docker-image

Bygger et Docker-image lokalt uten å pushe det til et registry. Denne actionen brukes primært for å bygge images som skal skannes for sårbarheter med Trivy.

Actionen setter opp Docker Buildx og bygger imaget med en enkel `docker build`-kommando. Imaget tagges med repository-navnet som standard.

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Build docker image
    id: build-docker-image
    uses: navikt/familie-baks-gha-workflows/.github/actions/build-docker-image@main
  - name: Scan image
    uses: navikt/familie-baks-gha-workflows/.github/actions/trivy@main
    with:
      image: ${{ steps.build-docker-image.outputs.image-ref }}
```

## Eksempel med egendefinert image-ref

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Build docker image
    uses: navikt/familie-baks-gha-workflows/.github/actions/build-docker-image@main
    with:
      image-ref: 'my-app:test'
```

