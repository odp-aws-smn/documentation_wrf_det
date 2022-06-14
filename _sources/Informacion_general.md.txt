# Información general

El SMN-Arg corre en forma operativa el modelo regional de alta resolución Weather Research and Forecasting Model (WRF) en su versión 4.0 con núcleo dinámico Advanced Research WRF (ARW) (<a href="https://www2.mmm.ucar.edu/wrf/users/docs/technote/v4_technote.pdf" target="_blank">Skamarock et al. 2019</a>). El modelo resuelve la convección en forma explícita con paso de tiempo variable y fue configurado utilizando las siguientes parametrizaciones: <br />
- Microfísica: WSM6 (un momento - 6 clases)
- Radiación onda larga: RRTM
- Radiación onda corta: Dudhia
- Capa Límite Planetaria: MYJ (Mellor, Yamada, Janjic)
- Modelo de suelo: NOAH, 4 capas (0-10 cm, 10-40 cm, 40-100 cm, 1-2 m)

Los pronósticos horarios generados con el modelo WRF-SMN cuentan con una resolución horizontal de 4 km con 45 niveles verticales (tope 10 hPa) y plazo máximo de 72 horas. Los mismos se inicializan con los análisis y pronósticos horarios del NCEP-NOAA Global Forecasting System Model 
(<a href="https://www.emc.ncep.noaa.gov/emc/pages/numerical_forecast_systems/gfs.php" target="_blank">GFS</a>) de 0,25° de resolución horizontal.<br />

La proyección de los datos es Conforme de Lambert y el dominio abarca el sur de Sudamérica, incluyendo Argentina y los océanos adyacentes, como se puede apreciar en la siguiente figura: <br />

![png](../figuras/Figura_dominio_AWS.png)  <br /> _Dominio WRF-Arg delimitado por contorno rojo_

Más detalles de la configuración se pueden encontrar en el siguiente <a href="http://repositorio.smn.gob.ar/handle/20.500.12160/1402" target="_blank">link</a>.
