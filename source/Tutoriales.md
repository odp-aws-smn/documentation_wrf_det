# Tutoriales


Los datos se encuentran disponibles en un repositorio de Amazon Web Service (AWS) en la siguiente dirección [https://direccion_del_repositorio.com](https://direccion_del_repositorio.com). Los datos pueden ser descargados manualmente desde esa dirección, pero también se pueden descargar de manera automática mediante comandos de Python.

## Librerías de Python a utilizar


*   [**S3Fs**](https://s3fs.readthedocs.io/en/latest/)
> Librería que permite el acceso y descarga de los datos del repositorio.

*   [**xarray**](https://xarray.pydata.org/en/stable/)
> Librería que permite una fácil lectura y procesamiento de los archivos [NetCDF](https://www.unidata.ucar.edu/software/netcdf/) en qué están guardados los datos.

* [**datetime**](https://docs.python.org/es/3/library/datetime.html)
> Librería para el manejo de fechas.

* [**Matplotlib**](https://matplotlib.org/)
> Librería para graficar.

* [**os**](https://docs.python.org/es/3.10/library/os.html)
> Librería de interfaz con el sistema operativo.

* [**Cartopy**](https://scitools.org.uk/cartopy/docs/latest/)
> Librería para el procesamiento de datos geográficos.

* [**Regionmask**](https://regionmask.readthedocs.io/en/stable/)
> Librería para generar máscaras de zonas geográficas.


```python
!pip install cartopy
!pip uninstall shapely -y
!pip install shapely --no-binary shapely
!pip install regionmask
!pip install s3fs
```


```python
#Importamos las librerías
import s3fs
import xarray as xr
import datetime
import matplotlib.pyplot as plt
import os
import cartopy.crs as ccrs
import cartopy.feature as cf
import regionmask
```


```python
from google.colab import drive
drive.mount('/content/drive')
```

## Descarga de los archivos

Los archivos subidos al repositorio se encuentran dentro de directorios asociados a la fecha y ciclo de inicialización del pronóstico. Por ejemplo, los datos del pronóstico inicializado en 1 de enero de 2022 a las 00Z se encuentran en el directorio 20220101 y, dentro de este, en el directorio 00.

Las variables disponibles son:
* Temperatura a 2 m (T2) en °C
* Humedad relativa a 2 m (HR2) en %
* Magnitud del viento a 10 m (magViento10) en m/s
* Dirección del viento a 10m (dirViento10) en grados
* Precipitación horaria acumulada (PP) en mm
* Temperatura mínima diaria (Tmin) en °C
* Temperatura máxima diaria (Tmax) en °C

Entre paréntesis se indica el nombre que tiene asignada cada variable en los archivos.

Los archivos dentro del repositorio se encuentran separados por plazo de pronóstico y por si son datos horarios o integrados a lo largo de un día. Las primeras 5 variables mencionadas tienen frecuencia horaria con lo cual el plazo se corresponde a las horas de pronóstico transcurridas desde la inicialización. En las últimas dos variables, al ser diarias, el plazo representa los días transcurridos desde la inicialización.


```python
#Fecha y ciclo para los que se quieren descargar los datos --> SELECCIONAR 
año = 2022
mes = 3
dia = 8
ciclo = 00


#Configuracion para las variables y plazos que se quiere descargar --> SELECCIONAR
variables_horarias = ['T2', 'HR', 'magViento10', 'dirViento10', 'PP'] #Lista de variables horarias a descargar
Pi_horarias = 0 #Plazo inicial para las variables horarias
Pf_horarias = 72 #Plazo final para las variables horarias
Int_horarias = 1 #Intervalo entre plazos para las variables horarias

variables_diarias = ['Tmin', 'Tmax'] #Lista de variables diarias a descargar
Pi_diarias = 0 #Plazo inicial para las variables diarias
Pf_diarias = 3 #Plazo final para las variables diarias
Int_diarias = 1 #Intervalo entre plazos para las variables diarias

DIR_SALIDA = './' #Directorio donde se van a guardar los datos descargados --> SELECCIONAR
```


```python
#Plazos de las variables horarias a descargar
plazos_horarias = [x for x in range(Pi_horarias, Pf_horarias + 1, Int_horarias)] 

#Plazos de las variables diarias a descargar
plazos_diarias = [x for x in range(Pi_diarias, Pf_diarias + 1, Int_diarias)] 

variables_horarias = ['T2', 'PP']
variables_diarias = ['Tmin']
variables = {'horarias':variables_horarias, 'diarias':variables_diarias}
plazos = {'horarias':plazos_horarias, 'diarias':plazos_diarias}
DIR_SALIDA = '/content/drive/MyDrive/AWS/archivos_prueba/' #Directorio donde se van a guardar los datos descargados
```


```python
#Fecha a descargar
FECHA = datetime.datetime(año, mes, dia, ciclo)

#Plazos de las variables horarias a descargar
plazos_horarias = [x for x in range(Pi_horarias, Pf_horarias + 1, Int_horarias)] 

#Plazos de las variables diarias a descargar
plazos_diarias = [x for x in range(Pi_diarias, Pf_diarias + 1, Int_diarias)] 

variables = {'horarias':variables_horarias, 'diarias':variables_diarias}
plazos = {'horarias':plazos_horarias, 'diarias':plazos_diarias}

#Uso de credenciales anonimas para acceder a la informacion del repositorio
fs = s3fs.S3FileSystem(anon=True)

DIR_REPO = '/PATH_AL_REPO/'

listas_ds = {'horarias':[], 'diarias':[]}
for tipo in variables.keys():
    s3path = FECHA.strftime(f'{DIR_REPO}/%Y%m%d/%H/{tipo}/*.nc)')
    archivos_disponibles = s3.glob(s3path)
    for f in archivos_disponibles:
        with s3.open(f) as archivo_s3:
            ds = xr.open_dataset(archivo_s3, decode_timedelta = False)
            listas_ds[tipo].append(ds)
    
#Concatenacion de los archivos
Dataset_horario = xr.combine_by_coords(listas_ds['horarias'], combine_attrs = 'drop_conflicts')
Dataset_diario = xr.combine_by_coords(listas_ds['diarias'], combine_attrs = 'drop_conflicts')
```


```python
#Fecha a descargar
FECHA = datetime.datetime(año, mes, dia, ciclo)

#Plazos de las variables horarias a descargar
plazos_horarias = [x for x in range(Pi_horarias, Pf_horarias + 1, Int_horarias)] 

#Plazos de las variables diarias a descargar
plazos_diarias = [x for x in range(Pi_diarias, Pf_diarias + 1, Int_diarias)] 

variables = {'horarias':variables_horarias, 'diarias':variables_diarias}
plazos = {'horarias':plazos_horarias, 'diarias':plazos_diarias}

#Uso de credenciales anonimas para acceder a la informacion del repositorio
fs = s3fs.S3FileSystem(anon=True)

DIR_REPO = '/PATH_AL_REPO/'


for tipo in variables.keys():
    for var in variables[tipo]:
        for p in plazos[tipo]:
            archivo = FECHA.strftime(f'{DIR_REPO}/%Y%m%d/%H/{tipo}/{var}_{p:03d}.nc)')
            if fs.exists(archivo):
                print(' Descargando el archivo: ', archivo.split('/')[-1])
                fs.get(archivo, DIR_SALIDA)
            else:
                print('No se encontro el archivo: ', archivo.split('/')[-1])

```

## Lectura de los archivos descargados

Una vez descargados todos los archivos requeridos se procede a leerlos y a concatenarlos para tener todas las variables en un mismo [Dataset](https://xarray.pydata.org/en/stable/generated/xarray.Dataset.html)


```python
#Lectura de los archivos
listas_ds = {'horarias':[], 'diarias':[]}
for tipo in variables.keys():
    for p in plazos[tipo]:
        archivo = f'{DIR_SALIDA}/{FECHA:%Y%m%d/%H}/{tipo}/{p:03d}.nc'
        if os.path.isfile(archivo):
            print(archivo)
            listas_ds[tipo].append(xr.open_dataset(archivo, decode_coords = 'all', decode_timedelta = False))

#Concatenacion de los archivos
Dataset_horario = xr.combine_by_coords(listas_ds['horarias'], combine_attrs = 'drop_conflicts')
Dataset_diario = xr.combine_by_coords(listas_ds['diarias'], combine_attrs = 'drop_conflicts')
```


```python
Dataset_horario
```

## Procesamiento de los pronósticos

A continuación se presentan algunos ejemplos de procesamiento de los pronósticos.

### Pronóstico de temperatura a 2 m para una latitud, longitud y fecha determinada


```python
latitud = -25
longitud = -70
año = 2022
mes = 1
dia = 2
hora = 12

FECHA = datetime.datetime(año, mes, dia, hora)

#Se busca la ubicacion del punto mas cercano a la latitud y longitud solicitada
data_crs = ccrs.LambertConformal(central_longitude = Dataset_horario.CEN_LON, 
                                 central_latitude = Dataset_horario.CEN_LAT, 
                                 standard_parallels = (Dataset_horario.TRUELAT1, Dataset_horario.TRUELAT2))
x, y = data_crs.transform_point(longitud, latitud, src_crs=ccrs.PlateCarree())

#Selecciono el dato mas cercano a la latitud, longitud y fecha escogida
pronostico = Dataset_horario.sel(dict(x = x, y = y, time = FECHA), method = 'nearest')['T2']


print(f'El valor pronosticado para la temperatura a 2 m es: {pronostico.values:0.2f}')
```

### Meteograma de temperatura a 2 m y precipitación


```python
latitud = -35
longitud = -55

#Se busca la ubicacion del punto mas cercano a la latitud y longitud solicitada
data_crs = ccrs.LambertConformal(central_longitude = Dataset_horario.CEN_LON, 
                                 central_latitude = Dataset_horario.CEN_LAT, 
                                 standard_parallels = (Dataset_horario.TRUELAT1, Dataset_horario.TRUELAT2))
x, y = data_crs.transform_point(longitud, latitud, src_crs=ccrs.PlateCarree())

#Selecciono el dato mas cercano a la latitud, longitud
pronostico = Dataset_horario.sel(dict(x = x, y = y), method = 'nearest')

#Obtengo la serie de temperatura a 2 m, precipitacion acumulada y de fechas
T2 = pronostico['T2']
PP = pronostico['PP']
fechas = pronostico['time']

#Inicio la figura
fig, ax = plt.subplots(figsize = (10, 8))
#Duplico el eje x
ax2 = ax.twinx()
#Grafico la precipitacion en barras
ax.bar(fechas, PP, color = 'blue', width = 0.03, label = 'Precip. Acum.')
#Grafico la tempertura con una linea
ax2.plot(fechas, T2, color = 'red', label = 'Temp. 2 m', linewidth = 3)
#Defino las etiquetas de los ejes
ax.set_xlabel('Fecha')
ax2.set_ylabel('T 2m (°C)')
ax.set_ylabel('PP (mm)')
#Defino el titulo de la figura
plt.title(f'Temperatura a 2 m y precipitacion acumulada \n lat = {latitud:0.2f}, lon = {longitud:0.2f}')
#Grafico la leyenda
fig.legend(loc = 'upper right')
#Ajusto el grafico al tamaño de la figura
plt.tight_layout()
```

### Mapa de temperatura media diaria para una región


```python
lat_min = -60
lat_max = -30
lon_min = -80
lon_max = -60

año_ini = 2022
mes_ini = 1
dia_ini = 18
hora_ini = 0

año_fin = 2022
mes_fin = 1
dia_fin = 18
hora_fin = 23

FECHA_INI = datetime.datetime(año_ini, mes_ini, dia_ini, hora_ini)
FECHA_FIN = datetime.datetime(año_fin, mes_fin, dia_fin, hora_fin)

#Defino los limite de la region a enmascarar
#esquinas = np.array([[lon_min, lat_min], [lon_min, lat_max], [lon_max, lat_max], [lon_max, lat_min]])
esquinas = [[lon_min, lat_min], [lon_min, lat_max], [lon_max, lat_max], [lon_max, lat_min]]

#Armo la mascara
region = regionmask.Regions([esquinas])
mascara = region.mask(Dataset_diario['lon'], Dataset_diario['lat'])

#selecciono la variable Tmin y la fecha escogida
T2 = Dataset_horario[['T2']]
T2_fechas = T2.sel(dict(time = slice(FECHA_INI, FECHA_FIN)))
T2_media = T2.mean(dim = 'time')

#Aplico la mascara eliminando los valores por fuera de esta
T2_region = T2_media.where(mascara == 0, drop = True)

# Determino la proyeccion de los datos
proyeccion = ccrs.LambertConformal(central_longitude = Dataset_diario.CEN_LON, 
                                   central_latitude = Dataset_diario.CEN_LAT, 
                                   standard_parallels = (Dataset_diario.TRUELAT1, 
                                                         Dataset_diario.TRUELAT2))

fig = plt.figure(figsize = (10, 8)), 
ax = plt.axes(projection = proyeccion)
cbar = ax.pcolormesh(T2_region['lon'], T2_region['lat'], T2_region['T2'], transform = ccrs.PlateCarree())
ax.add_feature(cf.COASTLINE) #Agrega las costas
ax.add_feature(cf.BORDERS) #Agrega los limites de los paises
ax.set_title(f'Temperatura media \n {FECHA_INI:%d/%m/%Y}')

gl = ax.gridlines(crs = ccrs.PlateCarree(), draw_labels = True, x_inline = False,
                  linewidth = 2, color = 'gray', alpha = 0.5, linestyle = '--')
gl.top_labels = False
gl.right_labels = False
plt.colorbar(cbar)
```
