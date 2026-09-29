# Auditoría de tarifas Cable Plus

Herramienta web para revisar el próximo cobro de la nómina de abonados antes de facturar. Funciona como un sitio estático: el archivo de nómina se lee y se procesa dentro del navegador, sin enviarlo a un servidor.

## Uso

1. Abra la página web.
2. Si ya existe una configuración, use **Cargar tarifas** y seleccione el JSON guardado. Ese único archivo recupera las tarifas y los precios aprobados.
3. Use **Cargar nómina** y seleccione el Excel o CSV cuyo nombre incluya la fecha `AAAAMMDD`.
4. Si ya había trabajado con una nómina anterior, responda **Sí, seleccionar Excel anterior** y cargue el último Excel exportado por la herramienta.
5. Revise las pestañas y filtros de comparación: **Sin cambios**, **Cambiaron**, **Clientes nuevos** y **Ya no aparecen**.
6. En la pestaña **Tarifas**, complete el total esperado de cada combinación.
7. Revise las anomalías usando las tres casillas: **Error SCORD**, **Error programa** y **Precio detectado correcto**.
8. Use **Guardar tarifas** para conservar en un solo JSON las tarifas y las aprobaciones, y **Exportar Excel** para obtener el informe completo.

Para retomar un trabajo después de cerrar o actualizar la página, use **Continuar auditoría** y seleccione el último Excel exportado por la herramienta. Se restauran la nómina, las tarifas configuradas, la lista completa de precios aprobados, las clasificaciones y las observaciones, y los resultados se recalculan con las reglas vigentes.

Al comparar una nómina nueva con una auditoría anterior, la herramienta usa el número de abonado. Si sus estados, planes, fechas, alquileres, adicionales y montos no cambiaron, recupera la clasificación y la observación anterior. Si algún dato importante cambió, el abonado vuelve a quedar pendiente y se muestra la diferencia. Los abonados que no existían se marcan como nuevos y los que desaparecieron se incluyen en una tabla separada.

**Precio detectado correcto** significa que la suma de TV + internet + alquiler + adicionales calculada por la herramienta es correcta para ese abonado. La aprobación se recuerda únicamente por número de abonado y monto. Si el mismo abonado conserva ese monto en otra auditoría, se reconoce automáticamente; si cambia, vuelve a aparecer como anomalía. Esta clasificación no elimina otros problemas de la misma línea, como alquiler o fecha.

Las tres clasificaciones manuales son excluyentes: **Error SCORD**, **Error programa** y **Precio detectado correcto**. Al marcar una, se desmarcan las otras para evitar decisiones contradictorias. La tabla principal mantiene visibles la búsqueda, el filtro y los encabezados mientras se recorren las filas.

Un precio próximo sin fecha no genera una anomalía por sí solo y se continúa utilizando el precio vigente. Sí se conservan como anomalías un próximo plan sin fecha, una fecha fuera del mes auditado, una diferencia de total y cualquier otro problema real.

Un servicio en estado `RETIRADO` aporta ₡0 al total y no genera una anomalía por ese motivo. El monto original permanece visible como evidencia. El código `SOLO CATV FTTH 102-129`, aunque aparezca en la columna de internet, se clasifica como televisión solamente. Con precio de internet en ₡0 es correcto; si trae un monto mayor a ₡0, se muestra como anomalía y ese monto no se suma al cobro.

El JSON de tarifas es un respaldo único. Contiene la configuración de planes y montos y, para cada precio aprobado, exclusivamente `abonado` y `monto`. No contiene nombres de clientes ni filas completas de la nómina. Debe resguardarse como información interna porque el número de abonado identifica al cliente.

El Excel de auditoría incluye las hojas **Precios aprobados**, **Cambios desde anterior**, **Clientes nuevos** y **Ya no aparecen**. Por eso **Continuar auditoría** puede recuperar todas las aprobaciones, decisiones y resultados de la comparación, además de la nómina y sus revisiones. El Excel debe resguardarse como información empresarial. La lectura, comparación y restauración se realizan localmente en el navegador; ningún archivo se envía a un servidor.

## Desarrollo local

Requiere Node.js 22.13 o superior.

```sh
npm ci
npm run build:pages
```

El sitio estático se genera en `dist-pages/`. La compilación original para Sites sigue disponible con `npm run build`.

## Publicación

El flujo `.github/workflows/deploy-pages.yml` compila y publica automáticamente en GitHub Pages cada vez que se envían cambios a la rama `main`. En el repositorio de GitHub debe seleccionarse **GitHub Actions** como origen de Pages.
