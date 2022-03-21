# Data Overview

La estructura de directorios y nombres de archivos es la siguiente: 

/DATA/WRF/{esquema}/{año}/{mes}/{día}/{ciclo}/WRFDETAR_{frecuencia}H_{año}{mes}{día}\_{ciclo}_{plazo}.nc

donde los campos entre llaves indican: <br />
{esquema} = 3 letras en mayúsculas que indican el tipo de esquema empleado (DET o ENS) <br />
{año} = 4 dígitos para el año <br />
{mes} = 2 dígitos para el mes <br />
{día} = 2 dígitos para el día <br />
{ciclo} = 2 dígitos para el ciclo de pronóstico <br />
{frecuencia} = 2 dígitos para indicar la frecuencia del los datos. Actualmente solo se disponibilizan 01H y 24H. <br />
{plazo} = 3 dígitos para el plazo de pronóstico.

Ejemplos:
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_01H_20220314_00_000.nc
* /DATA/WRF/DET/2022/03/14/00/WRFDETAR_24H_20220314_00_000.nc
