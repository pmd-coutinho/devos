# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a BlueBuild-based custom Fedora Atomic OS image builder. It creates a personalized Fedora Silverblue image published to GitHub Container Registry (ghcr.io/pmd-coutinho/devos).

## Build System

Builds are automated via GitHub Actions (`.github/workflows/build.yml`):
- Triggers: push to main, daily at 06:00 UTC, or manual dispatch
- Uses `blue-build/github-action@v1.10` to build and sign OCI container images
- Images are signed with Sigstore Cosign (public key: `cosign.pub`)

There are no local build commands - all builds run in GitHub Actions.

## Architecture

```
recipes/recipe.yml     # Main BlueBuild recipe - defines base image, packages, flatpaks
files/system/          # Files copied into the OS root filesystem (/, /usr, /etc)
files/scripts/         # Custom build scripts executed during image build
.github/workflows/     # CI/CD pipeline configuration
```

## Recipe Configuration

The recipe (`recipes/recipe.yml`) defines:
- **Base image:** `ghcr.io/ublue-os/silverblue-main` (Fedora 42)
- **Modules:** files, dnf (package management), default-flatpaks, signing
- COPR repos and packages are added via the `dnf` module
- Flatpaks are configured via the `default-flatpaks` module

## Key Files

- `recipes/recipe.yml` - Primary configuration for the OS image
- `cosign.pub` - Public key for verifying image signatures
- `.github/workflows/build.yml` - GitHub Actions build pipeline

## BlueBuild Documentation

Refer to https://blue-build.org/learn/getting-started/ for BlueBuild module documentation and recipe syntax.
