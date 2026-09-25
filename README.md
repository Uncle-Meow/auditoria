# Auditoría de tarifas Cable Plus

Herramienta web para revisar el próximo cobro de la nómina de abonados antes de facturar. Funciona como un sitio estático: el archivo de nómina se lee y se procesa dentro del navegador, sin enviarlo a un servidor.

## Uso

1. Abra la página web.
2. Si ya existe una configuración, use **Cargar tarifas** y seleccione el JSON guardado.
3. Use **Cargar nómina** y seleccione el Excel o CSV cuyo nombre incluya la fecha `AAAAMMDD`.
4. En la pestaña **Tarifas**, complete el total esperado de cada combinación.
5. Revise las anomalías usando las tres casillas: **Error SCORD**, **Error programa** y **Precio detectado correcto**.
6. Use **Guardar tarifas** para conservar un JSON reutilizable y **Exportar Excel** para obtener el informe completo y el respaldo de continuidad.

Para retomar un trabajo después de cerrar o actualizar la página, use **Continuar auditoría** y seleccione el último Excel exportado por la herramienta. Se restauran la nómina, las tarifas configuradas, los precios detectados correctos, las clasificaciones y las observaciones, y los resultados se recalculan con las reglas vigentes.

**Precio detectado correcto** significa que la suma de TV + internet + alquiler + adicionales calculada por la herramienta es correcta para ese abonado y que el valor **Esperado** ingresado como referencia debe investigarse. No modifica automáticamente la tarifa configurada. La diferencia se ignora en futuras nóminas únicamente cuando coinciden el abonado, la combinación de planes y el mismo precio; si alguno cambia, el cliente vuelve a revisión. Esta casilla es independiente de **Error SCORD** y **Error programa**, para que otros problemas de la misma línea —como alquiler o fecha— sigan siendo auditables.

El JSON de tarifas contiene únicamente identificadores de combinaciones, nombres de planes, categorías y montos. No contiene abonados, nombres de clientes ni filas de la nómina.

El Excel de auditoría sí contiene la nómina y debe resguardarse como información empresarial. La lectura y la restauración se realizan localmente en el navegador; el archivo no se envía a un servidor.

## Desarrollo local

Requiere Node.js 22.13 o superior.

```sh
npm ci
npm run build:pages
```

El sitio estático se genera en `dist-pages/`. La compilación original para Sites sigue disponible con `npm run build`.

## Publicación

El flujo `.github/workflows/deploy-pages.yml` compila y publica automáticamente en GitHub Pages cada vez que se envían cambios a la rama `main`. En el repositorio de GitHub debe seleccionarse **GitHub Actions** como origen de Pages.
