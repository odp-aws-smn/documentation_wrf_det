# Data Format

El formato de los archivos es **NetCDF** (de sus siglas en inglés Network Common Data Form). Este es un formato destinado a almacenar datos científicos multidimensionales (variables) como puede ser la temperatura y la humedad. La convención utilizada es la [CF](http://cfconventions.org/). Para más información sobre este formato: https://docs.unidata.ucar.edu/netcdf-c/current/index.html

Los datos dentro de cada archivo se estructuran en un Dataset:

**Dimensiones:**
|Dimensión   |Valor   |
|---|---|
| time  |1   |
|y   |1249   |
|x   |999   |

**Variables:**
|Variable   |Descripción   |Unidad   |
|---|---|---|
|PP   |Precipitación acumulada en un período de tiempo   |mm   |
|HR2   |Humedad relativa a 2 metros   |%   |
|T2   |Temperatura a 2 metros   |°C   |
|dirViento10   |Dirección del viento a 10 metros   |°   |
|magViento10   |Magnitud del viento a 10 metros   |m/s   |
|Tmax   |Temperatura máxima diaria   |°C   |
|Tmin   |Temperatura mínima diaria   |°C   |

En el caso de la Tmín válida para el día X el valor corresponde a la temperatura mínima pronosticada para el día X entre las 00Z y las 12Z.
Para Tmáx el valor del día X corresponde a la temperatura máxima pronosticada entre las 12Z del día X y las 00Z del día X+1.

Ejemplo: En el caso del archivo 

Variables de coordenadas

Atributos

* La retícula es Lambert Conformal
* La resolución es 4 km
