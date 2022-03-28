# Pronóstico para una latitud, longitud y fecha determinada

En este ejemplo se describe cómo obtener el pronóstico de una variable específica del dataset (temperatura a 2 m por ejemplo) en una latitud, longitud y fecha dadas. 


```python
import xarray as xr
import h5netcdf
import datetime
import s3fs
import cartopy.crs as ccrs
```

Se define la fecha de inicialización del pronóstico, y la validez y latitud y longitud a consultar.


```python
latitud = -25
longitud = -70

año_ini = 2022
mes_ini = 3
dia_ini = 21
hora_ini = 0

año_fcst = 2022
mes_fcst = 3
dia_fcst = 22
hora_fcst = 17
```

Se define la variable a consultar


```python
var = 'T2'
```

Se lee el archivo que posee el dato buscado


```python
FECHA_INI = datetime.datetime(año_ini, mes_ini, dia_ini, hora_ini)
FECHA_FCST = datetime.datetime(año_fcst, mes_fcst, dia_fcst, hora_fcst)

#Plazo de pronóstico
plazo = int((FECHA_FCST - FECHA_INI).total_seconds()/3600)

s3_file = f'smn-ar-wrf/DATA/WRF/DET/{FECHA_INI:%Y/%m/%d/%H}/WRFDETAR_01H_{FECHA_INI:%Y%m%d_%H}_{plazo:03d}.nc'

fs = s3fs.S3FileSystem(anon=True)

if fs.exists(s3_file):
    f = fs.open(s3_file)
    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
else:
    print('El archivo buscado no existe')

```

Se obtiene el valor pronosticado


```python
#Se busca la ubicacion del punto mas cercano a la latitud y longitud solicitada
data_crs = ccrs.LambertConformal(central_longitude = ds.CEN_LON, 
                                 central_latitude = ds.CEN_LAT, 
                                 standard_parallels = (ds.TRUELAT1, ds.TRUELAT2))
x, y = data_crs.transform_point(longitud, latitud, src_crs=ccrs.PlateCarree())

#Selecciono el dato mas cercano a la latitud, longitud y fecha escogida
pronostico = ds.sel(dict(x = x, y = y, time = FECHA_FCST), method = 'nearest')[var]

print(f'El valor pronosticado para la variable {var} en la latitud {latitud} y longitud {longitud} es: {pronostico.values:0.2f}')
```

    El valor pronosticado para la variable T2 en la latitud -25 y longitud -70 es: 27.21

