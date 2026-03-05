# Soil Data

WEPP soil parameter files and raster maps for Tenerife.

## Directory Structure

```
soil/
├── soil_files/          # WEPP soil parameter files (.sol)
├── soil_map/            # Soil raster maps (GeoTIFF)
│   ├── tf_soil_5.tif    # 5m resolution
│   ├── tf_soil_10.tif   # 10m resolution
│   └── tf_soil_25.tif   # 25m resolution
└── readme.txt           # Original source notes
```

## Soil Maps

| File | Resolution | Size | Dimensions | CRS |
|------|------------|------|------------|-----|
| tf_soil_5.tif | 5m | 206 MB | 15857 x 12967 px | WGS84 / UTM 28N |
| tf_soil_10.tif | 10m | 51 MB | 7928 x 6484 px | WGS84 / UTM 28N |
| tf_soil_25.tif | 25m | 8 MB | 3171 x 2593 px | WGS84 / UTM 28N |

### Raster Values

Pixel values correspond to soil_id in the .sol files (1-46).

**Special Values:**
- `20` = Rocks (no soil)
- `21` = Continuous urban fabric
- `19` = Not used (Rhodustalfs - no data available)

## Soil Parameter Files (.sol)

43 WEPP soil parameter files (IDs 1-46, excluding 19, 20, 21).

### File Format (WEPP 7778 format)

```
7778
# Comment: soil file built from profile [name]
# Author: [author]
# SoilType: [USDA classification]
Any comments:
1 0
'Profile Name' 'Texture' NumLayers Albedo Sat InitSat Interrill Rill CritShear
    Depth(mm) BulkDens(g/cc) Ksat(mm/hr) Ki Kb Sm Pct-Sand Pct-Clay CEC Pct-Rock Pct-ite
    [layer 2...]
1 -1 20000.000000 0.0036
255 255 255
```

### Soil File Columns

| Parameter | Units | Description |
|-----------|-------|-------------|
| Depth | mm | Layer depth from surface |
| BulkDens | g/cm3 | Bulk density |
| Ksat | mm/hr | Saturated hydraulic conductivity |
| Ki | - | Interrill erodibility |
| Kb | - | Baseline interrill erodibility |
| Sm | - | Soil moisture parameter |
| Pct-Sand | % | Sand fraction |
| Pct-Clay | % | Clay fraction |
| CEC | meq/100g | Cation exchange capacity |
| Pct-Rock | % | Rock fragment content |
| Pct-ite | % |Ite mineral content |

### Soil Types Present

Based on USDA Soil Taxonomy and WRB classifications:

- Calcitorrerts / Calcisol leptico vertico
- Haplustands / Andosol vitrico
- Vitritorrands / Andosol vitrico (esqueletico)
- Udivitrands / Andosol vitrico fulvico
- Hapludands / Andosol silico-andico
- Haplustepts / Cambisol eutrico
- And others...

## wepppy Integration

```python
# In .cfg file:
[soils]
wepp_chn_type = "default"
soils_map = "/path/to/soil_map/tf_soil_25.tif"  # or tf_soil_5.tif
ssurgo_db = "None"
```

The soil map raster values link to corresponding .sol files:
- Pixel value `1` -> `soil_files/1.sol`
- Pixel value `2` -> `soil_files/2.sol`
- etc.

## Sources

**Soil Map (1:25,000)**
Tejedor, M., Jimenez, C., Neris, J. (2026). Estudio de los recursos edaficos de Canarias. Gobierno de Canarias (Spain).

**Soil Profiles**
Guerra Garcia, J.A. (2009). Evaluacion de la degradacion de los suelos naturales de la isla de Tenerife: Secuencias edaficas evolutivas y regresivas. PhD Thesis, Universidad de La Laguna. 464 pp.
