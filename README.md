# tailscale-ubuntu

An Ubuntu-based [Tailscale](https://tailscale.com) container image, rebuilt daily from the latest Tailscale release on top of the latest `ubuntu` base image.

## Usage

```bash
docker pull ghcr.io/patrickwolf/tailscale-ubuntu:latest
```

```yaml
services:
  tailscale:
    image: ghcr.io/patrickwolf/tailscale-ubuntu:latest
```

## Tags

| Tag | Meaning |
| --- | --- |
| `latest` | Most recent build (latest Tailscale release on the latest Ubuntu base) |
| `vX.Y.Z` | Pins a specific Tailscale release (e.g. `v1.80.0`) |

The Ubuntu base and Tailscale versions used for each build are listed in the summary of the corresponding [workflow run](../../actions/workflows/build-ubuntu-tailscale.yml).

## Binaries

`tailscale` and `tailscaled` are installed in `/usr/local/bin`.