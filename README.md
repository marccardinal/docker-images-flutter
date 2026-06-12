# Docker Images for [Flutter](https://flutter.dev/)

[![Build Status][build_badge]][build_link]

You can either use it in CI or run locally via Docker:

```bash
docker run --rm -it -v ${PWD}:/build --workdir /build marccardinal/flutter:stable flutter test
```

The example above mounts the current working directory and runs `flutter test`.

## Available tags

| Tag | Contents |
|-----|----------|
| `stable` | Latest stable Flutter release |
| `latest` | Same as `stable` |
| `beta` | Latest beta Flutter release |
| `<version>` | Specific Flutter version (e.g. `3.44.0`) |

## Docker Hub

https://hub.docker.com/r/marccardinal/flutter

## Secrets required

Add these secrets to the GitHub repository settings before the first push:

- `DOCKERHUB_USERNAME` — your Docker Hub username
- `DOCKERHUB_TOKEN` — a Docker Hub access token (not your password)

[build_badge]: https://github.com/marccardinal/docker-images-flutter/actions/workflows/build.yml/badge.svg
[build_link]: https://github.com/marccardinal/docker-images-flutter/actions/workflows/build.yml
