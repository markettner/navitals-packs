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
      "downloadURL": "https://github.com/markettner/navitals-packs/releases/download/packs-2026-09/de-brandenburg.tar"
    }
  ]
}
```

`bbox` is `[west, south, east, north]`. The app only uses a pack when it covers *every* waypoint of
a route, since routing off the edge of the tiles leads into a dead end.

## Building a pack

From the app repository:

```bash
tools/packs/build_pack.sh de-brandenburg brandenburg-latest.osm.pbf "Brandenburg & Berlin" 11.26,51.36,14.77,53.56
tools/packs/build_index.py --base-url <release download URL> tools/packs/.work/out/*/pack.json > index.json
```

Build with pyvalhalla **3.7.x**. The graph tile header changed in 3.8, and a pack built with it will
not load in the app's Valhalla 3.6.3 runtime.

## Attribution and licence

The tiles are derived from [OpenStreetMap](https://www.openstreetmap.org/copyright) data,
© OpenStreetMap contributors, and are distributed under the
[Open Database License](https://opendatacommons.org/licenses/odbl/). Any use of them carries the
same attribution requirement.
