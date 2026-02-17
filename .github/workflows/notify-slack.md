# notify-slack

Sender en feilmelding til Slack når en workflow feiler. Denne workflowen brukes typisk som en separat jobb som kjøres når andre jobber feiler.

Meldingen inneholder workflow-navn, repository og en lenke til den feilede workflow-kjøringen slik at teamet raskt kan undersøke problemet.

## Forutsetninger

- Slack webhook URL må være konfigurert som en secret i repoet

## Eksempel

```yaml
notify:
  if: failure()
  needs: [build, deploy]
  uses: navikt/familie-baks-gha-workflows/.github/workflows/notify-slack.yaml@main
  secrets:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## Eksempel med komplett workflow

```yaml
name: Build and deploy

on:
  push:
    branches: [main]

jobs:
  build:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/build-maven-app.yaml@main
    with:
      java-version: '21'
      push-image: true
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  deploy:
    needs: build
    uses: navikt/familie-baks-gha-workflows/.github/workflows/deploy.yaml@main
    with:
      image: ${{ needs.build.outputs.image }}
      cluster: 'prod-gcp'
      resource: '.nais/nais.yaml'

  notify-on-failure:
    if: failure()
    needs: [build, deploy]
    uses: navikt/familie-baks-gha-workflows/.github/workflows/notify-slack.yaml@main
    secrets:
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

