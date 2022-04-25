# Pronóstico para una latitud, longitud y fecha determinada

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

Definimos la fecha de inicialización del pronóstico, y la validez y latitud y longitud a consultar. <br />
*We define the starting date of the forecast, the desired forecast time, latitude and longitude (March 21, 2022 00 UTC; March 22, 2022 17 UTC; -25; -70).*


```python
latitude = -25
longitude = -70

start_year = 2022
start_month = 4
start_day = 1
start_hour = 0

fcst_year = 2022
fcst_month = 4
fcst_day = 2
fcst_hour = 17

START_DATE = datetime.datetime(start_year, start_month, start_day, start_hour) # inicialization date of the forecast
FCST_DATE = datetime.datetime(fcst_year, fcst_month, fcst_day, fcst_hour)    # date of interest (valid date)

# Calculamos el plazo de pronóstico
# We get the forecast lead time
fhr = int((FCST_DATE - START_DATE).total_seconds()/3600)
```

Definimos la variable a consultar: <br />
*We define the variable to consult:*


```python
var = 'T2'
```

Leemos el archivo: <br />
*We read the file containing the information we are looking for:*


```python
# Opción 1: Para acceder al archivo online
# Option 1: To access the file online
#import s3fs
#s3_file = f'smn-ar-wrf/DATA/WRF/DET/{START_DATE:%Y/%m/%d/%H}/WRFDETAR_01H_{START_DATE:%Y%m%d_%H}_{fhr:03d}.nc'

#fs = s3fs.S3FileSystem(anon=True)

#if fs.exists(s3_file):
#    f = fs.open(s3_file)
#    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
#else:
#    print('El archivo buscado no existe')

# Opción 2: Para abrir un archivo ya descargado
# Option 2: To open an already downloaded file
filename = 'WRFDETAR_01H_{:%Y%m%d_%H}_{:03d}.nc'.format(START_DATE,fhr)
print(filename)
ds = xr.open_dataset(filename, decode_coords = 'all', engine = 'h5netcdf')
```

Obtenemos el valor pronosticado: <br />
*We get the appropriate forecast value:*



```python
# Buscamos la ubicación del punto más cercano a la latitud y longitud solicitada
# We search the closest gridpoint to the selected lat-lon 

data_crs = ccrs.LambertConformal(central_longitude = ds.CEN_LON, 
                                 central_latitude = ds.CEN_LAT, 
                                 standard_parallels = (ds.TRUELAT1, ds.TRUELAT2))
x, y = data_crs.transform_point(longitude, latitude, src_crs=ccrs.PlateCarree())

# Seleccionamos el dato mas cercano a la latitud, longitud y fecha escogida
# We extract the value at the chosen gridpoint
forecast = ds.sel(dict(x = x, y = y, time = FCST_DATE), method = 'nearest')[var]

print(f'The forecast value for the variable {var} at latitude {latitude} and longitude {longitude} is: {forecast.values:0.2f}')

```

    The forecast value for the variable T2 at latitude -25 and longitude -70 is: 24.44

