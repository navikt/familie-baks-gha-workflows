# setup-node-yarn

Setter opp Node.js og Yarn for bygging av frontend-prosjekter. Denne actionen konfigurerer også tilgang til GitHub Packages for nedlasting av interne npm-pakker.

Actionen utfører følgende:
1. Installerer Node.js med angitt versjon
2. Konfigurerer npm registry til `npm.pkg.github.com`
3. Setter opp yarn-cache for raskere bygg
4. Kjører `yarn install` med `--immutable` for å sikre at lockfilen er oppdatert

## Eksempel

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: navikt/familie-baks-gha-workflows/.github/actions/setup-node-yarn@main
    with:
      node-version: '22'
      reader-token: ${{ secrets.READER_TOKEN }}
  - run: yarn build
```

