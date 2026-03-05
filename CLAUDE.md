# CLAUDE.md - Project Context for AI Assistants

This document provides context for AI assistants working on this repository.

## Project Overview

This repository contains WEPP (Water Erosion Prediction Project) model input data for Tenerife, Canary Islands, Spain. It was created to support WEPPcloud modeling workflows.

## Repository Structure

```
tenerife-2026-wepp-resources/
├── main branch     # Normalized, documented resources
├── raw branch      # Original Google Drive sync (preserved for reference)
└── Git LFS         # Binary files (*.tif, *.adf, *.ovr, *.xlsx, *.tfw)
```

## Data Source

- **Google Drive**: https://drive.google.com/drive/folders/1fgDcUOyDrd_Wyrmxk5eqhdPuQAMQCGJx
- **Synced via**: `rclone sync "gdrive,shared_with_me:WEPP_files"`

## Related wepppy Integration

### Configuration Files

Located in `/Users/roger/src/wepppy/wepppy/nodb/configs/`:

- `tenerife-disturbed.cfg` - 25m resolution (uses 136_MDT25_TF DEM)
- `tenerife-5m-disturbed.cfg` - 5m resolution (uses MDT05_Tenerife DEM)

### Key Config Settings

```toml
[general]
dem_db = "tenerife/MDT05_Tenerife"  # or "tenerife/136_MDT25_TF"
locales = ["tenerife", "eu"]

[soils]
soils_map = "LOCALES_DIR/tenerife/soils/tf_soil_5.tif"  # or tf_soil_25.tif

[wmesque]
version = 2
endpoint = "https://wepp1.tail305ec9.ts.net/webservices/wmesque2/"
```

### Soil Files in wepppy Locales

Copied to `/Users/roger/src/wepppy/wepppy/locales/tenerife/soils/`:
- `tf_soil_5.tif`, `tf_soil_10.tif`, `tf_soil_25.tif`
- `db/*.sol` (43 soil parameter files)

## DEM Hosting on wepp1

DEMs are hosted on `wepp1.tail305ec9.ts.net` via wmesque2 service:

```
/geodata/tenerife/
├── MDT05_Tenerife/
│   ├── .vrt
│   └── MDT05_Tenerife.tif
└── 136_MDT25_TF/
    ├── .vrt
    └── 136_MDT25_TF.tif
```

### Catalog Entry

`/geodata/catalog` on wepp1 includes:
```
/geodata/tenerife/MDT05_Tenerife
/geodata/tenerife/136_MDT25_TF
```

### Verify DEM Access

```bash
curl "https://wepp.cloud/webservices/wmesque2/catalog"
curl -o test.tif "https://wepp.cloud/webservices/wmesque2/retrieve/tenerife/MDT05_Tenerife?bbox=-16.6,28.2,-16.5,28.3&cellsize=100"
```

## Normalizations Applied (raw → main)

| Original | Normalized | Reason |
|----------|------------|--------|
| `.cli/` | `climate_files/` | Hidden dirs cause issues on some platforms |
| `.par/` | `station_par_files/` | Descriptive naming |
| `.sol/` | `soil_files/` | Descriptive naming |
| ESRI ADF grids | GeoTIFF (`.tif`) | Broader compatibility |
| `stations.xlsx` | `stations.csv` | Added CSV version |

## Data Attribution

- **DEM**: GRAFCAN IDECanarias (5m: 2022, 25m: 2021)
- **Climate**: Cabildo de Tenerife Agroclimatic Network (62 stations, 1996-2025)
- **Soil Map**: Tejedor, Jimenez, Neris (2026) - Gobierno de Canarias
- **Soil Profiles**: Guerra Garcia (2009) - PhD Thesis, Universidad de La Laguna

## Common Tasks

### Re-sync from Google Drive

```bash
rclone sync "gdrive,shared_with_me:WEPP_files" /path/to/dest --drive-shared-with-me -P
```

### Convert ESRI ADF to GeoTIFF

```bash
gdal_translate -of GTiff tf_soil_5 tf_soil_5.tif
```

### Create VRT for wmesque2

```bash
# wmesque2 expects: /geodata/{dataset}/.vrt
mkdir -p /geodata/tenerife/NewDEM
gdalbuildvrt /geodata/tenerife/NewDEM/.vrt /geodata/tenerife/NewDEM/file.tif
```

## Coordinate Reference Systems

- **DEMs**: ETRS89 / UTM zone 28N (EPSG:25828)
- **Soil Maps**: WGS 84 / UTM zone 28N (EPSG:32628)

## File Counts

| Type | Count | Description |
|------|-------|-------------|
| `.cli` | 62 | CLIGEN daily climate files |
| `.par` | 62 | CLIGEN station parameter files |
| `.sol` | 43 | WEPP soil parameter files |
| `.tif` | 5 | GeoTIFF rasters (2 DEM + 3 soil) |

## Session History

- **2026-03-04**: Initial sync, normalization, repo creation, wepppy integration, wepp1 DEM hosting
