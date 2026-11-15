# Reproduce the release from the bundled archived extracts

The `inputs` directory contains the exact daily CSV aggregates used in this release and
the pinned Natural Earth polygons. These files let you rebuild the monthly artifacts without
re-fetching changing source partitions. The validation reports identify the release by its
dataset ID. Install the pinned Python dependencies before running these commands.

```sh
python -m pip install -r requirements.txt
python add_places.py --boundaries inputs/natural-earth.geojson --out inputs/geography.json
python build_pyroscale_data.py --daily inputs/terra-and-suomi --daily inputs/noaa20 --geography inputs/geography.json --start 2001-01 --end 2026-10
python forecast_pyroscale.py
python validate_release.py
```

Start the explorer with `python -m http.server 8765 --bind 127.0.0.1` and visit
[PyroScale locally](http://127.0.0.1:8765/). The core globe can load without an external
map or data service. Optional context layers require an internet connection.
