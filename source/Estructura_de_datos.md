# Estructura de datos

La estructura de directorios y nombres de archivos es la siguiente: 

/DATA/WRF/{esquema}/{año}/{mes}/{día}/{ciclo}/WRFDETAR_{frecuencia}H_{año}{mes}{día}\_{ciclo}_{plazo}.nc

donde los campos entre llaves indican: <br />
{esquema} = 3 letras en mayúsculas que indican el tipo de esquema empleado (DET o ENS) <br />
{año} = 4 dígitos para el año <br />
{mes} = 2 dígitos para el mes <br />
{día} = 2 dígitos para el día <br />
{ciclo} = 2 dígitos para el ciclo de pronóstico <br />
{frecuencia} = 2 dígitos para indicar la resolución temporal de los datos. En el caso de datos horarios toma valor 01 y en caso de los diarios 24. <br />
{plazo} = 3 dígitos para el plazo de pronóstico. Si la frecuencia es 01H la unidad del plazo son horas y si es 24H son días.

Ejemplos:
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_01H_20220314_00_005.nc corresponde a los pronósticos de las variables horarias inicializados el 14 de marzo de 2022 a las 00 UTC para el plazo 05 horas.
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_24H_20220314_00_001.nc corresponde a los pronósticos de las variables diarias inicializados el 14 de marzo de 2022 a las 00 UTC para el plazo 01 día.
