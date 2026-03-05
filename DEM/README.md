# DEM - Digital Elevation Models

Digital Elevation Models for Tenerife, Canary Islands.

## Files

| File | Resolution | Size | Dimensions | CRS |
|------|------------|------|------------|-----|
| MDT05_Tenerife.tif | 5m | 302 MB | 19741 x 15101 px | ETRS89 / UTM 28N |
| 136_MDT25_TF.tif | 25m | 37 MB | - | ETRS89 / UTM 28N |

## Auxiliary Files

- `*.tfw` - World files (georeferencing)
- `*.ovr` - Overview pyramids for fast rendering
- `*.aux.xml` - GDAL auxiliary metadata
- `*.xml` - Extended metadata

## Technical Specifications

### MDT05_Tenerife.tif (5m DEM)

```
Driver: GeoTIFF
Size: 19741 x 15101 pixels
Pixel Size: 5m x 5m
CRS: ETRS89 / UTM zone 28N (EPSG:25828)
Data Type: Float32
NoData Value: -3.4028235e+38
```

### 136_MDT25_TF.tif (25m DEM)

```
Driver: GeoTIFF
Size: 3171 x 2593 pixels (approx)
Pixel Size: 25m x 25m
CRS: ETRS89 / UTM zone 28N (EPSG:25828)
```

## wepppy Usage

DEMs are used by TOPAZ for watershed delineation and slope/aspect calculation.

```python
# In wepppy config:
[general]
dem_db = "tenerife/MDT05_Tenerife"  # 5m
# or
dem_db = "tenerife/136_MDT25_TF"    # 25m
```

## Hosting

Hosted on: `wepp1.tail305ec9.ts.net` (wmesque2)

## Source

- **5m**: GRAFCAN (2022). Modelo Digital de Elevaciones (MDT) 5x5m. IDECanarias. https://visor.grafcan.es/
- **25m**: GRAFCAN (2021). MDT 25x25m. https://opendata.sitcan.es/
