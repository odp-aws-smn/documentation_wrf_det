# Información general

El SMN-Arg corre en forma operativa el modelo regional de alta resolución Weather Research and Forecasting Model (WRF) en su versión con núcleo dinámico Advanced Research WRF (ARW) version 4.0 ([Skamarock et al. 2019](https://www2.mmm.ucar.edu/wrf/users/docs/technote/v4_technote.pdf){:target="_blank"}). El modelo resuelve la convección en forma explícita con paso de tiempo variable y fue configurado utilizando las siguientes parametrizaciones: <br />
- Microfísica: WSM6 (un momento - 6 clases)
- Radiación onda larga: RRTM
- Radiación onda corta: Dudhia
- Capa Límite Planetaria: MYJ (Mellor, Yamada, Janjic)
- Modelo de suelo: NOAH, 4 capas (0-10 cm, 10-40 cm, 40-100 cm, 1-2 m)

Los pronósticos horarios generados con el modelo WRF-Arg cuentan con una resolución horizontal de 4 km con 45 niveles verticales (tope 10 hPa) y plazo máximo de 72 horas. Los mismos se inicializan con los análisis y pronósticos horarios del NCEP-NOAA Global Forecasting System Model ([GFS](https://www.emc.ncep.noaa.gov/emc/pages/numerical_forecast_systems/gfs.php)) de 0,25° de resolución horizontal.<br />

La proyección de los datos es Conforme de Lambert y el dominio abarca todo Argentina como se puede apreciar en la siguiente figura: <br />

![png](../figuras/dominioWRF4.png)  <br /> *Dominio WRF-Arg*

Más detalles de la configuración se pueden encontrar en el siguiente [link](http://repositorio.smn.gob.ar/handle/20.500.12160/1402).
