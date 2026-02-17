# deploy

Deployer et Docker-image til et NAIS-cluster. Denne actionen wrapper NAIS deploy og setter opp nødvendige variabler.

Actionen setter `image` og `VERSION` som variabler som kan brukes i NAIS-ressursfilen. `VERSION` settes til `<repo-navn>:<commit-sha>`.

## Forutsetninger

- NAIS-ressursfil som bruker `{{ image }}` variabelen
- Workflow må ha `id-token: write` permission for autentisering mot NAIS

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: navikt/familie-baks-gha-workflows/.github/actions/deploy@main
    with:
      image: 'europe-north1-docker.pkg.dev/nais-management-233d/teamfamilie/min-app:2025.02.20-18.48-c123d9b'
      cluster: 'dev-gcp'
      resource: '.nais/nais.yaml'
```

## NAIS-ressursfil

Ressursfilen må bruke `{{ image }}` for å referere til imaget:

```yaml
apiVersion: nais.io/v1alpha1
kind: Application
metadata:
  name: min-app
spec:
  image: {{ image }}
  # ...
```

