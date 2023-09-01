# Formato de datos

El formato de los archivos es **NetCDF** (de sus siglas en inglés Network Common Data Form). Este es un formato destinado a almacenar datos científicos multidimensionales (variables) como puede ser la temperatura y la humedad. La convención utilizada es la <a href="http://cfconventions.org/" target="_blank">CF</a>. Para más información sobre este formato visitar el siguiente <a href="https://docs.unidata.ucar.edu/netcdf-c/current/index.html" target="_blank">link</a>.


**Proyección de los datos** <br />
El tipo de proyección utilizada es la <a href="https://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3/user_guide_V3.9/users_guide_chap3.html" target="_blank">*Confome de Lambert*</a> (retícula no regular). El centro de la retícula se encuentra ubicado en -35° de latitud y -65° de longitud, y la resolución espacial es de 4 km.

**Dimensiones**<br />
Las dimensiones de los datos se encuentra en la siguiente tabla:
|Dimensión   |Valor   |
|---|---|
| time  |1   |
|y   |1249   |
|x   |999   |

**Variables**<br />
Las variables presentes en los archivos son las siguientes: 
|Variable   |Descripción   |Unidad   |Precisión   |Frecuencia   |
|---|---|---|---|---|
|PP   |Precipitación acumulada en 10 miuntos   |mm   |float32   |10M   |
|PP   |Precipitación acumulada en una hora  |mm   |float32   |01H   |
|HR2   |Humedad relativa a 2 metros   |%   |float32   |01H   |
|T2   |Temperatura a 2 metros (\*)   |°C   |float32   |01H   |
|dirViento10   |Dirección del viento a 10 metros   |°   |float32   |01H   |
|magViento10   |Magnitud del viento a 10 metros (\*)   |m/s   |float32   |01H   |
|PSFC   |Presión en superficie   |hPa   |float32   |01H   |
|ACLWDNB   |Radiación de onda larga entrante (\**)   |J/m2   |float32   |01H   |
|ACLWUPB   |Radiación de onda larga saliente (\**)   |J/m2   |float32   |01H   |
|ACSWDNB   |Radiación de onda corta entrante (\**)   |J/m2   |float32   |01H   |
|TSLB   |Temperatura de suelo en la capa 0-10cm  |°C   |float32   |01H   |
|SMOIS   |Humedad de suelo en la capa 0-10cm  |m3/m3   |float32   |01H   |
|Freezing_level   |Altura sobre el nivel del mar de la isoterma de 0°C  |m   |float32   |01H   |
|Tmax   |Temperatura máxima diaria (\*)   |°C   |float32   |24H   |
|Tmin   |Temperatura mínima diaria (\*)   |°C   |float32   |24H   |

(\*) Variables calibradas con observaciones de superficie. Para más información consultar la nota técnica 
<a href="http://repositorio.smn.gob.ar/handle/20.500.12160/1405" target="_blank">Cutraro y otros, 2020</a>. En caso de que no se encuentren disponibles las variables calibradas, se presentará el valor pronosticado sin calibrar.<br />
(\**) Valor acumulado desde el inicio del pronóstico.

En el caso de la Tmin válida para el día X el valor corresponde a la temperatura mínima pronosticada para el día X entre las 00 y las 12 UTC.
Para Tmax el valor del día X corresponde a la temperatura máxima pronosticada entre las 12 UTC del día X y las 00 UTC del día X+1.

Por ejemplo, el archivo WRFDETAR_24H_20220314_00_001.nc que contiene los datos del ciclo 00 UTC para el primer plazo de pronóstico (1° día) tendrá la temperatura mínima pronosticada para el día 20220315 entre las 00 y las 12 UTC y la temperatura máxima pronosticada para el día 20220315 entre las 12 y las 00 UTC del día siguiente.

Para el caso de la PP válida para el día X en el plazo P, el valor corresponde a la precipitación acumulada pronosticada entre el plazo P-1 y P.

Por ejemplo, el archivo WRFDETAR_01H_20220314_00_036.nc que contiene los datos del ciclo 00 UTC para el plazo 36 de pronóstico tendrá la precipitación acumulada pronosticada válida para 15 de marzo de 2022 entre las 11 y las 12 UTC. En el caso del archivo WRFDETAR_10M_20220314_00_036.nc contiene los datos de precipitación acumulada cada 10 minutos entre las 11 y 12 UTC.

**Variables de coordenadas:**<br />
Las variables de coordenas presentes en los archivos son las siguientes:
|Variable   |Descripción   |Unidad   |Precisión   |
|---|---|---|---|
|time   |Tiempo   |Horas desde el inicio del ciclo de pronóstico   |float64   |
|y   |Coordenada y   |Metros desde el centro de la retícula   |float32   |
|x   |Coordenada x   |Metros desde el centro de la retícula   |float32   |
|lat   |Latitud   |° (convención entre 90° y -90°)   |float32   |
|lon   |Longitud   |° (convención entre -180° y 180°)   |float32   |
