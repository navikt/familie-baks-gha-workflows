# deploy

Deployer et Docker-image til et NAIS-cluster. Denne workflowen brukes typisk etter en vellykket bygg-jobb for å rulle ut applikasjonen.

Workflowen tar imot full image-path (inkludert tag) og deployer til angitt cluster ved hjelp av NAIS deploy. Den krever at du har en NAIS-ressursfil (f.eks. `nais.yaml`) som definerer applikasjonskonfigurasjonen.

## Forutsetninger

- NAIS-ressursfil i repoet (f.eks. `.nais/nais.yaml`)
- Tilgang til angitt cluster

## Eksempel

```yaml
deploy-dev:
  needs: build
  uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy.yaml@main
  with:
    image: ${{ needs.build.outputs.image }}
    cluster: 'dev-gcp'
    resource: '.nais/nais.yaml'
```

## Eksempel med flere miljøer

```yaml
deploy-dev:
  needs: build
  uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy.yaml@main
  with:
    image: ${{ needs.build.outputs.image }}
    cluster: 'dev-gcp'
    resource: '.nais/nais-dev.yaml'

deploy-prod:
  needs: [build, deploy-dev]
  uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy.yaml@main
  with:
    image: ${{ needs.build.outputs.image }}
    cluster: 'prod-gcp'
    resource: '.nais/nais-prod.yaml'
```

