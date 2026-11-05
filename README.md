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

## Docker Compose example with authentication

A ready-to-use example based on [Tailscale's official Docker Compose guide](https://tailscale.com/kb/1282/docker) is in [`examples/`](examples/):

1. Generate an [auth key](https://login.tailscale.com/admin/settings/keys) in the Tailscale admin console.
2. Copy the env file and add your key:
   ```bash
   cd examples
   cp .env.example .env   # then edit TS_AUTHKEY
   ```
3. Start it:
   ```bash
   docker compose up -d
   ```
4. The container appears as `tailscale-ubuntu` in your [Machines](https://login.tailscale.com/admin/machines) list. Open a shell with `docker exec -it tailscale-ubuntu bash`.

The auth key stays in `.env` (git-ignored) and node state persists in `./tailscale-state`, so the container stays logged in across restarts.