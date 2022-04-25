# Interpolación a retícula regular

En esta notebook mostramos cómo transformar los datos que se encuentran en la proyección Conforme de Lambert a una proyección cilíndrica de modo que queden en una retícula regular.<br />
*We  show an example of how to transform a dataset that is in a Lambert conformal projection to a regular grid (equirectangular projection).*


```python
# Importamos las librerías necesarias
# We import the necesssary libraries
import xarray as xr
import h5netcdf
import datetime
import xesmf as xe 
# Importante: La librería xesmf tiene problemas para instarla en una notebook de Google Colab 
# Important: The xesmf library has problems when installing it on a Google Colab notebook.
```

Definimos la fecha de inicialización y de validez del pronóstico a interpolar:<br />
*We define the forecast initialization date and the desired forecast time:*



```python
start_year = 2022
start_month = 4
start_day = 1
start_hour = 0

fcst_year = 2022
fcst_month = 4
fcst_day = 1
fcst_hour = 17

FCST_DATE = datetime.datetime(start_year, start_month, start_day, start_hour)
START_DATE = datetime.datetime(start_year, start_month, start_day, start_hour)

# Calculamos el plazo de pronóstico
# We get the forecast lead time
fhr = int((FCST_DATE - START_DATE).total_seconds()/3600)
```

Leemos el archivo que posee el dato buscado:<br />
*We read the file containing the information we are looking for:*


```python
# Opción 1: Para acceder a los archivos online (FEDE REVISA ESTO!!!)
# Option 1: To access files online
#import s3fs
#s3_file = f'smn-ar-wrf/DATA/WRF/DET/{START_DATE:%Y/%m/%d/%H}/WRFDETAR_01H_{START_DATE:%Y%m%d_%H}_{leadtime:03d}.nc'

#fs = s3fs.S3FileSystem(anon=True)

#if fs.exists(s3_file):
#    f = fs.open(s3_file)
#    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
#else:
#    print('The file does not exist')

# Opción 2: Para abrir un archivo ya descargado
# Option 2: To open an already downloaded file
filename = 'WRFDETAR_01H_{:%Y%m%d_%H}_{:03d}.nc'.format(START_DATE,fhr)
ds = xr.open_dataset(filename, decode_coords = 'all', engine = 'h5netcdf')
```

Definimos la nueva retícula a la que se quiere interpolar:<br />
*We define the target regular grid:*


```python
resolution_lat = 0.1
resolution_lon = 0.1
lat_min = -56
lat_max = -19
lon_min = -76
lon_max = -48

new_grid = xe.util.grid_2d(lon_min - resolution_lon/2, lon_max, resolution_lon, 
                           lat_min - resolution_lat/2, lat_max, resolution_lat)

```

Realizamos la interpolación:<br />
*We performe the interpolation:*


```python
regridder = xe.Regridder(ds, new_grid, 'bilinear')
ds_interpolated = regridder(ds, keep_attrs = True)
```
