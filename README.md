# APP-Compra-granos
## Aplicación realizada con los servicios de google para sistematizar la compra de granos en una empresa.

Este proyecto surge como respuesta a la necesidad de agilizar y simplificar el proceso de ingreso de las "órdenes de venta" diarias realizadas por una empresa que compra granos a distintos productores y acopios. El objetivo es llevar un registro actualizado de la posición diaria de compra, el cual requiere confirmación por parte de la mesa de granos antes de ser cargado en el sistema utilizado por la empresa.

Anteriormente, este proceso se realizaba a través de mensajes de WhatsApp y correos electrónicos, lo que resultaba tedioso y requería estar frente a una computadora. Con esta aplicación, que utiliza la integración de Google Sheets, formularios y App Script, es posible agilizar y dinamizar la operación, permitiendo que varios operadores trabajen simultáneamente en los servicios de Google, algo que no era posible anteriormente debido al uso de un archivo de Excel.

La elección de los servicios de Google se debe a su amplia difusión y facilidad de uso, ya que la mayoría de las personas están familiarizadas con ellos. Además, se ha aprendido a utilizar el lenguaje de App Script para personalizar y optimizar el flujo de trabajo.

Con esta aplicación, los operadores encargados de las compras de granos (sucursales que compran a productores) pueden confirmar los negocios a la mesa de casa central enviando las órdenes a través de un formulario desde cualquier ubicación utilizando sus teléfonos móviles. Estas órdenes de venta se registran en una base de datos central que se actualiza automáticamente y genera un PDF de confirmación del negocio, el cual se envía automáticamente al remitente de la orden de venta con los detalles de la transacción. Este PDF sirve como respaldo de la operación realizada.

Una vez que la orden se ha ingresado en la base de datos, que utiliza una planilla de Google Sheets, los datos se exportan a otra planilla que permite a los operadores diferenciar los distintos tipos de granos comprados, proporcionando detalles específicos de cada transacción. Esto permite listar las órdenes y marcarlas en verde una vez que se hayan ingresado de forma definitiva en el sistema de la empresa para su liquidación.

A continuación, se adjuntan imágenes que ilustran el proceso mencionado.

### Código que utiliza la planilla de Google Sheets para generar el pdf mediante un activador de envío de formulario.

`function myFunction() {
  //definimos las variables del proyecto
  var app = SpreadsheetApp; // accedemos a la aplicación de spreeadsheet
  var ss = app.openById("1dJMujPzzmGfJiB4KHTPFXvYZY516Dot5wQ5nt02q"); //seleccionamos el archivo que vamos a utilizar
  var sheet = ss.getSheetByName("SOJA"); //definimos en cual hoja vamos a trabajar
  var filaInicial = sheet.getLastRow(); //seleccionamos la ultima fila creada para enviar el email y que cada vez que se agregue salga
  var numeroFilas = 1 ; //seleccionamos cuantas filas vamos a procesar
  // @ts-ignore
  var rangoDeDatos = sheet.getRange(filaInicial,2, numeroFilas,10000000); // definimos el rango donde se encuentra la información
  var datos = rangoDeDatos.getValues(); //guardamos toda la información del rango de datos en la variable datos

  var plantillaOv_cargada = "11pA4yKl7_fewoD4_BMOvxm3af_vz8vHHE25RO";
  var carpetaOv_Cargada = '1rQyIGw4H-wN4V0R4-lqlgQukPiBK';

  for (i in datos){
    var columna = datos[i];
    var nombreSucursal = columna[2];
    var nombreCliente = columna[3];
    var nombreEntidad = columna[4];
    var nombreGrano = columna[5];
    var nombreKg = columna[6];
    var nombreCosecha = columna[7];
    var nombreCanjeycobro = columna[8];
    var nombreCondicion = columna[9];
    var nombreMoneda = columna[10];
    var nombrePrecio = columna[11];
    var nombreDesde = columna[12];
    var nombreHasta = columna[13];
    var nombreObservaciones = columna[14];
    var nombreObervacionespizarra = columna[15];
    var numeroContrato = columna[16];
    var nombreEmail = columna[20];
    var nombreId = columna[20];

  };

  var nombreIdcliente = "OV Nro:" + " " + nombreId + "-" + nombreCliente
  
  // buscamos la plantilla de las ov y luego creamos una copia le damos un nombre nuevo y lo guardamos en una carpeta de ordenes de ventas
  DriveApp.getFileById(plantillaOv_cargada).makeCopy('OV Nro:' + nombreId, DriveApp.getFolderById(carpetaOv_Cargada));
  var ids = DriveApp.getFilesByName('OV Nro:' + nombreId);
  while (ids.hasNext()){
    var id = ids.next();
    var id = id.getUrl();

  }

  var plantillaNueva = DocumentApp.openByUrl("'" + id + "'");
  var body = plantillaNueva.getBody(); //estamos dentor del cuerpo de la plantilla
    body.replaceText('%nombreSucursal%', nombreSucursal);
    body.replaceText('%nombreCliente%', nombreCliente);
    body.replaceText('%nombreCanjeycobro%', nombreCanjeycobro);
    body.replaceText('%nombreCosecha%', nombreCosecha);
    body.replaceText('%nombreDesde%', nombreDesde);
    body.replaceText('%nombreHasta%', nombreHasta);
    body.replaceText('%nombreObservaciones%', nombreObservaciones);
    body.replaceText('%nombreMoneda%', nombreMoneda);
    body.replaceText('%nombreKg%', nombreKg);
    body.replaceText('%nombrePrecio%', nombrePrecio);
    body.replaceText('%nombreGrano%', nombreGrano);
    body.replaceText('%nombreCondicion%', nombreCondicion);
    body.replaceText('%nombreId%', nombreId);
  plantillaNueva.saveAndClose();
  `
  
  
  ### Formulario desde el que se ingresan las ordenes de venta.
  
  [![screencapture-docs-google-forms-d-e-1-FAIp-QLSe-Db-Bu-Xd-KX320-F6m-VFNOJ2-W5o-Gv-Vlj0w7-A1s4l-ZMJJwix-Rgk-A-viewfor.png](https://i.postimg.cc/MZykn1Y2/screencapture-docs-google-forms-d-e-1-FAIp-QLSe-Db-Bu-Xd-KX320-F6m-VFNOJ2-W5o-Gv-Vlj0w7-A1s4l-ZMJJwix-Rgk-A-viewfor.png)](https://postimg.cc/MXK34M4d)
  
  ### PDF que importa datos del formulario ingresado y se genera en base a lo ingresado
  
  [![Orden-de-Venta.png](https://i.postimg.cc/Fz1JBhp0/Orden-de-Venta.png)](https://postimg.cc/8fQ5FQqC)
  
  ### Ejemplo del PDF. Cada pdf generado es guardado en la nube de Google Drive, dividido por mes en el que ingresa.
  
  [![Orden-de-Venta-Prueba-Mayo2023.png](https://i.postimg.cc/TPFw3nJ5/Orden-de-Venta-Prueba-Mayo2023.png)](https://postimg.cc/GBPrM8Nb)
  
  ### Imagen de base de datos general donde ingresan las ordenes.
  
  [![Screenshot-2023-05-04-150458.png](https://i.postimg.cc/593VPwQ5/Screenshot-2023-05-04-150458.png)](https://postimg.cc/PNP9Jv4N)
  
  ### Planilla que utilizan los operadores para confirmar y cargar al sistema de la empresa.
  
  La fórmula utilizada para importar datos de la planilla madre es: 
  
  =QUERY(IMPORTRANGE("1d3G1-0n--E0X8DmjO8rKNtLKWE00XcujYTUR";"ov_hab!$A:$P");"SELECT * WHERE Col6='Soja'";1)
  
  [![Screenshot-2023-05-04-150912.png](https://i.postimg.cc/3xZfhB79/Screenshot-2023-05-04-150912.png)](https://postimg.cc/xXc3yLXb)
  
  
  Por privacidad de la empresa las columnas de Cliente y Sucursal fueron eliminadas.
  El operador solo tiene permisos para modificar la columna "A", la cual como se puede observar en la imagen permite tildar cuando esta se encuentra en estado "Confirmado".
  
