# cnpg-postgis-timescaledb

A [CloudNativePG](https://cloudnative-pg.io/) operand image: PostgreSQL 17 with TimescaleDB and PostGIS.

[![Build and Push](https://github.com/schr4der/cnpg-postgis-timescaledb/actions/workflows/build.yml/badge.svg)](https://github.com/schr4der/cnpg-postgis-timescaledb/actions/workflows/build.yml)
![PostgreSQL 17](https://img.shields.io/badge/PostgreSQL-17-blue)
![Registry](https://img.shields.io/badge/ghcr.io-schr4der%2Fcnpg--postgis--timescaledb-brightgreen)

## What it is

An *operand* image is the Postgres image the CNPG operator runs in each `Cluster` pod (as opposed to the operator itself). This one extends `ghcr.io/cloudnative-pg/postgresql:17-standard-bookworm` with the Debian packages `timescaledb-2-postgresql-17` and `postgresql-17-postgis-3`, and runs as UID 26 per CNPG operand conventions.

## Image

Published publicly to GitHub Container Registry — no credentials needed to pull:

```sh
docker pull ghcr.io/schr4der/cnpg-postgis-timescaledb:17
```

| Tag | Mutability | Published |
| --- | --- | --- |
| `17` | Moving | On every merge to `main` |
| `17-<git sha>` | Immutable | On every merge to `main` |
| `v*` (e.g. `v1.0.0`) | Immutable | When a git tag is pushed; also creates a GitHub Release |

## Usage with CloudNativePG

Point your `Cluster` at the image and preload TimescaleDB:

```yaml
apiVersion: postgresql.cnpg.io/v1
kind: Cluster
metadata:
  name: example
spec:
  imageName: ghcr.io/schr4der/cnpg-postgis-timescaledb:17
  postgresql:
    shared_preload_libraries:
      - timescaledb
```

`shared_preload_libraries: [timescaledb]` is required for TimescaleDB to load.

Note: a running cluster does not pick up a re-pushed moving tag (`17`); the cluster must be rolled/restarted. Pin an immutable tag (`17-<git sha>` or `v*`) to avoid this.

This image is consumed by the [ESDeploy](https://github.com/EnsembleScientific/ESDeploy) Helm chart, which already sets `shared_preload_libraries`.

## Building locally

```sh
docker build . -t ghcr.io/schr4der/cnpg-postgis-timescaledb:17
```
