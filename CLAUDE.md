# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repo publishes multi-arch (`linux/amd64`, `linux/arm64`) Docker images for Flutter to Docker Hub as `marccardinal/flutter`. It is a fork of `cirruslabs/docker-images-flutter`, migrated from Cirrus CI to GitHub Actions.

Images build on top of `marccardinal/android-sdk:36` (maintained in the sibling repo `marccardinal/docker-images-android`).

## Key files

- `sdk/Dockerfile` — single Dockerfile; accepts `flutter_version` as a build arg; based on `docker.io/marccardinal/android-sdk:36`
- `.github/workflows/build.yml` — CI matrix; holds the authoritative `FLUTTER_VERSION` values for `latest`, `stable`, and `beta` tags; build-only on PRs, pushes on `master`
- `scripts/update_flutter_versions.sh` — fetches current stable/beta versions from the Flutter releases API and patches the matrix in `.github/workflows/build.yml` via `yq`
- `.github/workflows/check_flutter_versions.yml` — runs `update_flutter_versions.sh` on a schedule and opens a PR if versions changed

## How versions are updated

1. The `check_flutter_versions` workflow runs every 2 hours and calls `scripts/update_flutter_versions.sh`.
2. That script fetches `releases_linux.json` from Google and rewrites the three `FLUTTER_VERSION` entries in `.github/workflows/build.yml` using `yq`.
3. If anything changed, `peter-evans/create-pull-request` opens a PR automatically.
4. Merging to `master` triggers the build workflow, which builds and pushes the images.

To run the version update manually (requires `curl`, `jq`, `yq`):

```bash
bash scripts/update_flutter_versions.sh
```

## Building locally

```bash
docker build \
  --tag marccardinal/flutter:local \
  --build-arg flutter_version=3.44.0 \
  sdk
```

For multi-arch: add `--platform linux/amd64,linux/arm64` and use `docker buildx build`.

## CI secrets required

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
