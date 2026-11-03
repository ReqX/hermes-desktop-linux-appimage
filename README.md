# Hermes Desktop for Linux (AppImage)

[![Build & Publish](https://github.com/ReqX/hermes-desktop-linux-appimage/actions/workflows/build-appimage.yml/badge.svg)](https://github.com/ReqX/hermes-desktop-linux-appimage/actions/workflows/build-appimage.yml)
[![Release](https://img.shields.io/github/v/release/ReqX/hermes-desktop-linux-appimage?label=latest%20appimage)](https://github.com/ReqX/hermes-desktop-linux-appimage/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-purple.svg)](LICENSE)

Autonomous Linux x86_64 AppImage build pipeline for [Nous Research Hermes Agent](https://github.com/NousResearch/hermes-agent) desktop workspace.

---

## Overview

Official upstream releases of Hermes Agent package native desktop applications for macOS (`.dmg`) and Windows (`.msixbundle`), while Linux release legs remain unmaintained in upstream CI. 

This repository maintains an independent, automated build pipeline that tracks official release tags, compiles the Electron application in ephemeral GitHub-hosted environments, and delivers verified, standalone AppImages for Linux desktops.

### Key Highlights
- **100% Upstream Source:** Builds directly from official release tags of `NousResearch/hermes-agent`.
- **Reproducible & Ephemeral:** Compiled on standard `ubuntu-24.04` GitHub runners with no pre-baked binary dependencies.
- **Supply-Chain Integrity:** Verifies Git commit SHA against internal metadata stamps (`install-stamp.json`) and publishes SHA-256 checksums alongside every release.
- **Desktop Ready:** Single-file distribution ideal for immutable distributions (Bazzite, Fedora Atomic/Silverblue, SteamOS, openSUSE MicroOS) without host dependency pollution.

---

## Installation & Updates via Gear Lever

On immutable Linux distributions such as **Bazzite** or any modern desktop with [Gear Lever](https://github.com/mijorus/gearlever) installed:

1. Download the latest `Hermes-Desktop-x86_64.AppImage` from [Releases](https://github.com/ReqX/hermes-desktop-linux-appimage/releases).
2. Open **Gear Lever** and import the AppImage.
3. Under the application's update settings:
   | Setting | Value |
   |---|---|
   | **Update Manager** | `GithubUpdater` |
   | **Repository** | `ReqX/hermes-desktop-linux-appimage` |
   | **Asset Pattern** | `Hermes-Desktop-x86_64.AppImage` |
   | **Pre-releases** | Off |

Gear Lever will automatically poll for new releases, prompt for updates, replace the binary cleanly, and maintain your desktop integration seamlessly.

---

## Connecting to Hermes (Remote Gateway Mode)

Hermes Desktop operates natively as a remote client:

1. Launch **Hermes Desktop**.
2. On first run, select **Connect to existing Hermes**.
3. Provide your gateway or reverse proxy endpoint (e.g. `https://hermes.example.com` or local Tailscale FQDN).
4. Authenticate using your gateway credentials. 

All agent tools, shell executions, and models execute securely on your remote server; the AppImage serves purely as a lightweight, reactive GUI workspace.

---

## Manual Verification

Checksums are published with every release. To verify your downloaded package:

```bash
sha256sum -c Hermes-Desktop-x86_64.AppImage.sha256
```

---

## Build Automation Details

The automated pipeline (`.github/workflows/build-appimage.yml`) executes daily at 12:00 UTC and on manual dispatch:
1. Detects new stable releases on `NousResearch/hermes-agent` (supporting SemVer and CalVer schemes).
2. Validates if an existing release tag has already been built to prevent redundant executions.
3. Sets up Node.js 26+ runtime environment.
4. Executes native electron-builder compilation for `x86_64`.
5. Validates commit provenance, seals checksums, and publishes the GitHub release asset.

---

## License

This project is licensed under the [MIT License](LICENSE).  
Hermes Agent is developed and maintained by Nous Research. This repository is an independent community distribution created and maintained by [Mike Grossmann (ReqX)](https://github.com/ReqX).
