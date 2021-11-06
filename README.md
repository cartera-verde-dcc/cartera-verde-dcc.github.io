# Aplicación web del proyecto "Cartera verde de proyectos financiables alineados con la NDC"
Esta es una aplicación web para consultar y explorar los datos generados por el proyecto "Cartera verde de proyectos financiables alineados con la NDC". Fue desarrollada con los paquetes [flexdasboard](https://pkgs.rstudio.com/flexdashboard/) y [Shiny](https://shiny.rstudio.com/) del lenguaje de programación [R](https://www.r-project.org/).

La aplicación está disponible en la dirección:  
[Cartera verde de proyectos financiables alineados con la NDC]()

## Procesamiento de los datos de entrada
La aplicación recibe tres archivos de entrada:
- proyectos.csv
- cantones.geojson
- regiones-mideplan.geojson

### proyectos.csv
Este archivo CSV se generó a partir de un archivo Excel llamado `datos/PRODUCTO 3 BASE DE DATOS de Proyectos Verdes vNov01.xlsx` (las versiones anteriores de este archivo están en `datos/bak/`). Este archivo Excel proviene de una exportación de los datos recolectados mediante un formulario Google Forms. Tiene dos filas en blanco al inicio. En la fila 3, hay varias columnas con encabezado o que se identificaron como importantes:
- 31: "Latitud"
- 32: "Longitud"
- 33: "Nombre del proyecto o iniciativa de cambio climático" 
- 34: "Objetivo del proyecto"
- 35: "Descripción de la actividad o proyecto"
- 39: "Principal tema que atiende el proyecto"
- 40: "Regiones (de Mideplan) de influencia del proyecto o iniciativa"
- 41: "Cantones de influencia del proyecto o iniciativa"
- 42: "Tema de acción climática"
- 43: "Subtema de acción climática"
- 44: "Mitigación y adaptación"
- 58: "Justificación áreas de acción de las NDC"
- 69: "Justificación de los Ejes seleccionados del Plan Nacional de Descarbonización"
- 76: "Justificación de los Ejes seleccionados de la Política Nacional de Adaptación"

El archivo Excel tiene **200 filas y 121 columnas** de datos.

#### Procedimiento para generar el archivo CSV
- El archivo Excel se abrió en LibreOffice Calc.
- Con la opción de menú **File | Save As...**, el archivo se guardó en formato CSV con el nombre `datos/proyectos.csv` (*Edit Filter Settings = Character set:Unicode(UTF-8) Field delimiter:, String delimiter:" Save content cell as shown*) (las versiones anteriores de este archivo están en `datos/bak/`).
- Se eliminaron las tres primeras filas (incluyendo la que tiene los encabezados) y se guardó nuevamente el archivo.
- En la fila 10, columna 32; fila 133, columna 32 y fila 135, columna 32 se cambió una coma por un punto y se guardó nuevamente el archivo.

Para asignar nombres temporales a las 121 columnas, se abrió con un editor de texto el archivo `datos/proyectos.csv` y se insertó al principio la línea:

`c001,c002,c003,c004,c005,c006,c007,c008,c009,c010,c011,c012,c013,c014,c015,c016,c017,c018,c019,c020,c021,c022,c023,c024,c025,c026,c027,c028,c029,c030,c031,c032,c033,c034,c035,c036,c037,c038,c039,c040,c041,c042,c043,c044,c045,c046,c047,c048,c049,c050,c051,c052,c053,c054,c055,c056,c057,c058,c059,c060,c061,c062,c063,c064,c065,c066,c067,c068,c069,c070,c071,c072,c073,c074,c075,c076,c077,c078,c079,c080,c081,c082,c083,c084,c085,c086,c087,c088,c089,c090,c091,c092,c093,c094,c095,c096,c097,c098,c099,c100,c101,c102,c103,c104,c105,c106,c107,c108,c109,c110,c111,c112,c113,c114,c115,c116,c117,c118,c119,c120,c121`

#### Cambios realizados con R (en `index.Rmd`)
- Se eliminaron los espacios sobrantes en los nombres de los cantones.
- Se reemplazó "Perez Zeledón" con "Pérez Zeledón".
- Se reemplazó "Talmanca" con "Talamanca".

#### Pendientes
- Monteverde no está en el mapa de cantones del IGN.
- En `datos/proyectos.csv` hay tres filas para `(cedula, nombre)` = `(3002045043, "Asociación Centro Científico Tropical")`.

Además,

- ¿Cuáles columnas debe incluírse en el conjunto de datos?

### cantones.geojson
```shell
$ ogr2ogr \
    -simplify 100 \
    -makevalid \
    cantones-simplificadas_100m.geojson \
    WFS:"http://geos.snitcr.go.cr/be/IGN_5/wfs" "IGN_5:limitecantonal_5k" 
```

### regiones-mideplan.geojson
```shell
# Borrado de archivos generados en ejecuciones anteriores
rm regiones-mideplan.geojson

# Archivo temporal
cp regiones-mideplan-atlas2014/regiones_mideplan.* .

# Reubicación de polígonos en regiones
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'CENTRAL' WHERE REGION = 'CARTAGO' OR (REGION = 'HEREDIA' AND NCANTON <> 'SARAPIQUI')" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'HUETAR ATLANTICA' WHERE NDISTRITO = 'HORQUETAS'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'HUETAR NORTE' WHERE NCANTON = 'SARAPIQUI' AND NDISTRITO <> 'HORQUETAS'" regiones_mideplan.shp

# Renombramiento de regiones
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Brunca' WHERE REGION = 'BRUNCA'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Central' WHERE REGION = 'CENTRAL'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Chorotega' WHERE REGION = 'CHOROTEGA'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Huetar Caribe' WHERE REGION = 'HUETAR ATLANTICA'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Huetar Norte' WHERE REGION = 'HUETAR NORTE'" regiones_mideplan.shp
ogrinfo -dialect sqlite -sql "UPDATE regiones_mideplan SET REGION = 'Pacífico Central' WHERE REGION = 'PACIFICO CENTRAL'" regiones_mideplan.shp

# Unión de polígonos en regiones
ogr2ogr \
  -dialect sqlite -sql "SELECT ST_Union(geometry), REGION FROM regiones_mideplan WHERE REGION IS NOT NULL GROUP BY REGION" \
  -t_srs EPSG:4326 \
  -simplify 100 \
  -makevalid \
  regiones-mideplan.geojson \
  regiones_mideplan.shp \
  -nln regiones-mideplan

# Borrado del archivo temporal  
rm regiones_mideplan.*
```
