# WEPP_files - Tenerife WEPP Model Input Data

This repository contains input data for running the Water Erosion Prediction Project (WEPP) model on Tenerife, Canary Islands, Spain.

## Data Source

Shared via Google Drive: https://drive.google.com/drive/folders/1fgDcUOyDrd_Wyrmxk5eqhdPuQAMQCGJx

## Directory Structure

```
WEPP_files/
├── DEM/                    # Digital Elevation Models
│   ├── MDT05_Tenerife.tif  # 5m resolution DEM (302 MB)
│   └── 136_MDT25_TF.tif    # 25m resolution DEM (37 MB)
├── climate/                # Climate and station data
│   ├── climate_files/      # CLIGEN daily climate files (.cli)
│   ├── station_par_files/  # CLIGEN parameter files (.par)
│   ├── stations.csv        # Station metadata
│   └── stations.xlsx       # Station metadata (original)
└── soil/                   # Soil data
    ├── soil_files/         # WEPP soil parameter files (.sol)
    └── soil_map/           # Soil raster maps
        ├── tf_soil_5.tif   # 5m resolution soil map
        ├── tf_soil_10.tif  # 10m resolution soil map
        └── tf_soil_25.tif  # 25m resolution soil map
```

## WEPPcloud/wepppy Integration

### DEM Hosting

DEMs are hosted on: `wepp1.tail305ec9.ts.net` (wmesque2)

### Configuration Files

- **tenerife-disturbed.cfg** - 25m resolution configuration
- **tenerife-5m-disturbed.cfg** - 5m resolution configuration

### Coordinate Reference System

- **DEM**: ETRS89 / UTM zone 28N (EPSG:25828)
- **Soil Maps**: WGS 84 / UTM zone 28N (EPSG:32628)

## File Counts

| Type | Count | Description |
|------|-------|-------------|
| .cli | 62 | CLIGEN daily climate files (30-year simulations) |
| .par | 62 | CLIGEN station parameter files |
| .sol | 43 | WEPP soil parameter files |
| .tif | 5 | GeoTIFF rasters (2 DEM + 3 soil maps) |

## Data Attribution

### DEM Sources

- **5m DEM**: GRAFCAN (2022). Modelo Digital de Elevaciones (MDT) 5x5m de la isla de Tenerife. IDECanarias.
- **25m DEM**: GRAFCAN (2021). Modelo Digital de Elevaciones (MDT) 25x25m. Gobierno de Canarias.

### Climate Data

- **Source**: Cabildo de Tenerife Agroclimatic Network
- **URL**: https://www.agrocabildo.org/agrometeorologia_estaciones.asp
- **Stations**: 62 meteorological stations across Tenerife
- **Period**: 1996-2025 (up to 30 years of observations)

### Soil Data

- **Soil Map**: Tejedor, M., Jimenez, C., Neris, J. (2026). Estudio de los recursos edaficos de Canarias. Gobierno de Canarias.
- **Soil Profiles**: Guerra Garcia, J.A. (2009). PhD Thesis, Universidad de La Laguna.

## Usage with wepppy

```python
from wepppy.nodb import NoDbProject

# Load Tenerife 25m configuration
project = NoDbProject.load_config('tenerife-disturbed')

# Load Tenerife 5m configuration
project = NoDbProject.load_config('tenerife-5m-disturbed')
```

## License

Data provided for research and educational purposes. See individual source attributions for specific licensing terms.
