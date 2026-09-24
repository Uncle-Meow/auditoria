# Auditoría de tarifas Cable Plus

Herramienta web para revisar el próximo cobro de la nómina de abonados antes de facturar. Funciona como un sitio estático: el archivo de nómina se lee y se procesa dentro del navegador, sin enviarlo a un servidor.

## Uso

1. Abra la página web.
2. Si ya existe una configuración, use **Cargar tarifas** y seleccione el JSON guardado.
3. Use **Cargar nómina** y seleccione el Excel o CSV cuyo nombre incluya la fecha `AAAAMMDD`.
4. En la pestaña **Tarifas**, complete el total esperado de cada combinación.
5. Revise las anomalías y clasifique cada caso como **Error confirmado** o **Posible excepción** cuando corresponda.
6. Use **Guardar tarifas** para conservar un JSON reutilizable y **Exportar Excel** para obtener el informe completo y el respaldo de continuidad.

Para retomar un trabajo después de cerrar o actualizar la página, use **Continuar auditoría** y seleccione el último Excel exportado por la herramienta. Se restauran la nómina, las tarifas configuradas, las clasificaciones y las observaciones, y los resultados se recalculan con las reglas vigentes.

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
