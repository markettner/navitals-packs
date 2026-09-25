# Navitals routing packs

Prebuilt [Valhalla](https://valhalla.github.io/valhalla/) routing tiles for the Navitals iOS app,
so a route can be planned with no signal at all.

This repository is public because the app downloads these files without credentials. It holds no
application source: only `index.json` and the tile archives attached to its releases.

## Using them

`index.json` is what the app fetches. Each entry describes one region and where its archive lives:

```json
{
  "packs": [
    {
      "id": "de-brandenburg",
      "name": "Brandenburg & Berlin",
      "bbox": [11.26, 51.36, 14.77, 53.56],
      "sizeBytes": 370421760,
      "hasElevation": false,
      "builtAt": "2026-09-17T16:01:50.931355Z",
      "archiveURL": "https://github.com/markettner/navitals-packs/releases/download/packs/de-brandenburg.tar.gz",
      "archiveBytes": 126004234,
      "archiveSHA256": "…",
      "brouterBytes": 44800000
    }
  ],
  "overlay": {"version": "2026-10-01", "bounds": [-31.27, 32.63, 46.39, 71.19]}
}
```

`bbox` is `[west, south, east, north]`. The app only uses a pack when it covers *every* waypoint of
a route, since routing off the edge of the tiles leads into a dead end.

## How it is built

Rebuilt by the workflows in `.github/workflows`, from scripts in the (private) app repository:

- **Monthly** (`monthly.yml`): every region's pack from current Geofabrik extracts, replaced in
  place on the `packs` release as `<id>.tar.gz`, plus the BRouter tile mirror on
  [navitals-brouter](https://github.com/markettner/navitals-brouter). The app offers an update when
  an archive's checksum changes.
- **Quarterly** (`overlays.yml`): the cycle and hiking route overlay for Europe, as a new release
  `overlays-<date>`. `index.json`'s `overlay` entry names the current one.

Packs are built with pyvalhalla **3.7.x**. The graph tile header changed in 3.8, and a pack built
with it will not load in the app's Valhalla 3.6.3 runtime. The releases here are kept under 5 GB in
total, so there is only ever one generation of packs and two of the overlay.

## Attribution and licence

The tiles are derived from [OpenStreetMap](https://www.openstreetmap.org/copyright) data,
© OpenStreetMap contributors, and are distributed under the
[Open Database License](https://opendatacommons.org/licenses/odbl/). Any use of them carries the
same attribution requirement.
