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

Definimos la fecha de inicialización y el plazo de pronóstico a interpolar:<br />
*We define the forecast initialization date and the desired forecast lead time:*



```python
init_year = 2022
init_month = 4
init_day = 1
init_hour = 0
INIT_DATE = datetime.datetime(init_year, init_month, init_day, init_hour)

lead_time = 12
```

Leemos el archivo que posee el dato buscado:<br />
*We read the file containing the information we are looking for:*


```python
# Opción 1: Para acceder a los archivos online
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
filename = 'WRFDETAR_01H_{:%Y%m%d_%H}_{:03d}.nc'.format(INIT_DATE,lead_time)
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
print(ds_interpolated)
```

    <xarray.Dataset>
    Dimensions:            (time: 1, y: 371, x: 281)
    Coordinates:
        Lambert_Conformal  float32 ...
      * time               (time) datetime64[ns] 2022-04-01T12:00:00
        lat                (y, x) float64 -56.0 -56.0 -56.0 ... -19.0 -19.0 -19.0
        lon                (y, x) float64 -76.0 -75.9 -75.8 ... -48.2 -48.1 -48.0
    Dimensions without coordinates: y, x
    Data variables:
        PP                 (time, y, x) float32 0.001074 0.001887 ... 0.0 0.0
        T2                 (time, y, x) float32 5.486 5.433 5.269 ... 22.96 23.02
        HR2                (time, y, x) float32 85.47 86.79 87.88 ... 66.78 68.96
        dirViento10        (time, y, x) float32 290.7 290.4 289.0 ... 119.4 144.1
        magViento10        (time, y, x) float32 12.31 11.37 10.67 ... 4.247 2.574
    Attributes: (12/20)
        title:          Python PostProcessing for SMN WRF-ARW Deterministic SFC
        institution:    Servicio Meteorologico Nacional
        source:          OUTPUT FROM WRF V4.0 MODEL
        start_lat:      -54.386837
        start_lon:      -94.33081
        end_lat:        -11.645958
        ...             ...
        DX:             4000.0
        DY:             4000.0
        START_DATE:     2022-04-01_00:00:00
        Conventions:    CF-1.8
        NCO:            netCDF Operators version 4.7.5 (Homepage = http://nco.sf....
        regrid_method:  bilinear



Para descargar la notebook, acceder al siguiente [link](../notebooks/Regrid.ipynb). <br />
*To download the notebook, go to the following [link](../notebooks/Regrid.ipynb).*
