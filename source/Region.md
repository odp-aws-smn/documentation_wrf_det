# Gráfico de una región

Ejemplo para el cálculo de la temperatura mínima media en una región


```python
import xarray as xr
import h5netcdf
import datetime
import s3fs
import cartopy.crs as ccrs
import cartopy.feature as cf
import matplotlib.pyplot as plt
import regionmask
```

Se define la fecha de inicialización del pronóstico.


```python
año_ini = 2022
mes_ini = 3
dia_ini = 21
hora_ini = 0
```

Se define la región a graficar


```python
lat_min = -60
lat_max = -30
lon_min = -80
lon_max = -60
```

Lectura de los pronóstico


```python
FECHA_INI = datetime.datetime(año_ini, mes_ini, dia_ini, hora_ini)

fs = s3fs.S3FileSystem(anon=True)

files = fs.glob(f'smn-ar-wrf/DATA/WRF/DET/{FECHA_INI:%Y/%m/%d/%H}/WRFDETAR_24H_{FECHA_INI:%Y%m%d_%H}_*.nc')

ds_list = []
for s3_file in files:
    print(s3_file)
    f = fs.open(s3_file)
    ds_tmp = xr.open_dataset(f, decode_coords = 'all', engine = 'h5netcdf')
    ds_list.append(ds_tmp)

ds = xr.combine_by_coords(ds_list, combine_attrs = 'drop_conflicts')
```

Se seleccionan los datos pertenecientes a la región y se calcula la temperatura mínima media diaria


```python
esquinas = [[lon_min, lat_min], [lon_min, lat_max], [lon_max, lat_max], [lon_max, lat_min]]

#Armo la mascara
region = regionmask.Regions([esquinas])
mascara = region.mask(ds['lon'], ds['lat'])

#selecciono la variable Tmin y la fecha escogida
Tmin = ds[['Tmin']]
Tmin_media = Tmin.mean(dim = 'time')

#Aplico la mascara eliminando los valores por fuera de esta
Tmin_region = Tmin_media.where(mascara == 0, drop = True)
```


```python
# Determino la proyeccion de los datos
proyeccion = ccrs.LambertConformal(central_longitude = ds.CEN_LON, 
                                   central_latitude = ds.CEN_LAT, 
                                   standard_parallels = (ds.TRUELAT1, 
                                                         ds.TRUELAT2))

fig = plt.figure(figsize = (10, 8)), 
ax = plt.axes(projection = proyeccion)
cbar = ax.pcolormesh(Tmin_region['lon'], Tmin_region['lat'], Tmin_region['Tmin'], transform = ccrs.PlateCarree())
ax.add_feature(cf.COASTLINE) #Agrega las costas
ax.add_feature(cf.BORDERS) #Agrega los limites de los paises
ax.set_title(f'Temperatura mínima media')

gl = ax.gridlines(crs = ccrs.PlateCarree(), draw_labels = True, x_inline = False,
                  linewidth = 2, color = 'gray', alpha = 0.5, linestyle = '--')
gl.top_labels = False
gl.right_labels = False
plt.colorbar(cbar)
```

![png](../figuras/Region.png)
    
Para descargar la notebook acceder al siguiente [link](../notebooks/Region.ipynb)
