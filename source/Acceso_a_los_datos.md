# Acceso a los datos

Los datos se encuentran disponibles en el **portal de AWS**: [https://registry.opendata.aws/smn-ar-wrf-dataset/](https://registry.opendata.aws/smn-ar-wrf-dataset/).

La descarga de los datos se puede realizar de las siguientes maneras:

**Vía URL**<br />
Los archivos pueden ser descargados directamente accediendo al siguiente link: [https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html](https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html).
Los datos se almacenan utilizando el Amazon Simple Storage Service (S3). Para más información sobre esta herramienta se pueden visitar https://docs.aws.amazon.com/es_es/AmazonS3/latest/userguide/Welcome.html.

**AWS CLI**<br /> 
Los datos se pueden descargar utilizando AWS CLI. Para más información sobre su instalación visitar el siguiente 
[link](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).<br />
A continuación se muestra, a modo de ejemplo, la descarga de un archivo individual y de todo el directorio de un día:

```bash
#!/usr/bin/env bash

# Se descarga el archivo del plazo 02 UTC del ciclo 00 UTC del día 21 de marzo 2022 al directorio_salida:
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_002.nc directorio_salida

# Se descarga todos los plazos del ciclo 00 UTC del día 21 de marzo 2022 al directorio_salida:
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/ --recursive directorio_salida
```

**Python**<br />
Para descargar los archivos se utiliza la librería [s3sf](https://pypi.org/project/s3fs/). <br />
A continuación se muestra, a modo de ejemplo, la descarga del archivo de un día:


```python
import s3fs
# Se descarga el archivo del ciclo 00 UTC del día 21 de marzo 2022 para el plazo 0 
s3_file = 's3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_000.nc' 
fs = s3fs.S3FileSystem(anon=True)
data = fs.get(s3_file)
```

