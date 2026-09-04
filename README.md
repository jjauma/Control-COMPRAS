# Precios de compra · Milgrup

Panel para seguir la evolución de precios y volúmenes de compra por proveedor.
Funciona igual que el dashboard de Milcugat: una sola página web, sin servidor ni
base de datos, alojada en GitHub Pages.

## Qué contiene el repositorio

| Fichero | Para qué sirve |
|---|---|
| `index.html` | La aplicación entera. Es el único fichero de programa. |
| `datos.json` | Todas las compras. Es el que se actualiza cada mes. |
| `README.md` | Estas instrucciones. |

De partida vienen cargadas 5.296 compras entre diciembre de 2025 y septiembre de
2026, por 147.220 € y 502 referencias: el histórico completo de Makro de
Milburguers y Cirera 23, más las facturas de Balart Subirana, Sasa Alimentació,
Merka Pizza, Copral, Prodesco, Coca-Cola, Empuriagel, Hielo Polar, Escolà,
Panoteca y Arcon Food.

Dos avisos sobre la cobertura de esos datos de partida:

- De **Balart Subirana** están cargadas las 16 referencias de más peso, que suman
  entre el 74 % y el 80 % del importe de cada factura. El resto del catálogo, muy
  fragmentado, quedó fuera. Los totales de ese proveedor están por tanto
  incompletos; los precios que sí figuran son correctos.
- Las facturas de **Sasa Alimentació** solo listan albaranes con su importe, sin
  detalle de producto. No hay precios que analizar, así que su gasto mensual entra
  como una sola línea llamada «Compra del mes». Aparecerá en el gasto por
  proveedor pero nunca en las variaciones de precio.

## Ponerlo en marcha

1. Entra en [github.com/new](https://github.com/new) y crea un repositorio llamado
   `precios-compras`.
2. Elige **Private** si no quieres que tus precios de compra sean públicos. Ojo:
   GitHub Pages sobre repositorios privados requiere plan de pago. Si lo dejas
   público, cualquiera que dé con la dirección verá los datos.
3. Pulsa **uploading an existing file** y arrastra los tres ficheros.
4. Abajo del todo, **Commit changes**.
5. Ve a **Settings → Pages**. En «Source» elige `Deploy from a branch`, rama `main`
   y carpeta `/ (root)`. Guarda.
6. Espera un par de minutos. Tu panel estará en
   `https://jjauma.github.io/precios-compras`.

## Cambiar el código de acceso

Está escrito en claro dentro de `index.html`. Ábrelo en GitHub, pulsa el lápiz
para editarlo y busca cerca del final:

```js
const PIN = "1234";          // cámbialo por el tuyo
const PEDIR_PIN = true;      // ponlo en false para quitar la pantalla de acceso
```

Cambia `1234` por el código que quieras y guarda.

**Esto no es seguridad.** El código va escrito en la propia página y cualquiera
puede leerlo o abrir `datos.json` directamente. Sirve para que nadie entre por
descuido, nada más. Si los precios de compra te preocupan de verdad, el
repositorio tiene que ser privado.

## Actualizar los datos cada mes

El panel no guarda nada por su cuenta: lo que importes vive solo en la pestaña que
tengas abierta. Para que quede guardado hay que subir el fichero a GitHub.

1. Descarga del portal de Makro el fichero del mes, para cada sociedad.
2. Abre el panel, ve a **Datos** y arrastra los ficheros a la caja.
3. Comprueba en el registro cuántas compras se han añadido. Las que ya estaban se
   descartan solas, así que puedes volver a subir un mes sin duplicar nada.
4. Pulsa **Descargar datos.json**.
5. En GitHub, entra en `datos.json`, pulsa el lápiz, borra el contenido y pega el
   del fichero nuevo. O más fácil: **Add file → Upload files** y arrastra el nuevo
   `datos.json` encima del anterior. **Commit changes**.

### Proveedores que no son Makro

Copral, Merka o Prodesco mandan las facturas en PDF y no hay forma de leerlas
automáticamente. Para esos, en **Datos** tienes **Descargar plantilla CSV**: la
abres con Numbers o Excel, rellenas una línea por producto y precio, la guardas
como CSV y la arrastras igual que las de Makro.

Las columnas son: `proveedor, sociedad, fecha, factura, referencia, descripcion,
unidad, familia, precio, cantidad, importe`. Con proveedor, fecha, referencia,
precio y cantidad ya funciona; el resto es opcional.

Como esas facturas son mensuales, basta una línea por referencia con la cantidad
total del mes. Si un producto aparece a dos precios distintos en la misma factura,
pon dos líneas.

## Las cinco pestañas

**Qué se ha movido** es la pantalla principal. Compara el primer y el último precio
pagado dentro del periodo filtrado y ordena por impacto: lo que más te cuesta o te
ahorra al año, arriba. El impacto multiplica la diferencia de precio por el volumen
medio mensual y por doce, así que es un orden de magnitud para priorizar, no una
cifra exigible a nadie.

**Ficha de producto** es el histórico de una referencia: precio mes a mes, unidades
compradas y el listado de todas las compras con fecha, centro y número de factura.
Es la pestaña de la que salió la reclamación a Makro por Barberá.

**Comparar proveedores** enfrenta productos equivalentes servidos por más de uno.
No son idénticos —cambian marca y formato—, así que conviene catarlos antes de
mover nada. Necesita el filtro de proveedor en «Todos».

**Volumen y gasto** es la foto de conjunto: gasto por mes, por proveedor y por
familia de producto.

**Datos** es donde se importa, se exporta y se ve qué hay cargado.

## Detalles que conviene saber

En la exportación de Makro, la columna «Cantidad» a veces son cajas y no kilos: en
el pollo o la picanha, `precio × cantidad` no da el importe. El panel deriva la
cantidad dividiendo el importe entre el precio, de forma que siempre cuadre y que
los kilos de la ficha de producto sean kilos de verdad.

Los abonos entran como cantidades negativas. Buena parte no son devoluciones de
producto sino correcciones de caja el mismo día, que se compensan solas.

Las equivalencias entre proveedores están escritas a mano dentro de `datos.json`,
en el apartado `equivalencias`. Para añadir una nueva hay que editar el fichero, y
es la única parte que pide tocar código.

## Si algo va mal

**La página pide el código y no pasa de ahí.** El código por defecto es `1234`. Si
lo cambiaste y no lo recuerdas, míralo en `index.html`.

**Abro `index.html` y no veo ningún dato.** Es lo normal si has hecho doble clic en
el fichero. Por seguridad, el navegador no deja que una página abierta desde el
disco lea ficheros vecinos, así que no encuentra `datos.json`. Tienes dos salidas:

- La buena: subirlo a GitHub y abrirlo desde la dirección de Pages.
- La rápida, para mirarlo sin subir nada: entra en la pestaña **Datos** y arrastra
  tu `datos.json` a la caja. Se carga al momento.

**Sale «No se ha encontrado datos.json».** Además de lo anterior, comprueba que el
fichero se llame exactamente así y esté en la misma carpeta que `index.html`.

**Quiero probarlo en el Mac antes de subirlo.** Abre Terminal, escribe `cd ` (con
el espacio), arrastra la carpeta a la ventana y pulsa Enter. Luego:

```
python3 -m http.server 8000
```

Y abre `http://localhost:8000` en el navegador. Para pararlo, Control-C.

**Al arrastrar un fichero dice que el formato no se reconoce.** El panel espera una
exportación de Makro con sus columnas originales, o un CSV con las de la plantilla.
Si has abierto y guardado el `.ods` desde Numbers, puede que hayan cambiado las
cabeceras: vuelve a descargarlo del portal de Makro sin tocarlo.

**Los gráficos no aparecen.** Necesita conexión para cargar las tipografías y la
librería que lee los ficheros. Recarga la página.
