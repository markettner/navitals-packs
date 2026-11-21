# Navitals regions

The index of offline regions for the Navitals iOS app, so a route can be planned and found with no
signal at all.

This repository is public because the app downloads these files without credentials. It holds no
application source: only `index.json`, and the route overlay attached to its releases.

## Using them

`index.json` is what the app fetches. Each entry describes one region and where its parts live:

```json
{
  "packs": [
    {
      "id": "de-brandenburg",
      "name": "Berlin & Brandenburg",
      "bbox": [11.26, 51.36, 14.77, 53.56],
      "builtAt": "2026-12-01T08:39:09Z",
      "brouterBytes": 44800000,
      "searchURL": "https://github.com/markettner/navitals-search/releases/download/search/de-brandenburg.search.sqlite.gz",
      "searchBytes": 30000000,
      "searchSizeBytes": 80000000,
      "searchSHA256": "…",
      "mapURL": "https://github.com/markettner/navitals-maps/releases/download/maps/de-brandenburg.map.pmtiles",
      "mapBytes": 60000000,
      "mapSHA256": "…"
    }
  ],
  "overlay": {"version": "2026-10-01", "bounds": [-31.27, 32.63, 46.39, 71.19]}
}
```

`bbox` is `[west, south, east, north]`. A region's route data is its cells of the BRouter tiles on
[navitals-brouter-europe](https://github.com/markettner/navitals-brouter-europe), measured in `brouterBytes`;
its offline search index is on [navitals-search](https://github.com/markettner/navitals-search),
its basic map and route overlay cut on [navitals-maps](https://github.com/markettner/navitals-maps).

Until November 2026 every region also carried Valhalla routing tiles, attached here to the `packs`
release. The app no longer uses them, and they are gone, with the `sizeBytes` and `hasElevation`
fields that described them.

## How it is built

Rebuilt by the workflows in `.github/workflows`, from scripts in the (private) app repository:

- **Monthly** (`monthly.yml`): the BRouter tile mirror, then every region's search index from
  current Geofabrik extracts and its basic map, each replaced in place, and the regions in
  `index.json` rewritten. The app offers an update when a part's checksum changes.
- **Quarterly** (`overlays.yml`): the cycle and hiking route overlay for Europe, as a new release
  `overlays-<date>`. `index.json`'s `overlay` entry names the current one.
- **Monthly, on the 3rd** (`water.yml`): water tiles for the paddling profile, BRouter tiles of
  Europe's water ways, weirs, locks and mapped portages, replaced in place on the `tiles` release
  of [navitals-water](https://github.com/markettner/navitals-water), which lists them in
  `tiles.json`.

The releases here are kept under 5 GB in total, so there are only ever two generations of the
overlay.

## Attribution and licence

The data is derived from [OpenStreetMap](https://www.openstreetmap.org/copyright),
© OpenStreetMap contributors, and is distributed under the
[Open Database License](https://opendatacommons.org/licenses/odbl/). Any use of it carries the
same attribution requirement.
