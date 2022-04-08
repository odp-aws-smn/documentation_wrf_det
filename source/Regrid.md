# Interpolación a retícula regular

En esta notebook se da un ejemplo de cómo transformar los datos que se encuentran en la proyección Conforme de Lambert a una proyección cilíndrica de modo que queden en una retícula regular.

```python
# Se importan las librerías necesarias
import xarray as xr
import h5netcdf
import datetime
import s3fs
import xesmf as xe
```

Se define la fecha de inicialización y de validez del pronóstico a interpolar:

```python
año_ini = 2022
mes_ini = 3
dia_ini = 21
hora_ini = 0

año_fcst = 2022
mes_fcst = 3
dia_fcst = 22
hora_fcst = 17
```

Se lee el archivo que posee el dato buscado:

```python
FECHA_INI = datetime.datetime(año_ini, mes_ini, dia_ini, hora_ini)
FECHA_FCST = datetime.datetime(año_fcst, mes_fcst, dia_fcst, hora_fcst)

# Plazo de pronóstico
plazo = int((FECHA_FCST - FECHA_INI).total_seconds()/3600)

s3_file = f'smn-ar-wrf/DATA/WRF/DET/{FECHA_INI:%Y/%m/%d/%H}/WRFDETAR_01H_{FECHA_INI:%Y%m%d_%H}_{plazo:03d}.nc'

fs = s3fs.S3FileSystem(anon=True)

if fs.exists(s3_file):
    f = fs.open(s3_file)
    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
else:
    print('El archivo buscado no existe')
```

Se define la nueva retícula a la que se quiere interpolar:

```python
resolucion_lat = 0.1
resolucion_lon = 0.1
lat_min = -56
lat_max = -19
lon_min = -76
lon_max = -48

nueva_reticula = xe.util.grid_2d(lon_min - resolucion_lon/2, lon_max, resolucion_lon, 
                                 lat_min - resolucion_lat/2, lat_max, resolucion_lat)

```

Se realiza la interpolación:

```python
regridder = xe.Regridder(ds, nueva_reticula, 'bilinear')
ds_interpolado = regridder(ds, keep_attrs = True)
```

Para descargar la notebook, acceder al siguiente [link](../notebooks/Regrid.ipynb).
