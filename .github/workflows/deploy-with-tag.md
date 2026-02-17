# deploy-with-tag

Deployer en spesifikk image-tag til et NAIS-cluster. Denne workflowen er nyttig for manuelle deploys eller rollbacks der du vil deploye en tidligere bygget versjon.

I motsetning til `deploy.yaml` som krever full image-path, tar denne workflowen kun imot en tag (f.eks. `2025.02.20-18.48-c123d9b`). Image-path konstrueres automatisk basert på repository-navnet.

## Forutsetninger

- NAIS-ressursfil i repoet (f.eks. `.nais/nais.yaml`)
- Image-taggen må finnes i Google Artifact Registry

## Eksempel

```yaml
manual-deploy:
  uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy-with-tag.yaml@main
  with:
    image-tag: '2025.02.20-18.48-c123d9b'
    cluster: 'dev-gcp'
    resource: '.nais/nais.yaml'
```

## Eksempel med workflow_dispatch

For å kunne trigge manuelt fra GitHub UI:

```yaml
name: Manual deploy

on:
  workflow_dispatch:
    inputs:
      image-tag:
        description: 'Image tag to deploy'
        required: true
      cluster:
        description: 'Cluster'
        required: true
        type: choice
        options:
          - dev-gcp
          - prod-gcp

jobs:
  deploy:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy-with-tag.yaml@main
    with:
      image-tag: ${{ inputs.image-tag }}
      cluster: ${{ inputs.cluster }}
      resource: '.nais/nais.yaml'
```

