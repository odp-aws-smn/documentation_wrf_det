# Pronóstico para una latitud, longitud y fecha determinada

(Última actualización 1 jun 2023)

En este ejemplo se describe cómo obtener el pronóstico de una variable específica del dataset (temperatura a 2 m por ejemplo) en una latitud, longitud y fecha dadas. <br />
*In this example we show how to obtain the forecast of a specific variable (2-meter temperature for example) for a given latitude, longitude and date.*


```python
# Importamos las librerías necesarias
# We import the necessary libraries
import xarray as xr
import h5netcdf
import datetime
import cartopy.crs as ccrs
```

Definimos la fecha de inicialización del pronóstico, plazo de pronóstico y latitud y longitud a consultar. <br />
*We define the forecast initialization date, the desired lead time, latitude and longitude (March 21, 2022 00 UTC; March 22, 2022 17 UTC; -25; -70).*


```python
init_year = 2022
init_month = 4
init_day = 1
init_hour = 0
INIT_DATE = datetime.datetime(init_year, init_month, init_day, init_hour)

lead_time = 17

latitude = -25
longitude = -70
```

Definimos la variable a consultar: <br />
*We define the variable to consult:*


```python
var = 'T2'
```

Leemos el archivo: <br />
*We read the file containing the information we are looking for:*


```python
# Descomentar la opción elegida:

# --------
# Opción 1: Para acceder al archivo online
# Option 1: To access the file online
#!pip install s3fs
#import s3fs
#s3_file = f'smn-ar-wrf/DATA/WRF/DET/{INIT_DATE:%Y/%m/%d/%H}/WRFDETAR_01H_{INIT_DATE:%Y%m%d_%H}_{lead_time:03d}.nc'
#fs = s3fs.S3FileSystem(anon=True)
#if fs.exists(s3_file):
#    f = fs.open(s3_file)
#    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
#else:
#    print('El archivo buscado no existe')
# --------

# --------
# Opción 2: Para abrir un archivo ya descargado
# Option 2: To open an already downloaded file
filename = 'WRFDETAR_01H_{:%Y%m%d_%H}_{:03d}.nc'.format(INIT_DATE,lead_time)
print(filename)
ds = xr.open_dataset(filename, decode_coords = 'all', engine = 'h5netcdf')
# --------
```

    /content/drive/MyDrive/ColabNotebooks/20220401_files/WRFDETAR_01H_20220401_00_017.nc


Obtenemos el valor pronosticado: <br />
*We get the appropriate forecast value:*



```python
# Buscamos la ubicación del punto más cercano a la latitud y longitud solicitada
# We search the closest gridpoint to the selected lat-lon 
data_crs = ccrs.LambertConformal(central_longitude = ds['Lambert_Conformal'].attrs['longitude_of_central_meridian'], 
                                 central_latitude = ds['Lambert_Conformal'].attrs['latitude_of_projection_origin'], 
                                 standard_parallels = ds['Lambert_Conformal'].attrs['standard_parallel'])
x, y = data_crs.transform_point(longitude, latitude, src_crs=ccrs.PlateCarree())

# Seleccionamos el dato mas cercano a la latitud, longitud y fecha escogida
# We extract the value at the chosen gridpoint

forecast = ds.sel(dict(x = x, y = y), method = 'nearest')[var]

print(f'The forecast value for the variable {var} at latitude {latitude} and longitude {longitude} is: {forecast.values[0]:0.2f}°C')

```

    The forecast value for the variable T2 at latitude -25 and longitude -70 is: 26.33°C



```python

```


Para descargar la notebook, acceder al siguiente [link](../notebooks/Get_lat_lon_fecha.ipynb). <br />
*To download the notebook, go to the following [link](../notebooks/Get_lat_lon_fecha.ipynb).*
