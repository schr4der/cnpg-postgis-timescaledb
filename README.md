# cnpg-postgis-timescaledb

CloudNativePG operand image: PostgreSQL 17 with TimescaleDB and PostGIS.
Referenced by the ESDeploy chart (`charts/esci/values.yaml` → cnpg image).

The cluster spec must set `shared_preload_libraries: [timescaledb]` (already
set in the ESDeploy chart). Running clusters don't pick up a re-pushed moving
tag — roll the cluster via ESDeploy to apply changes.

## Building

GitHub Actions pushes `cnpg-timescaledb:17` and `:17-<sha>` to the
DigitalOcean registry on merge to main. Requires the
`DIGITALOCEAN_ACCESS_TOKEN` secret and `REGISTRY_NAME` variable (`esci-cr`).

Local:

```bash
docker build . -t registry.digitalocean.com/esci-cr/cnpg-timescaledb:17
docker push registry.digitalocean.com/esci-cr/cnpg-timescaledb:17
```
