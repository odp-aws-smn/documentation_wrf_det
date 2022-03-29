# Acceso a los datos

Para acceder a los datos se puede usar los siguiente:

**vía URL**<br />
Los archivos pueden ser descargados directamente accediendo al siguiente link: https://smn-ar-wrf.s3-us-west-2.amazonaws.com/index.html

**AWS CLI**<br />
Los datos de S3 se pueden descargar utilizando AWS CLI. Para más información sobre su instalación visitar el siguiente 
[link](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).<br />
Using the object keys described above, the following bash script downloads a shapefile (2020-01-01_performance_fixed_tiles.zip) for fixed performance tiles aggregated over Q1 2020 using aws s3 cp.
#!/usr/bin/env bash
export FORMAT='shapefiles' # (shapefiles|parquet)
export TYPE='fixed'        # (fixed|mobile)
export YYYY='2020'         # 2019,2020,2021
export Q='1'               # 1,2,3,4 (to date)

aws s3 cp s3://ookla-open-data/${FORMAT}/performance/type=${TYPE}/year=${YYYY}/quarter=${Q}/ . \
--recursive \
--no-sign-request
To download the 2020 Q3 mobile and fixed time series datasets, we can also specify the full S3 URI to download the objects:
#!/usr/bin/env bash

Mobile 2020 Q3
aws s3 cp s3://ookla-open-data/parquet/performance/type=mobile/year=2020/quarter=3/2020-07-01_performance_mobile_tiles.parquet --no-sign-request

Fixed 2020 Q3
aws s3 cp s3://ookla-open-data/parquet/performance/type=fixed/year=2020/quarter=3/2020-07-01_performance_fixed_tiles.parquet --no-sign-request

**Python**<br />
Utilizando la librería [s3sf](https://pypi.org/project/s3fs/) <br />
Por ejemplo: <br />
import s3fs <br />
s3_file = 's3://smn-ar-wrf/DATA/WRF/DET/2022/03/21/00/WRFDETAR_01H_20220321_00_000.nc'   # nombre del archivo a descargar <br />
fs = s3fs.S3FileSystem(anon=True)<br />
fs.get(s3_file)<br />

**R**<br />
El paquete de R [ooklaOpenDataR](https://github.com/teamookla/ooklaOpenDataR) provee funciones para acceder y trabajar con los datos. ME PARECE QUE ESTO NO NOS SIRVE ....<br />

VER SI LA LIBRERIA AWS.S3 SIRVE: (https://cloud.r-project.org/web/packages/aws.s3/aws.s3.pdf)<br />
library(asw.s3)<br />
b <- bucketlist()<br />
get_bucket(b[1,1])<br />
get_bucket_df(b[1,1])<br />

opcion con libreria paws: (https://paws-r.github.io/)<br />
library("aws.s3")<br />
library("paws")<br />
paws::s3(config = list(endpoint = "myendpoint"))<br />
mycsv_raw <- s3$get_object(Bucket = "mybucket", key="myfile.csv")<br />
