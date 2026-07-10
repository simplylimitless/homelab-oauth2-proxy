# homelab-oauth2-proxy

A thin Docker wrapper around the official [`oauth2-proxy`](https://github.com/oauth2-proxy/oauth2-proxy) image, published to GitHub Container Registry (GHCR).

## Why a derivative image?

Keeping your own copy of the upstream `oauth2-proxy` image lets you pin to a known-good version, add custom config maps or middleware in a later layer, and avoid rate-limits on pulling from Docker Hub.

## Supported architectures

Published as a multi-arch manifest for:

- `linux/amd64`
- `linux/arm64`

Docker/Podman pull the correct variant for your host automatically. The upstream `oauth2-proxy` image also publishes `linux/arm/v7`, `linux/ppc64le`, and `linux/s390x`, but this project only builds and publishes `amd64` and `arm64`.

## Image tags

| Tag | Source tag |
|-----|------------|
| `ghcr.io/simplylimitless/homelab-oauth2-proxy:latest` | upstream `quay.io/oauth2-proxy/oauth2-proxy:latest` |
| `ghcr.io/simplylimitless/homelab-oauth2-proxy:<run_number>` | immutable build tag per CI run |

## Local usage

```shell
docker pull ghcr.io/simplylimitless/homelab-oauth2-proxy:latest
docker run --rm ghcr.io/simplylimitless/homelab-oauth2-proxy:latest --help
```

## Configuration

Configure oauth2-proxy at runtime via flags, env vars, or a config file mounted into the container. See the [official docs](https://oauth2-proxy.github.io/oauth2-proxy/docs/) for full details — key ones:

| Flag | Env Var | Default |
|------|---------|---------|
| `--provider` | `OAUTH2_PROXY_PROVIDER_NAME` | `github` |
| `--client-id` | `OAUTH2_PROXY_CLIENT_ID` | _(required)_ |
| `--client-secret` | `OAUTH2_PROXY_CLIENT_SECRET` | _(required)_ |
| `--redirect-url` | `OAUTH2_PROXY_REDIRECT_URL` | `https://_/.auth/oauth2/callback` |
| `--authenticated-urls` | `OAUTH2_PROXY_AUTHENTICATED_URLS` | _(none)_ |

## CI / Publishing

Pushing to `main` triggers `.github/workflows/docker.yml` which:

1. Checks out the repo
2. Sets up QEMU and Buildx for multi-arch builds
3. Logs into GHCR
4. Builds and pushes a multi-arch image (`linux/amd64`, `linux/arm64`)

No changes to this Dockerfile are needed to publish — it re-bases every push on the upstream `latest`.

## License

Same as [`oauth2-proxy`](https://github.com/oauth2-proxy/oauth2-proxy#license).
