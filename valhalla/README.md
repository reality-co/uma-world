# uma-world Valhalla map services

Split by region because loading the entire planet in a single instance is hard

Process for deployment:

- pick a region central to the continent, list the available regions using `fly platform regions`
- c/v the fly.toml file and edit the app, mount and tileset name
- pick the right images in the `tile_urls` env var, full list on [geofabrik.de](http://download.geofabrik.de/index.html)
- create the volume in the right region: `fly vol create uma_world_valhalla_CONTINENT --no-encryption -r REGION`
- scale volume to fit the geo data: `fly vol extend -s 500`
- `fly launch` (or `fly deploy` for subsequent deploys)
- scale instance cpu for faster computation of tiles: `fly scale vm shared-cpu-4x`
- give it enough ram: `fly scale memory 2048`
- then set up certificates and DNS CNAME record on Cloudflare
