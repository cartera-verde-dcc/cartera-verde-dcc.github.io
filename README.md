# Sitio web del proyecto "Cartera verde de proyectos financiables alineados con la NDC"

## Procesamiento de los datos de entrada
Los programas reciben como entrada un archivo CSV, el cual se generó a partir de un archivo Excel llamado `datos/PRODUCTO 3 BASE DE DATOS de Proyectos Verdes MANUEL VARGAS.xlsx`. Este archivo Excel proviene de una exportación de los datos recolectados mediante un formulario Google Forms. Tiene dos filas en blanco al inicio. En la fila 3, hay solamente tres columnas con encabezado:
- AE (31): "Latitud"
- AF (32): "Longitud"
- AO (41): "Cantones en los que está presente la iniciativa"

El archivo Excel tiene **200 filas y 121 columnas** de datos.

### Procedimiento para generar el archivo CSV
- El archivo Excel se abrió en LibreOffice Calc.
- Con la opción de menú **File | Save As...**, el archivo se guardó en formato CSV con el nombre `datos/proyectos.csv` (*Edit Filter Settings = Character set:Unicode(UTF-8) Field delimiter:, String delimiter:" Save content cell as shown*).
- Se eliminaron las tres primeras filas (incluyendo la que tiene los encabezados) y se guardó nuevamente el archivo.

Para asignar nombres temporales a las 121 columnas, se abrió con un editor de texto el archivo `datos/proyectos.csv` y se insertó al principio la línea:

`c001,c002,c003,c004,c005,c006,c007,c008,c009,c010,c011,c012,c013,c014,c015,c016,c017,c018,c019,c020,c021,c022,c023,c024,c025,c026,c027,c028,c029,c030,c031,c032,c033,c034,c035,c036,c037,c038,c039,c040,c041,c042,c043,c044,c045,c046,c047,c048,c049,c050,c051,c052,c053,c054,c055,c056,c057,c058,c059,c060,c061,c062,c063,c064,c065,c066,c067,c068,c069,c070,c071,c072,c073,c074,c075,c076,c077,c078,c079,c080,c081,c082,c083,c084,c085,c086,c087,c088,c089,c090,c091,c092,c093,c094,c095,c096,c097,c098,c099,c100,c101,c102,c103,c104,c105,c106,c107,c108,c109,c110,c111,c112,c113,c114,c115,c116,c117,c118,c119,c120,c121`
