# Acceso a los datos

Los datos se encuentran disponibles en el **portal de AWS**: <a href="https://registry.opendata.aws/smn-ar-wrf-dataset/" target="_blank">https://registry.opendata.aws/smn-ar-wrf-dataset/</a>.

La descarga de los datos se puede realizar de las siguientes maneras:

**Vía URL**<br />
Los archivos pueden ser descargados directamente accediendo al siguiente link: <a href="https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html" target="_blank">https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html</a>.<br />
Los datos se almacenan utilizando el Amazon Simple Storage Service (S3). Para más información sobre esta herramienta visitar <a href="https://docs.aws.amazon.com/es_es/AmazonS3/latest/userguide/Welcome.html" target="_blank">https://docs.aws.amazon.com/es_es/AmazonS3/latest/userguide/Welcome.html</a>.

**AWS CLI**<br /> 
Los datos se pueden descargar utilizando AWS CLI. Para más información sobre su instalación visitar el siguiente 
<a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" target="_blank">link</a>.<br />
A continuación se muestra, a modo de ejemplo, la descarga de un archivo individual y de todo el directorio de un día:

```bash
#!/usr/bin/env bash

# Se descarga el archivo del plazo 02 UTC del ciclo 00 UTC del día 21 de marzo 2022 al directorio_salida:
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_002.nc directorio_salida

# Se descarga todos los plazos del ciclo 00 UTC del día 21 de marzo 2022 al directorio_salida:
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/ --recursive directorio_salida
```

**Python**<br />
Para descargar los archivos se utiliza la librería <a href="https://pypi.org/project/s3fs/" target="_blank">s3fs</a>. <br />
A continuación se muestra, a modo de ejemplo, la descarga del archivo de un día:

```python
import s3fs
# Se descarga el archivo del ciclo 00 UTC del día 21 de marzo 2022 para el plazo 0 
s3_file = 's3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_000.nc' 
fs = s3fs.S3FileSystem(anon=True)
data = fs.get(s3_file)
```

