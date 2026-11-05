# Used by Option B in docker-compose.yml.
# Add the packages and tools you want baked into the image.
FROM ghcr.io/patrickwolf/tailscale-ubuntu:latest

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl git vim htop \
    && rm -rf /var/lib/apt/lists/*