# Guia de repaso DIW: CSS, layouts, formularios y responsive

## 1. Box model

Cada elemento es una caja formada por:

```text
margin -> border -> padding -> content
```

- `content`: donde va el texto, imagen, input, etc.
- `padding`: espacio interior entre contenido y borde.
- `border`: borde de la caja.
- `margin`: espacio exterior, separa la caja de otras cajas.

### `box-sizing: content-box`

Es el valor clasico por defecto.

```css
.caja {
  box-sizing: content-box;
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
```

Ancho real:

```text
200 + 20 + 20 + 5 + 5 = 250px
```

El `width` solo mide el contenido.

### `box-sizing: border-box`

Mucho mas comodo para maquetar.

```css
.caja {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid black;
}
```

Ancho real:

```text
200px
```

El contenido se hace mas pequeno para que dentro quepan padding y border.

### Regla habitual

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Asi todos los calculos son mas previsibles.

## 2. Calcular medidas

Formula mental si usas `content-box`:

```text
ancho total = width + padding izquierda/derecha + border izquierda/derecha + margin izquierda/derecha
```

Formula mental si usas `border-box`:

```text
ancho total = width + margin izquierda/derecha
```

El `margin` siempre queda fuera de la caja. No entra ni en `content-box` ni en `border-box`.

### Porcentajes

```css
.hijo {
  width: 50%;
}
```

Ese `50%` se calcula tomando como referencia el ancho disponible del contenedor padre. Si el padre tiene padding, la caja hija vive dentro del area de contenido del padre, no encima del padding.

Ejemplo:

```css
.padre {
  width: 800px;
  padding: 40px;
}

.hijo {
  width: 50%;
}
```

Si el padre usa `content-box`, su contenido mide 800px, asi que el hijo mide 400px. El padding del padre queda alrededor.

## 3. Display

### `display: block`

- Ocupa toda la linea disponible.
- Empieza en linea nueva.
- Acepta `width`, `height`, `margin`, `padding`.
- Ejemplos: `div`, `p`, `section`, `article`, `header`, `footer`, `form`, `h1`.

### `display: inline`

- Solo ocupa lo que ocupa su contenido.
- No fuerza salto de linea.
- `width` y `height` no funcionan como esperas.
- Ejemplos: `span`, `a`, `strong`, `em`, `label`.

### `display: inline-block`

- Se coloca en linea.
- Acepta `width`, `height`, `padding` y `margin`.
- Util para botones simples, etiquetas, chips o elementos pequenos que deben ir en una linea.

### `display: flex`

- Convierte el contenedor en flexible.
- Sus hijos directos pasan a ser flex items.
- Ideal para alinear y repartir espacio en una dimension.

### `display: flow-root`

- Crea un nuevo contexto de formato de bloque.
- Sirve mucho cuando hay elementos con `float` dentro de un contenedor.
- Hace que el padre tenga en cuenta la altura de sus hijos flotados.
- Tambien ayuda a aislar margenes internos y evitar algunos efectos raros de colapso.

Ejemplo tipico:

```html
<div class="contenedor">
  <img class="foto" src="foto.jpg" alt="">
  <p>Texto rodeando la imagen.</p>
</div>
```

```css
.contenedor {
  display: flow-root;
}

.foto {
  float: left;
  margin-right: 16px;
}
```

Antes se usaba mucho el "clearfix" para arreglar padres con hijos flotados. Ahora
`display: flow-root` es una forma mas limpia.

### `display: grid`

- Ideal para filas y columnas a la vez.
- Muy bueno para layouts principales, galerias, dashboards y formularios complejos.

## 4. Flexbox

Flexbox tiene dos ejes:

- Eje principal: depende de `flex-direction`.
- Eje cruzado: perpendicular al principal.

```css
.contenedor {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 16px;
}
```

### Propiedades del contenedor

- `flex-direction: row`: hijos en fila.
- `flex-direction: column`: hijos en columna.
- `justify-content`: alinea en el eje principal.
- `align-items`: alinea en el eje cruzado.
- `gap`: separacion entre items.
- `flex-wrap: wrap`: permite que los items bajen de linea si no caben.

### Propiedades del item

```css
.item {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 200px;
}
```

- `flex-grow`: cuanto crece si sobra espacio.
- `flex-shrink`: cuanto se encoge si falta espacio.
- `flex-basis`: tamano inicial antes de repartir espacio.

Atajo:

```css
flex: 1 1 200px;
```

Significa:

```text
grow 1, shrink 1, basis 200px
```

### `flex-shrink`

Por defecto es `1`.

```css
.no-se-encoge {
  flex-shrink: 0;
}
```

Si pones `flex-shrink: 0`, el item intenta conservar su tamano. Cuidado: puede provocar overflow si no cabe.

## 5. Float

Hoy no se usa para maquetar una pagina completa. Para eso usa flex o grid.

Usalo si quieres que un texto rodee una imagen o una caja:

```css
img {
  float: left;
  margin-right: 16px;
}
```

Si despues quieres que algo no rodee al flotado:

```css
.siguiente {
  clear: both;
}
```

Si el padre no envuelve bien a sus hijos flotados:

```css
.padre {
  display: flow-root;
}
```

## 6. Position

### `static`

Valor por defecto. El elemento sigue el flujo normal.

### `relative`

El elemento mantiene su hueco original, pero puedes desplazarlo:

```css
.caja {
  position: relative;
  top: 10px;
}
```

### `absolute`

Sale del flujo normal. Se posiciona respecto al primer ancestro que tenga `position` distinto de `static`.

```css
.tarjeta {
  position: relative;
}

.badge {
  position: absolute;
  top: 8px;
  right: 8px;
}
```

### `fixed`

Se queda fijo respecto a la ventana.

```css
.menu {
  position: fixed;
  top: 0;
  left: 0;
}
```

### `sticky`

Empieza normal y luego se queda pegado cuando llega a una posicion.

```css
.indice {
  position: sticky;
  top: 0;
}
```

## 7. Formularios HTML

Estructura basica:

```html
<form>
  <fieldset>
    <legend>Datos</legend>

    <label for="nombre">Nombre</label>
    <input id="nombre" name="nombre" type="text">

    <button type="submit">Enviar</button>
  </fieldset>
</form>
```

Etiquetas clave:

- `form`: formulario completo.
- `label`: texto asociado a un campo.
- `input`: campo. Cambia segun `type`.
- `textarea`: texto largo.
- `select`: desplegable.
- `option`: opcion dentro de un select.
- `fieldset`: agrupa campos.
- `legend`: titulo del grupo.
- `button`: boton.

Tipos utiles de `input`:

- `text`
- `email`
- `password`
- `number`
- `date`
- `color`
- `range`
- `checkbox`
- `radio`
- `file`
- `submit`

### Box-sizing por defecto en formularios

La regla general de CSS es:

```css
box-sizing: content-box;
```

Ese es el valor inicial de la propiedad para casi todos los elementos. Pero los
navegadores aplican hojas de estilo internas y algunos controles de formulario suelen
venir con `border-box`.

Resumen practico:

| Elemento | Box-sizing habitual por defecto |
| --- | --- |
| `input type="text"`, `email`, `password`, `number`, `date` | `content-box` como referencia teorica, aunque puede variar segun navegador |
| `textarea` | normalmente `content-box` |
| `button` | normalmente `border-box` |
| `select` | normalmente `border-box` |
| `input type="button"`, `submit`, `reset` | normalmente `border-box` |
| `input type="checkbox"`, `radio`, `color`, `search` | normalmente `border-box` |

Para evitar dudas en una practica, puedes normalizarlo:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Idea de examen:

- Si te preguntan por el valor inicial CSS: `content-box`.
- Si te preguntan por controles concretos renderizados por el navegador: `button`,
  `select` y varios `input` especiales suelen usar `border-box`.

## 8. Media queries

Mobile first:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr;
}

@media (min-width: 700px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1000px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

Primero escribes el CSS de movil. Luego, con `min-width`, adaptas para pantallas mas grandes.

Desktop first:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

@media (max-width: 700px) {
  .grid {
    grid-template-columns: 1fr;
  }
}
```

Primero escribes escritorio. Luego, con `max-width`, corriges en pantallas pequenas.

## 9. Preguntas tipo examen

1. Si una caja tiene `width: 300px`, `padding: 20px` y `border: 5px`, cuanto ocupa con `content-box`?

Respuesta: `300 + 40 + 10 = 350px`, sin contar margin.

2. Si la misma caja usa `border-box`, cuanto ocupa?

Respuesta: `300px`, sin contar margin.

3. Que diferencia hay entre `justify-content` y `align-items`?

Respuesta: `justify-content` alinea en el eje principal. `align-items` alinea en el eje cruzado.

4. Que hace `flex-shrink: 0`?

Respuesta: evita que el item se encoja si falta espacio, aunque puede causar overflow.

5. Cuando usar `float`?

Respuesta: para que texto rodee una imagen/caja. Para layouts modernos, mejor flex o grid.

6. Que necesita un hijo `absolute` para colocarse dentro de una tarjeta?

Respuesta: que la tarjeta tenga `position: relative`.

7. Para que sirve `fieldset`?

Respuesta: para agrupar controles relacionados de un formulario.

8. Que hace una media query?

Respuesta: aplica CSS solo si se cumple una condicion, normalmente el ancho de pantalla.

## 10. Plantilla parecida al examen anterior

He creado estos archivos:

- `examen-anterior.html`
- `examen-anterior.css`

La estructura que describes se resuelve asi:

```text
contenedor 1200px
  navbar ancho 100%
  main con display: flow-root
    caja izquierda flotada a la izquierda, sin hueco
    caja derecha flotada a la derecha, sin hueco
  footer ancho 100%
```

### Calculo de anchuras

Si el contenedor mide 1200px y no hay espacios entre cajas:

```text
izquierda 400px + derecha 800px = 1200px
```

```css
.izquierda {
  float: left;
  width: 400px;
}

.derecha {
  float: right;
  width: 800px;
  text-align: left;
}
```

Si hubiera padding en el `main`, tendrias que restarlo. Pero en esta version no hay
padding entre cajas porque recuerdas que estaban pegadas.

### Trampa de los inputs con ancho

Si a un input le pones:

```css
input {
  width: 100%;
  padding: 10px;
  border: 2px solid black;
}
```

y el input esta en `content-box`, puede salirse de su formulario, porque el calculo real es:

```text
100% + padding izquierdo + padding derecho + border izquierdo + border derecho
```

Solucion tipica:

```css
input {
  box-sizing: border-box;
  width: 100%;
}
```

Asi el `100%` ya incluye contenido, padding y border.

### Trampa de la caja morada hasta el footer

Con `float`, las cajas no se estiran automaticamente para igualar alturas como si fueran
columnas de tabla. Si quieres que la caja morada llegue hasta el footer, tienes varias opciones:

1. Dar la misma altura a las dos cajas.

```css
.caja {
  height: 560px;
}
```

2. Poner el fondo morado tambien en el contenedor de las cajas.

```css
.contenido {
  display: flow-root;
  background: purple;
}
```

En la practica he usado las dos ideas para que se vea claro: las cajas tienen la misma altura
y el contenedor central tiene fondo morado.

### Por que `display: flow-root` en el `main`

Las cajas estan flotadas con `float`. Los floats salen parcialmente del flujo normal.
Si el padre no los encierra bien, el footer puede colocarse mal.

Solucion moderna:

```css
.contenido {
  display: flow-root;
}
```

Esto sustituye al clearfix clasico.

### Lista sin estilos por defecto

Para quitar puntos y espacios de una lista:

```css
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
```

### Etiqueta HTML para poner texto en linea

La etiqueta que seguramente recuerdas es:

```html
<span>Texto 1</span>
<span>Texto 2</span>
```

`span` es inline por defecto. Eso significa que no empieza en una linea nueva,
al contrario que `div`, `p`, `section` o `article`, que son block.

Tambien son inline por defecto:

- `a`
- `strong`
- `em`
- `label`

Pero para un footer con dos textos sueltos, lo mas normal es usar `span`.
