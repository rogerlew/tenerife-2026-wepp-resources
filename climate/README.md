# Climate Data

CLIGEN climate data for 62 meteorological stations across Tenerife.

## Directory Structure

```
climate/
├── climate_files/       # Daily climate files (.cli)
├── station_par_files/   # Station parameter files (.par)
├── stations.csv         # Station metadata (CSV)
├── stations.xlsx        # Station metadata (Excel)
└── readme.txt           # Original source notes
```

## Station Metadata (stations.csv)

| Column | Description |
|--------|-------------|
| id_weatherstation | Unique station identifier |
| name | Station name (matches .cli/.par filenames) |
| date_install | Installation date |
| last_data | Most recent data date |
| Years | Years of observation |
| municipality_id | Municipality code |
| municipality_name | Municipality name |
| place | Specific location |
| latitude | Decimal degrees (WGS84) |
| longitude | Decimal degrees (WGS84) |
| station_type | A, B, C, D, or E (see types below) |
| x_cords | UTM X (zone 28N) |
| y_cords | UTM Y (zone 28N) |
| altitude | Elevation in meters |
| datalogger_type | Equipment type |
| sensors_count | Number of sensors |

## Station Types

| Type | Sensors |
|------|---------|
| A | Temperature, precipitation, humidity, radiation, wind, evaporation (indoor/outdoor) |
| B | Temperature, precipitation, humidity, radiation, wind, evaporation, leaf wetness |
| C | Temperature, precipitation, humidity, radiation, wind, leaf wetness |
| D | Temperature, precipitation, humidity, radiation, wind direction |
| E | Temperature, precipitation, humidity, radiation, wind speed |

## Climate Files (.cli)

CLIGEN version 4.30 daily climate simulation files.

### File Format

```
4.30                                    # CLIGEN version
   1   0   0                            # Flags
   Station:  OROTAV01                   # Station name
 Latitude Longitude Elevation Obs.Years Beginning Year Simulated
    28.41   -16.51     214       30        1996           30
 Observed monthly ave max temperature (C)
   20.7  20.4  20.8  21.1  21.9  23.2  24.2  25.3  25.7  25.2  23.2  22.0
 Observed monthly ave min temperature (C)
   [12 monthly values...]
 Observed monthly ave solar radiation (Langleys/day)
   [12 monthly values...]
 Observed monthly ave precipitation (mm)
   [12 monthly values...]
 da mo year  prcp  dur   tp     ip  tmax  tmin  rad  w-vl w-dir  tdew
             (mm)  (h)               (C)   (C) (l/d) (m/s)(Deg)   (C)
 17  8 1996   0.0  0.00  0.00   0.00  24.0  19.7    18   0.3   347  17.4
 [daily records...]
```

### Daily Record Columns

| Column | Units | Description |
|--------|-------|-------------|
| da | - | Day of month |
| mo | - | Month |
| year | - | Year |
| prcp | mm | Precipitation |
| dur | hours | Storm duration |
| tp | - | Time to peak (fraction) |
| ip | - | Peak intensity ratio |
| tmax | C | Maximum temperature |
| tmin | C | Minimum temperature |
| rad | Langleys/day | Solar radiation |
| w-vl | m/s | Wind velocity |
| w-dir | degrees | Wind direction |
| tdew | C | Dew point temperature |

## Parameter Files (.par)

CLIGEN station parameter files containing statistical distributions.

### File Naming

```
Estacion_{id}_{NAME}.par
```

### Key Parameters

- Monthly precipitation statistics (mean, std dev, skew)
- Wet/dry day transition probabilities P(W/W), P(W/D)
- Temperature statistics (TMAX, TMIN, std devs)
- Solar radiation (mean and std dev)
- Wind direction frequency and speed by 16 compass directions
- Dew point temperatures

## wepppy Integration

```python
# Climate configuration in .cfg file:
[climate]
cligen_db = "ghcn_stations.db"
observed_clis_wc = "/path/to/climate_files/*.cli"
```

## Source

Cabildo de Tenerife Agroclimatic Network
https://www.agrocabildo.org/agrometeorologia_estaciones.asp
