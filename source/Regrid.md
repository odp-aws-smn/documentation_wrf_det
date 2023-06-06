# Interpolación a retícula regular

(Última actualización 1 jun 2023)

En esta notebook mostramos cómo transformar los datos que se encuentran en la proyección Conforme de Lambert a una proyección cilíndrica de modo que queden en una retícula regular.<br />
*We  show an example of how to transform a dataset that is in a Lambert conformal projection to a regular grid (equirectangular projection).*


```python
# En caso de utilizar Google Colab, descomentar las siguientes líneas

#!pip install -q condacolab
#import condacolab
#condacolab.install()
#!conda install -c conda-forge xesmf "esmpy<8.4" pandas=1.4
```


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
init_year = 2023
init_month = 4
init_day = 1
init_hour = 0
INIT_DATE = datetime.datetime(init_year, init_month, init_day, init_hour)

lead_time = 12
```

Leemos el archivo que posee el dato buscado:<br />
*We read the file containing the information we are looking for:*


```python
# Descomentar la opción elegida:

# --------
# Opción 1: Para acceder a los archivos online
# Option 1: To access files online
! pip install s3fs
import s3fs
s3_file = f'smn-ar-wrf/DATA/WRF/DET/{INIT_DATE:%Y/%m/%d/%H}/WRFDETAR_01H_{INIT_DATE:%Y%m%d_%H}_{lead_time:03d}.nc'
fs = s3fs.S3FileSystem(anon=True)
if fs.exists(s3_file):
    f = fs.open(s3_file)
    ds = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
else:
    print('The file does not exist')
# --------

# --------
# Opción 2: Para abrir un archivo ya descargado
# Option 2: To open an already downloaded file
#filename = '/content/drive/MyDrive/ColabNotebooks/20220401_files/WRFDETAR_01H_{:%Y%m%d_%H}_{:03d}.nc'.format(INIT_DATE,lead_time)
#print(filename)
#ds = xr.open_dataset(filename, decode_coords = 'all', engine = 'h5netcdf')
# --------
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
    Dimensions:            (time: 1, y: 370, x: 280)
    Coordinates:
      * time               (time) datetime64[ns] 2023-04-01T12:00:00
        Lambert_Conformal  object ...
        lat                (y, x) float64 -56.0 -56.0 -56.0 ... -19.1 -19.1 -19.1
        lon                (y, x) float64 -76.0 -75.9 -75.8 ... -48.3 -48.2 -48.1
    Dimensions without coordinates: y, x
    Data variables:
        PP                 (time, y, x) float32 0.0235 0.003517 ... 0.0 0.0
        T2                 (time, y, x) float32 5.836 5.955 6.022 ... 23.48 23.71
        PSFC               (time, y, x) float32 988.3 988.3 988.3 ... 915.2 909.8
        TSLB               (time, y, x) float32 0.01001 0.01001 ... 22.35 21.95
        SMOIS              (time, y, x) float32 1.0 1.0 1.0 ... 0.23 0.228 0.2166
        ACLWDNB            (time, y, x) float32 1.423e+07 1.427e+07 ... 1.578e+07
        ACLWUPB            (time, y, x) float32 1.491e+07 1.491e+07 ... 1.776e+07
        ACSWDNB            (time, y, x) float32 2.178e+04 2.788e+04 ... 2.711e+06
        HR2                (time, y, x) float32 88.85 89.89 89.52 ... 75.32 72.43
        dirViento10        (time, y, x) float32 275.5 271.8 268.9 ... 179.1 179.5
        magViento10        (time, y, x) float32 7.756 7.279 6.595 ... 2.855 3.026
    Attributes: (12/13)
        title:          Python PostProcessing for SMN WRF-ARW
        institution:    Servicio Meteorologico Nacional
        source:          OUTPUT FROM WRF V4.0 MODEL
        min_lat:        -56.853172
        min_lon:        -94.33081
        max_lat:        -11.645958
        ...             ...
        MAP_PROJ:       Lambert Conformal
        DX:             4000.0
        DY:             4000.0
        START_DATE:     2023-04-01 00:00:00
        Conventions:    CF-1.8
        regrid_method:  bilinear




Para descargar la notebook, acceder al siguiente [link](../notebooks/Regrid.ipynb). <br />
*To download the notebook, go to the following [link](../notebooks/Regrid.ipynb).*


