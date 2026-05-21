# ASTER – Apple Screen Time ExploreR

ASTER is an interactive tool for extracting and analysing iOS/macOS screen time data from Apple biome exports. It parses raw `App.InFocus` biome files, correlates them with device metadata from `sync.db`, and produces clean, per-device tables that can be exported as CSV, JSON, or Parquet.

## What it does

- Parses compressed `App.InFocus` biome folders (SEGB v1/v2 format with protobuf payloads)
- Queries a `sync.db` Apple device database to resolve device identifiers to human-readable names and platforms
- Cleans and deduplicates app open/close events per device
- Presents enriched, per-device screen time data in an interactive table with download options

## Input files

| File | Description |
|------|-------------|
| `sync.db` | Apple device peer database — found on a Mac at `~/Library/Application Support/com.apple.syncedpreferences/` |
| `App.InFocus.zip` | Zip of the `App.InFocus` biome folder — found on a Mac at `~/Library/Biome/streams/` (or extracted from an iPhone backup) |

## Workflow

1. **Connect to sync.db** — upload the file and run the device query to load a list of known devices
2. **Parse App.InFocus zip** — upload the zip and click "Parse" to extract screen time events
3. **Review results** — per-device tables are shown in an accordion; download each as CSV, JSON, or Parquet
4. **Clean up** — click "Delete Temporary File" to remove the temp copy of `sync.db` from the server

## Output columns

`timestamp`, `open_close`, `app_bundle`, `app_version`, `last_sync_date`, `device_identifier`, `device_name`

---

## Docker

Use the provided `Dockerfile` to build an image that already has the Python dependencies installed and can run the app with `marimo run app.py`.

1. Build the image:

	```bash
	docker build -t aster .
	```

2. Run the app (on port 8080):

	```bash
	docker run --rm -p 8080:8080 aster
	```

3. Open [http://localhost:8080](http://localhost:8080) in your browser and upload the `sync.db` and zipped `App.InFocus` folder as described in the UI.

If you need to work with files outside the container, mount a host directory and point the UI file picker to it.
