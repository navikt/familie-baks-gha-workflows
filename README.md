# familie-baks-gha-workflows
Felles GitHub Actions workflows for Team BAKS sine applikasjoner

# Zizmor – statisk analyse av GitHub Actions-workflows

[zizmor](https://docs.zizmor.sh) scanner workflows og actions under `.github/` for kjente
sikkerhetsproblemer (upinnede actions, for brede permissions, template injection osv.), som
anbefalt av AppSec i [security-playbook](https://sikkerhet.nav.no/docs/verktoy/zizmor).
Den delte workflowen `zizmor.yaml` kjører scanningen, laster opp SARIF til code scanning
(Security-fanen, category `zizmor`) og skriver en oppsummering av funnene til jobb-oppsummeringen.

zizmor-versjonen er pinnet i `zizmor.yaml` og bumpes med én endring her for hele fleeten.
Dette repoet scanner også seg selv via `zizmor-self-scan.yaml`.

## Ta i bruk i et repo

Legg til `.github/workflows/zizmor.yaml`:

```yaml
name: Zizmor

on:
  push:
    branches: [main]
    paths: ['.github/**']
  pull_request:
    paths: ['.github/**']
  schedule:
    - cron: '0 6 * * 1'
  workflow_dispatch:

permissions: {}

concurrency:
  group: ${{ github.workflow }}-${{ github.ref_name }}
  # Avbryt kun PR-kjøringer: en avbrutt kjøring på main laster ikke opp SARIF,
  # og alerts blir stående utdatert til neste trigger.
  cancel-in-progress: ${{ github.event_name == 'pull_request' }}

jobs:
  zizmor:
    uses: navikt/familie-baks-gha-workflows/.github/workflows/zizmor.yaml@main # ratchet:exclude
    permissions:
      security-events: write # laste opp SARIF til code scanning
      contents: read # sjekke ut koden som skal scannes
      actions: read # kreves av upload-sarif i private repoer
```

Legg til `.github/zizmor.yml`:

```yaml
rules:
  unpinned-uses:
    config:
      policies:
        "navikt/familie-baks-gha-workflows/*": ref-pin
        "*": hash-pin
  ref-version-mismatch:
    disable: true
```

* `unpinned-uses`: delte workflows og actions fra dette repoet refereres bevisst med `@main`
  (`# ratchet:exclude`), fordi main her er tillitsgrensen og callerne skal plukke opp
  forbedringer uten en PR i hvert repo. Alt annet må SHA-pinnes. Mønsteret må ha `/*` for å
  matche subpaths – `owner/repo` alene matcher ikke.
* `ref-version-mismatch`: midlertidig avskrudd fordi mange ratchet-kommentarer bruker
  major-tags (f.eks. `# ratchet:actions/checkout@v4`) som aldri matcher en eksakt SHA, så
  audit-en gir mest støy. Oppfølging er å normalisere kommentarene til eksakte versjoner og
  skru den på igjen. Selve SHA-pinningen håndheves uansett av `unpinned-uses`.

Andre unntak legges i samme fil, alltid med en begrunnelse.

## Inputs

| Input | Default | Beskrivelse |
| --- | --- | --- |
| `runs-on` | `ubuntu-latest` | Runner-image. |
| `fail-on-findings` | `false` | Feil jobben ved funn. Default er ikke-blokkerende: funn havner kun i Security-fanen og jobb-oppsummeringen. |
| `fail-on-severity` | `error` | Laveste SARIF-nivå som blokkerer når `fail-on-findings` er `true`: `error`, `warning` eller `note`. zizmor mapper High → error, Medium → warning og Informational → note. |

## Hvor havner funnene?

* **Security-fanen** (code scanning, category `zizmor`) – alle funn med detaljer, også på main.
* **Jobb-oppsummeringen** – antall funn per regel. Skrives alltid, også når SARIF ikke kan
  lastes opp (PR-er fra forks har read-only `GITHUB_TOKEN`).

zizmor setter aldri exit-kode ved funn i SARIF-modus, så blokkerende modus leses ut av
resultatfila i et eget steg. Exit-kode ≠ 0 fra selve scanningen betyr at zizmor feilet
(ugyldig konfig, crash) og feiler jobben uansett.

# Kontaktinformasjon
For NAV-interne kan henvendelser rettes til #team-familie på slack. Ellers kan man opprette et issue her på github.
