# Estructura de datos

La estructura de directorios y nombres de archivos es la siguiente: 

/DATA/WRF/{esquema}/{año}/{mes}/{día}/{inicialización}/WRFDETAR_{frecuencia}\_{año}{mes}{día}\_{inicialización}_{plazo}.nc

donde los campos entre llaves indican: <br />
{esquema} = 3 letras en mayúsculas que indican el tipo de esquema empleado (DET) <br />
{año} = 4 dígitos para el año <br />
{mes} = 2 dígitos para el mes <br />
{día} = 2 dígitos para el día <br />
{inicialización} = 2 dígitos para la hora de inicialización del ciclo de pronóstico (00, 06, 12 ó 18) <br />
{frecuencia} = 2 dígitos para indicar la resolución temporal de los datos y una letra que indica la unidad de tiempo. Por ejemplo, en el caso de datos cada 10 minutos toma el valor de 10M, para datos horarios toma valor 01H y en caso de los diarios 24H.  <br />
{plazo} = 3 dígitos para el plazo de pronóstico. En el caso de datos horarios toma valores de 0 a 73 y la unidad del plazo es hora. En el caso de datos diarios toma valores 000, 001, 002 ó 003 y la unidad del plazo es día. 

Todas las horas se consideran en relación al "Universal Time Coordinated" (UTC). 

Ejemplos:
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_10M_20220314_00_012.nc corresponde a los pronósticos inicializados el 14 de marzo de 2022 a las 00 UTC para el plazo 12 horas de las variables con una frecuencia de 10 minutos. Este archivo contiene 6 tiempos cuya validez va desde 12:00UTC hasta 12:50UTC. 
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_01H_20220314_00_005.nc corresponde a los pronósticos inicializados el 14 de marzo de 2022 a las 00 UTC para el plazo 05 horas de las variables con frecuencia horaria. Este archivo contiene 1 solo tiempo con validez 05:00UTC.
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_24H_20220314_00_001.nc corresponde a los pronósticos inicializados el 14 de marzo de 2022 a las 00 UTC para el plazo 01 día de las variables con frecuencia diaria. Este archivo contiene 1 solo tiempo con validez 15 de marzo de 2022. 
