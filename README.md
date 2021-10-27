# Sitio web del proyecto "Cartera verde de proyectos financiables alineados con la NDC"

## Procesamiento de los datos de entrada
Los programas reciben como entrada un archivo CSV, el cual se generó a partir de un archivo Excel llamado `datos/PRODUCTO 3 BASE DE DATOS de Proyectos Verdes MANUEL VARGAS.xlsx`. Este archivo Excel proviene de una exportación de los datos recolectados mediante un formulario Google Forms. Tiene dos filas en blanco al inicio. En la fila 3, hay tres columnas con encabezado:
- AE (31): "Latitud"
- AF (32): "Longitud"
- AO (41): "Cantones en los que está presente la iniciativa"

### Procedimiento para generar el archivo CSV
- El archivo Excel se abrió en LibreOffice Calc.
- Con la opción de menú **File | Save As...**, el archivo se guardó en formato CSV con el nombre `datos/proyectos.csv` (*Edit Filter Settings = Character set:Unicode(UTF-8) Field delimiter:, String delimiter:" Save content cell as shown*).
- Se eliminaron las tres primeras filas (incluyendo la que tiene los encabezados) y se guardó nuevamente el archivo.
