# tailscale-ubuntu

An Ubuntu-based [Tailscale](https://tailscale.com) container image, built with Tailscale's official `build_docker.sh` (same `tailscale`, `tailscaled` and `containerboot` as the official image) on top of the latest `ubuntu` base. It is checked daily and rebuilt only when a new Tailscale release or a new Ubuntu base image is available.

```bash
docker pull ghcr.io/patrickwolf/tailscale-ubuntu:latest
```

## Tags

| Tag | Meaning |
| --- | --- |
| `latest` | Most recent build |
| `vX.Y.Z` | Pins a Tailscale release (e.g. `v1.104.5`) |

Each image carries OCI annotations with the Tailscale version and the Ubuntu base digest it was built from. The exact Ubuntu version is shown in each [workflow run](../../actions/workflows/build-ubuntu-tailscale.yml) summary.

## Examples

Each example folder is self-contained (compose file, `Dockerfile`, `.env.example`), so you can copy just one. Both run a single container as `root`, keep the Tailscale state (`/var/lib/tailscale`) and the home directory (`/root`) in named volumes, and include Tailscale SSH (`TS_EXTRA_ARGS=--ssh`; remove it to disable, and add an `ssh` rule to your tailnet policy).

| Example | Use when |
| --- | --- |
| [`examples/userspace`](examples/userspace) | You need to be reachable over Tailscale, but scripts in the container don't need to connect to other tailnet devices. No extra privileges. |
| [`examples/kernel`](examples/kernel) | Programs in the container must reach tailnet devices directly, or it acts as an exit node/subnet router. Needs `/dev/net/tun` and `NET_ADMIN`. |

```bash
cd examples/userspace        # or examples/kernel
cp .env.example .env         # put your auth key (https://login.tailscale.com/admin/settings/keys) in .env
docker compose up -d
docker exec -it tailscale-ubuntu bash
```

The container appears as `tailscale-ubuntu` in your [Machines](https://login.tailscale.com/admin/machines) list. The key stays in `.env` (git-ignored).

### Adding your own software

Packages installed with `apt` in a running container are lost when it is recreated. Edit the `Dockerfile` in your chosen example folder, then in its compose file comment out the `image:` line (Option A) and uncomment the `build:` and `image: my-tailscale-ubuntu` lines (Option B). Run `docker compose up -d --build`.