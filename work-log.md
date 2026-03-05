# WEPP_files Work Log

## 2026-03-04: Initial Setup and Normalization

### Source Data

Synced from Google Drive shared folder:
https://drive.google.com/drive/folders/1fgDcUOyDrd_Wyrmxk5eqhdPuQAMQCGJx

**Sync Statistics:**
- Total Size: 445 MB downloaded
- Total Files: 227
- Total Directories: 12

### Directory Normalization

Renamed hidden dot-directories to standard names for cross-platform compatibility:

| Original | New Name | Description |
|----------|----------|-------------|
| `climate/.cli/` | `climate/climate_files/` | CLIGEN daily climate files |
| `climate/.par/` | `climate/station_par_files/` | CLIGEN parameter files |
| `soil/.sol/` | `soil/soil_files/` | WEPP soil parameter files |

### File Format Conversions

#### Soil Maps: ESRI ADF to GeoTIFF

Converted ESRI Arc/Info Grid (ADF) format to GeoTIFF for broader compatibility:

```bash
gdal_translate -of GTiff tf_soil_5 tf_soil_5.tif
gdal_translate -of GTiff tf_soil_10 tf_soil_10.tif
gdal_translate -of GTiff tf_soil_25 tf_soil_25.tif
```

**Output Sizes:**
| File | Resolution | Size |
|------|------------|------|
| tf_soil_5.tif | 5m | 206 MB |
| tf_soil_10.tif | 10m | 51 MB |
| tf_soil_25.tif | 25m | 8 MB |

#### Station Metadata: XLSX to CSV

```python
import pandas as pd
df = pd.read_excel('stations.xlsx')
df.to_csv('stations.csv', index=False)
```

- 62 stations converted
- Original XLSX retained for reference

### Documentation Created

Created comprehensive README.md files:

1. **Root README.md** - Overview, directory structure, file counts, data attribution
2. **DEM/README.md** - DEM specifications, technical details, source citations
3. **climate/README.md** - Station metadata schema, .cli/.par file formats
4. **soil/README.md** - Soil map specifications, .sol file format

### wepppy Integration

#### Files Copied to wepppy Locales

```
/Users/roger/src/wepppy/wepppy/locales/tenerife/soils/
├── tf_soil_5.tif
├── tf_soil_10.tif
├── tf_soil_25.tif
└── db/
    └── *.sol (43 soil files)
```

#### Config Files Modified/Created

**Updated: `tenerife-disturbed.cfg`**
- Changed `dem_db` from `eu/eu-dem-v1.1` to `tenerife/136_MDT25_TF`
- Updated `soils_map` to `LOCALES_DIR/tenerife/soils/tf_soil_25.tif`
- Added `[wmesque]` section with endpoint for wepp1.tail305ec9.ts.net

**Created: `tenerife-5m-disturbed.cfg`**
- New 5m resolution configuration
- `dem_db = "tenerife/MDT05_Tenerife"`
- `soils_map = "LOCALES_DIR/tenerife/soils/tf_soil_5.tif"`
- `cellsize = 5`
- Adjusted TOPAZ parameters: `mcl=40`, `csa=4`, `zoom_min=13`

### DEM Hosting

DEMs to be hosted on: `wepp.cloud` (wmesque2 service)

Expected paths on server:
- `/geodata/tenerife/MDT05_Tenerife.tif` (5m DEM)
- `/geodata/tenerife/136_MDT25_TF.tif` (25m DEM)

### Pending Tasks

1. Upload DEMs to wepp1.tail305ec9.ts.net geodata directory
2. Verify wmesque2 catalog includes tenerife datasets
3. Test config files with wepppy project creation
4. Sync soil files to production server if needed

---

## File Inventory After Modifications

```
WEPP_files/
├── README.md                    [NEW]
├── work-log.md                  [NEW]
├── DEM/
│   ├── README.md                [NEW]
│   ├── readme.txt               [ORIGINAL]
│   ├── 136_MDT25_TF.tif
│   ├── 136_MDT25_TF.tfw
│   ├── MDT05_Tenerife.tif
│   ├── MDT05_Tenerife.tfw
│   └── [auxiliary files]
├── climate/
│   ├── README.md                [NEW]
│   ├── readme.txt               [ORIGINAL]
│   ├── stations.csv             [NEW - converted from xlsx]
│   ├── stations.xlsx            [ORIGINAL]
│   ├── climate_files/           [RENAMED from .cli]
│   │   └── *.cli (62 files)
│   └── station_par_files/       [RENAMED from .par]
│       └── *.par (62 files)
└── soil/
    ├── README.md                [NEW]
    ├── readme.txt               [ORIGINAL]
    ├── soil_files/              [RENAMED from .sol]
    │   └── *.sol (43 files)
    └── soil_map/
        ├── tf_soil_5.tif        [NEW - converted from ADF]
        ├── tf_soil_10.tif       [NEW - converted from ADF]
        ├── tf_soil_25.tif       [NEW - converted from ADF]
        ├── tf_soil_5/           [ORIGINAL ADF directory]
        ├── tf_soil_10/          [ORIGINAL ADF directory]
        ├── tf_soil_25/          [ORIGINAL ADF directory]
        └── info/                [ORIGINAL ADF metadata]
```
