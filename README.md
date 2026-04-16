# Unraid Grafana + Prometheus Stack

This repository provides a Docker Compose monitoring stack for Unraid using your Docker Hub images:
- `gnawhnehpets/unraid-prometheus`
- `gnawhnehpets/unraid-grafana`

## Included Services
- Prometheus
- Grafana
- Blackbox Exporter (optional profile)
- Node Exporter (optional profile)
- cAdvisor (optional profile)

## Quick Start
1. Copy `.env.example` to `.env` and set a strong Grafana password.
2. Ensure the Docker Hub images exist (or run the publish workflow first).
3. Start core services:
   ```bash
   docker compose up -d
   ```
4. Optionally start exporters:
   ```bash
   docker compose --profile exporters up -d
   ```

## Access
- Grafana: http://<unraid-ip>:3000
- Prometheus: http://<unraid-ip>:9090

## Add Your Service Targets
- Edit `prometheus/targets/static-services.yml` for `/metrics` endpoints.
- Edit `prometheus/targets/blackbox-targets.yml` for health/HTTP probe endpoints.

Prometheus automatically refreshes file-based targets every 30s.

## Unraid Multi-Network Note
This stack expects existing external Docker networks in Unraid:
- `monitoring` (where Grafana/Prometheus run)
- `media` (where Radarr/Sonarr/Plex run)

`prometheus` and `blackbox-exporter` are attached to both so probes can reach media services directly by container DNS names.

Minimum preconfigured probe targets:
- Radarr: `radarr:8989` and `192.168.10.164:8989`
- Sonarr: `sonarr:7878` and `192.168.10.164:7878`
- Plex: `plex:32400/identity` and `192.168.10.164:32400/identity`

## CI Workflows
- `Config Validate`: checks Compose and Prometheus config.
- `DockerHub Publish Images`: builds and pushes both images to Docker Hub.

Required GitHub secret:
- `DOCKERHUB_TOKEN` (Docker Hub access token for user `gnawhnehpets`)

## Future Dashboards
Place custom dashboard JSON files in `grafana/dashboards/`.
They are auto-loaded via Grafana provisioning.
