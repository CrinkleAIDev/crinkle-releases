# Crinkle — official releases

Official download repository for **[Crinkle](https://www.crinkle.dev)**, the autonomous
AI software team that runs on your machine and refuses to call a project done until the
evidence passes.

This repo hosts installers and their checksums only. The Crinkle application source is
**not** distributed here. Product site: [crinkle.dev](https://www.crinkle.dev) ·
benchmarks and methodology: [crinkle.dev/benchmarks](https://www.crinkle.dev/benchmarks.html).

> In independent grading, the baseline agent made **8 false completion claims** across the
> hard suite; Crinkle made **zero** — when the proof was incomplete, it did not call the
> project done. [See the benchmarks →](https://www.crinkle.dev/benchmarks.html)

## Install (Windows 10/11)

**Recommended — Microsoft Store** (one click, auto-updates, no SmartScreen warning):
[Get Crinkle from the Microsoft Store](https://apps.microsoft.com/detail/9PLZXNXNXT29).

**Any OS with Node 20+:** `npx crinkle`

**Standalone installer** (this repo):

1. Download [Crinkle-Setup.exe](https://github.com/CrinkleAIDev/crinkle-releases/releases/latest/download/Crinkle-Setup.exe) (about 104 MB).
2. Run it. The standalone build is not yet code-signed, so Windows SmartScreen will warn
   the first time: click **More info → Run anyway**. Verify the checksum below first if you
   want certainty about what you're running.
3. Crinkle opens as a desktop app. Connect any model you already have (OpenAI, Anthropic,
   Gemini, NVIDIA NIM, or a local model via Ollama / LM Studio / Open WebUI) and go.

## Verify your download

PowerShell:

```powershell
Get-FileHash "$env:USERPROFILE\Downloads\Crinkle-Setup.exe" -Algorithm SHA256
```

The output must match the table below exactly. You can also look the hash up on
VirusTotal: paste it into [virustotal.com](https://www.virustotal.com/gui/home/search).

| Version | File | SHA-256 |
|---|---|---|
| 0.2.0 (beta) | Crinkle-Setup.exe | `288A402EFCE5B41CF268E1853425C2007BF5E3AB4694AD8DD2B6CE10D2E01D3E` |
| 0.1.0-alpha.17 | Crinkle-Setup.exe | `D93682F585B790D9524745F665263E49D3B5941029E9659BC83294EB0DC350CF` |

Every release also ships `latest.yml`, electron-builder's update metadata, which embeds
its own SHA-512 of the installer.

## What runs where

- **Local:** your projects, code, run history, and provider API keys never leave your
  machine. Keys are encrypted at rest with Windows DPAPI. The agents, the browser-based
  verification, and the preview server all run locally.
- **Network calls the app makes:** (1) the AI providers **you** connect, with your own
  keys or subscriptions; (2) github.com, to check this repo for a newer release;
  (3) optional anonymous usage counters that you can turn off in Settings. There is no
  account requirement and no telemetry containing your code or prompts.
- Full details: [privacy policy](https://www.crinkle.dev/privacy.html) ·
  [terms](https://www.crinkle.dev/terms.html).

## Code signing

The beta installer is unsigned; Authenticode signing is planned before the stable
release (the release pipeline already refuses to ship a public beta build without a
valid signature once a certificate is configured). Until then, the checksum table above
is the integrity mechanism.

## History

Crinkle was briefly developed under the working name "Relay", which is why very old
links may mention `relay-releases` — GitHub redirects them here. Same product, same team.

## Support

support@crinkle.dev · [crinkle.dev](https://www.crinkle.dev)
