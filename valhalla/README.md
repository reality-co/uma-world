# uma-world Valhalla map services

Split by region because loading the entire planet in a single instance is hard

Process for deployment:

- pick a region central to the continent, list the available regions using `fly platform regions`
- c/v the fly.toml file and edit the app, mount and tileset name
- pick the right images in the `tile_urls` env var, full list on [geofabrik.de](http://download.geofabrik.de/index.html)
- create the volume in the right region: `fly vol create uma_world_valhalla_CONTINENT --no-encryption -r REGION`
- scale volume to fit the geo data: `fly vol extend -s 000`
- `fly deploy`
- scale instance cpu for faster computation of tiles: `fly scale vm shared-cpu-0x`
- give it enough ram: `fly scale memory 0000`
- allocate an IP: `fly ips allocate-v6` (no need for IPv4)
- then set up certificates on Fly dashboard directly and DNS CNAME record on Cloudflare


NAME | REGIONS | CPU | RAM | DISK
---- | ------- | --- | --- | ----
europe.untitledmap.app | Europe | shared-4x | 4096 | 500
america.untitledmap.app | North, Central and South America | shared-4x | 2048 | 230
