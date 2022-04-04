# Acceso a los datos

Los datos se encuentran disponibles en el **portal de AWS**: [https://registry.opendata.aws/smn-ar-wrf/](https://registry.opendata.aws/smn-ar-wrf/)

La descarga de los datos se puede realizar de las siguientes maneras:

**vía URL**<br />
Los archivos pueden ser descargados directamente accediendo al siguiente link: [https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html](https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html)

**AWS CLI**<br /> 
Los datos de S3 se pueden descargar utilizando AWS CLI. Para más información sobre su instalación visitar el siguiente 
[link](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).<br />
Por ejemplo, para descargar el archivo del plazo 02 UTC del ciclo 00 UTC del día 21 de marzo 2022 ejecutar la siguiente línea: 
```bash
#!/usr/bin/env bash
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_002.nc directorio_salida
```
Para descargar todos los plazos del ciclo 00 UTC del día 21 de marzo 2022:
```bash
#!/usr/bin/env bash
aws s3 cp --no-sign-request s3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/ --recursive directorio_salida
```

**Python**<br />
Utilizando la librería [s3sf](https://pypi.org/project/s3fs/) <br />
Por ejemplo, para descargar los archivos del ciclo 00 UTC del día 21 de marzo 2022 ejecutar las siguientes líneas: <br />
```python
import s3fs
s3_file = 's3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_000.nc'   # nombre del archivo a descargar 
fs = s3fs.S3FileSystem(anon=True)
data = fs.get(s3_file)
```

