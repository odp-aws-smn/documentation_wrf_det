# Formato de datos

El formato de los archivos es **NetCDF** (de sus siglas en inglés Network Common Data Form). Este es un formato destinado a almacenar datos científicos multidimensionales (variables) como puede ser la temperatura y la humedad. La convención utilizada es la [CF](http://cfconventions.org/). Para más información sobre este formato visitar el siguiente [link](https://docs.unidata.ucar.edu/netcdf-c/current/index.html).


**Proyección de los datos** <br />
El tipo de proyección utilizada es la [*Confome de Lambert*](https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/user_guide_V3.9/users_guide_chap3.html) (retícula no regular). El centro de la retícula se encuentra ubicado en -35° de latitud y -65° de longitud, y la resolución espacial es de 4 km.

**Dimensiones**<br />
Las dimensiones de los datos se encuentra en la siguiente tabla:
|Dimensión   |Valor   |
|---|---|
| time  |1   |
|y   |1249   |
|x   |999   |

**Variables**<br />
Las variables presentes en los archivos son las siguientes: 
|Variable   |Descripción   |Unidad   |Precisión   |
|---|---|---|---|
|PP   |Precipitación acumulada en un período de tiempo   |mm   |float32   |
|HR2   |Humedad relativa a 2 metros   |%   |float32   |
|T2   |Temperatura a 2 metros (\*)   |°C   |float32   |
|dirViento10   |Dirección del viento a 10 metros   |°   |float32   |
|magViento10   |Magnitud del viento a 10 metros (\*)   |m/s   |float32   |
|Tmax   |Temperatura máxima diaria (\*)   |°C   |float32   |
|Tmin   |Temperatura mínima diaria (\*)   |°C   |float32   |

(\*) Variables calibradas con observaciones de superficie. Para más información consultar la nota técnica [Cutraro y otros, 2020](http://hdl.handle.net/20.500.12160/1405). En caso de que no se encuentren disponibles las variables calibradas, se presentará el valor pronosticado sin calibrar.

En el caso de la Tmin válida para el día X el valor corresponde a la temperatura mínima pronosticada para el día X entre las 00 y las 12 UTC.
Para Tmax el valor del día X corresponde a la temperatura máxima pronosticada entre las 12 UTC del día X y las 00 UTC del día X+1.

Por ejemplo, el archivo WRFDETAR_24H_20220314_00_001.nc que contiene los datos del ciclo 00 UTC para el primer plazo de pronóstico (1° día) tendrá la temperatura mínima pronosticada para el día 20220315 entre las 00 y las 12 UTC y la temperatura máxima pronosticada para el día 20220315 entre las 12 y las 00 UTC del día siguiente.

Para el caso de la PP válida para el día X en el plazo P, el valor corresponde a la precipitación acumulada pronosticada entre el plazo P-1 y P.

Por ejemplo, el archivo WRFDETAR_01H_20220314_00_036.nc que contiene los datos del ciclo 00 UTC para el plazo 36 de pronóstico tendrá la precipitación acumulada pronosticada válida para 20220315 entre las 11 y las 12 UTC.

**Variables de coordenadas:**<br />
|Variable   |Descripción   |Unidad   |Precisión   |
|---|---|---|---|
|time   |Tiempo   |Horas desde el inicio del ciclo de pronóstico   |int   |
|y   |Coordenada y   |Metros desde el centro de la retícula   |float32   |
|x   |Coordenada x   |Metros desde el centro de la retícula   |float32   |
|lat   |Latitud   |° (convención entre 90° y -90°)   |float32   |
|lon   |Longitud   |° (convención entre -180° y 180°)   |float32   |
