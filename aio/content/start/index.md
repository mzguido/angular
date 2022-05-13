# Parte 1: Empezando con una aplicacion básica de Angular

¡Bienvenido a Angular!

Este turorial presenta los conceptos básicos de Angular guiando a través de un sitio de e-commerce con un catálogo, un carrito de compras y un formulario de pago.
Para ayudarte a empezar de inmediato, esta guia usa una aplicacion simple y lista para usar en la que puede examinar y modificar de forma interactiva (sin tener que [Configurar el ambiente y el espacio de trabajo locales](guide/setup-local "Guía de configuración")).

<div class="callout is-helpful">
<header>¿Eres nuevo en el desarrollo web?</header>

Hay muchos recursos para complementar la documentación de Angular. La documentación MDN de Mozilla incluye las introducciones de [HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML "Learning HTML: Guides and tutorials") y [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript "JavaScript").
[TypeScript's docs](https://www.typescriptlang.org/docs/home.html "TypeScript documentation") incluye un tutorial de 5 minutos. Muchas plataformas de cursos online, como [Udemy](http://www.udemy.com "Udemy online courses") y [Codecademy](https://www.codecademy.com/ "Codecademy online courses"), también cubren lo básico del desarrollo web

</div>

{@a new-project}
## Crear un proyecto de muestra

<h4>
<live-example name="getting-started-v0" noDownload>Presiona aquí para crear un proyecto de muestra listo para usar en StackBlitz.</live-example>
</h4>

<div class="lightbox">
  <img src="generated/images/guide/start/new-app-all.gif" alt="Starter online store app">
</div>

* El panel de previsualización de la derecha muestra el estado inicial de la aplicación de Angular.
Define un marco con la barra superior (contiene el nombre de la tienda y el icono del carrito de compra) y el título de la lista de productos (que se compretará y actuliazará de forma dinamica con datos de la aplicación)

* El panel del proyecto de la izquierda muestra los archivos fuente que componen la aplicación, estan incluidos todos los archivos de configuración e infraestructura. El archivo seleccionado actualmente aparece en el panel del editor en el medio.

Antes de ver la estructura fuente, la siguiente sección muestra como completar la *plantilla* HTML para la lista de productos, utilizando los datos de ejemplo proporcionados.
Esto deberia darte una idea de la facíl que es modificar y actualizar la página dinámicamente. 

<div class="callout is-helpful">
<header>Consejos de StackBlitz</header>

* Inicia sesión en StackBlitz para que puedas guardar y reanudar tu trabajo.
Si tienes una cuenta de GitHub, puedes iniciar sesión en StackBlitz
con esa cuenta. Para guardar su progreso, primero
bifurque el proyecto usando el botón bifurcación en la parte superior izquierda,
luego podrás guardar tu trabajo en tu propia cuenta de StackBlitz haciendo clic en el botón Guardar.
* Para copiar un código de ejemplo de este tutorial, haz clic en el icono
en la parte superior derecha del cuadro de código de ejemplo y luego pega el
fragmento de código del portapapeles en StackBlitz.
* Si el panel de vista previa de StackBlitz no muestra lo que
esperas, guarda y luego haz clic en el botón actualizar.
* StackBlitz mejora continuamente, por lo que puede haber
ligeras diferencias en el código generado, pero el comportamiento 
de la aplicación será el mismo.
* Cuando generas las aplicaciones de ejemplo que
acompañan los tutoriales de StackBlitz, StackBlitz crea los archivos iniciales y datos de prueba para tí. Los archivos que usaras en todo
los tutoriales están en la carpeta `src` de las
aplicaciones de ejemplo de StackBlitz.

</div>

<div class="alert is-important">

Si va directamente al [entorno de desarrollo en línea de StackBlitz](https://stackblitz.com/) y elige [iniciar un nuevo espacio de trabajo de Angular](https://stackblitz.com/fork/angular), obtiene aplicación de código auxiliar, en lugar de esta [muestra ilustrativa](#new-project). Una vez que haya sido introducido a los conceptos básicos aquí, esto puede ser útil para trabajar de forma interactiva mientras aprende Angular.

En el desarrollo real, normalmente utilizaras [Angular CLI](guide/glossary#command-line-interface-cli "Definition of CLI"), una poderosa herramienta de línea de comandos que te permite generar y modificar aplicaciones. Para obtener una guía completa paso a paso que muestra cómo usar la CLI para crear un nuevo proyecto y todas sus partes, consulte [Aplicación y tutorial Tour de héroes](tutorial).

</div>


{@a template-syntax}
## Sintaxis de la plantilla

La sintaxis de plantilla de Angular extiene HTML y JavaScript.
Esta sección presenta la sintaxis de la plantilla mejorando el área "Productos".

<div class="alert is-helpful">

Para ayudarte a comenzar, los siguientes pasos utilizan datos de productos predefinidos del archivo `products.ts` (ya creado en el ejemplo de StackBlitz) y métodos del archivo` product-list.component.ts`.

</div>

1. En la carpeta `product-list`, abre el archivo de plantilla `product-list.component.html`.

1. Modifica la plantilla de la lista de productos para mostrar una lista de nombres de productos.

    1. Each product in the list displays the same way, one after another on the page. To iterate over the predefined list of products, put the `*ngFor` directive on a `<div>`, as follows:

    1. Cada producto de la lista se muestra de la misma forma, una tras otra en la página. Para iterar sobre la lista de productos predefinidos, coloque la directiva `*ngFor` dentro de un `<div>`, como se muestra a continuación:

      <code-example header="src/app/product-list/product-list.component.html" path="getting-started/src/app/product-list/product-list.component.2.html" region="ngfor">
      </code-example>

      Con `*ngFor`, el `<div>` se repite para cada uno de los productos de la lista.

      <div class="alert is-helpful">

      `*ngFor` es una "directiva estructural". Las directivas estructurales dan forma o remodelan la estructura del DOM, generalmente agregando, quitando y manipulando los elementos a los que están adjuntos. Las directivas con un asterisco, `*`, son directivas estructurales. 

      </div>

    1. Para mostrar los nombres de los productos, utiliza la sintaxis de interpolación `{{}}`. La interpolación representa el valor de una propiedad como texto. Dentro de `<div>`, agrega un `<h3>` para mostrar la interpolación de la propiedad del nombre del producto:

      <code-example path="getting-started/src/app/product-list/product-list.component.2.html" header="src/app/product-list/product-list.component.html" region="interpolation">
      </code-example>

       El panel de vista previa se actualiza inmediatamente para mostrar el nombre de cada producto en la lista.

      <div class="lightbox">
        <img src="generated/images/guide/start/template-syntax-product-names.png" alt="Product names added to list">
      </div>

1. Para hacer que el nombre de cada producto sea un enlace a los detalles del producto, agrega el elemento `<a>` y establece su título como el nombre del producto utilizando la sintaxis de enlace de propiedad `[]`, de la siguiente manera:

    <code-example path="getting-started/src/app/product-list/product-list.component.2.html" header="src/app/product-list/product-list.component.html">
    </code-example>

     En el panel de vista previa, manten el puntero sobre un nombre de producto para ver el valor de la propiedad del nombre enlazado, que es el nombre del producto más la palabra "detalles".
     La interpolación `{{ }}` le permite renderizar el valor de propiedad como texto; el enlace de propiedad `[ ]` le permite utilizar el valor de la propiedad en una expresión de plantilla.

    <div class="lightbox">
      <img src="generated/images/guide/start/template-syntax-product-anchor.png" alt="Product name anchor text is product name property">
    </div>


4. Agrega las descripciones de los productos. En el elemento `<p>`, usa una directiva `*ngIf` para que Angular cree el elemento `<p>` solo si el producto actual tiene una descripción.

    <code-example path="getting-started/src/app/product-list/product-list.component.3.html" header="src/app/product-list/product-list.component.html">
    </code-example>

    Ahora la aplicación muestra el nombre y la descripción de cada producto en la lista. Ten en cuenta que el último producto no tiene un párrafo de descripción. Debido a que la propiedad de descripción del producto está vacía, Angular no crea el elemento `<p>` ni incluye la palabra "Descripción".

    <div class="lightbox">
      <img src="generated/images/guide/start/template-syntax-product-description.png" alt="Product descriptions added to list">
    </div>

5. Agrega un botón para que los usuarios puedan compartir un producto con sus amigos. Vincula el evento `click` del botón al método `share()` (en `product-list.component.ts`). El enlace de eventos usa paréntesis, `( )`, alrededor del evento, como en el siguiente `<button>`:

    <code-example path="getting-started/src/app/product-list/product-list.component.4.html" header="src/app/product-list/product-list.component.html">
    </code-example>

    Ahora cada producto tiene un botón "Compartir"

    <div class="lightbox">
      <img src="generated/images/guide/start/template-syntax-product-share-button.png" alt="Share button added for each product">
    </div>

    Prueba el botón "Compartir"

    <div class="lightbox">
      <img src="generated/images/guide/start/template-syntax-product-share-alert.png" alt="Alert box indicating product has been shared">
    </div>

Ahora la aplicación tiene una lista de productos y una función para compartir.
En el proceso, haz aprendido a usar cinco características comunes de la sintaxis de la plantilla de Angular:
* `*ngFor`
* `*ngIf`
* Interpolacion `{{ }}`
* Enlace de propiedad `[ ]`
* Enlace de de evento `( )`


<div class="alert is-helpful">

For a fuller introduction to Angular's template syntax, see [Introduction to components and templates](guide/architecture-components#template-syntax "Template Syntax").

Para una introducción más completa a la sintaxis de la plantilla de Angular, ve a [Introducción a componentes y plantillas](guide/architecture-components#template-syntax "Sintaxis de plantilla").

</div>


{@a components}
## Components

Los *Componentes* definen áreas de responsabilidad en la interfaz de usuario, o UI,
que le permiten reutilizar conjuntos de funciones de la interfaz de usuario.
Ya haz creado uno con el componente de la lista de productos.

Un componente consta de tres cosas:
* **Una clase de componente** que maneja datos y funcionalidad. En la sección anterior, los datos del producto y el método `share()` en la clase de componente manejan datos y funcionalidad, respectivamente.
* **Una plantilla HTML** que determina la interfaz de usuario. En la sección anterior, la plantilla HTML de la lista de productos muestra el nombre, la descripción y un botón "Compartir" para cada producto.
* **Estilos de componentes específicos** que definen la apariencia.
Aunque la lista de productos no define ningún estilo, aquí es donde el CSS del componente
reside.

<!--
### Class definition

Let's take a quick look a the product list component's class definition:

1. In the `product-list` directory, open `product-list.component.ts`.

1. Notice the `@Component` decorator. This provides metadata about the component, including its templates, styles, and a selector.

    * The `selector` is used to identify the component. The selector is the name you give the Angular component when it is rendered as an HTML element on the page. By convention, Angular component selectors begin with the prefix such as `app-`, followed by the component name.

    * The template and style filename also are provided here. By convention each of the component's parts is in a separate file, all in the same directory and with the same prefix.

1. The component definition also includes an exported class, which handles functionality for the component. This is where the product list data and `Share()` method are defined.

### Composition
-->

Una aplicación Angular comprende un árbol de componentes, en el que cada componente de Angular tiene un propósito y una responsabilidad específicos.

Actualmente, la aplicación de ejemplo tiene tres componentes:

<div class="lightbox">
  <img src="generated/images/guide/start/app-components.png" alt="Online store with three components">
</div>

* `App-root` (cuadro naranja) es el armazón de la aplicación. Este es el primer componente que se carga y el padre de todos los demás componentes. Puedes pensar en el como la página base.
* `App-top-bar` (fondo azul) es el nombre de la tienda y el botón de pago.
* `App-product-list` (cuadro violeta) es la lista de productos que modificaste en la sección anterior.

La siguiente sección amplía las capacidades de la aplicación al agregar un nuevo componente&mdash;una alerta de producto&mdash;como elemento hijo del componente de lista de productos.


<div class="alert is-helpful">

Para más información sobre componentes y cómo interactúan con las plantillas, visita[Introducción a componentes y plantillas](guide/architecture-components "Conceptos > Introducción a componentes y plantillas").

</div>


{@a input}
## Entrada/Input

Actualmente, la lista de productos muestra el nombre y la descripción de cada producto.
El componente de lista de productos también define una propiedad de `productos` que contiene datos importados para cada producto del array `productos` en `productos.ts`.

El siguiente paso es crear una nueva función de alerta que tome un producto como entrada. La alerta verifica el precio del producto y, si el precio es superior a $700, muestra un botón "Notificarme" que permite a los usuarios registrarse para recibir notificaciones cuando el producto sale a la venta.

1. Crear un nuevo componente de alertas de producto

    1. Haz clic derecho en la carpeta `app` y usa el `Angular Generator` para generar un nuevo componente llamado `product-alerts`.

        <div class="lightbox">
          <img src="generated/images/guide/start/generate-component.png" alt="StackBlitz command to generate component">
        </div>

         El generador crea archivos iniciles para las tres partes del componente:
        * `product-alerts.component.ts`
        * `product-alerts.component.html`
        * `product-alerts.component.css`

1. Abre `product-alerts.component.ts`.

    <code-example header="src/app/product-alerts/product-alerts.component.ts" path="getting-started/src/app/product-alerts/product-alerts.component.1.ts" region="as-generated"></code-example>

    1. Observa el decorador `@Component()`. Este indica que la siguiente clase es un componente. Proporciona metadatos sobre el componente, incluido su selector, plantillas y estilos.

        * El `selector` identifica el componente. El selector es el nombre que le da al componente Angular cuando se representa como un elemento HTML en la página. Por convención, los selectores de componentes de Angular comienzan con el prefijo `app-`, seguido del nombre del componente.

        * Los nombres de archivo de plantilla y estilo hacen referencia a los archivos HTML y CSS que genera StackBlitz.

    1. La definición del componente también exporta la clase, `ProductAlertsComponent`, que maneja la funcionalidad del componente.

1. Configura el componente de alerta de nuevos productos para recibir un producto como entrada:

    1. Importa `Input` desde `@angular/core`.

        <code-example path="getting-started/src/app/product-alerts/product-alerts.component.1.ts" region="imports" header="src/app/product-alerts/product-alerts.component.ts"></code-example>

    1. En la definición de la clase `ProductAlertsComponent`, define una propiedad llamada `producto` con un decorador `@Input()`. El decorador `@Input ()` indica que el valor de la propiedad pasa del padre del componente, el componente de la lista de productos.

        <code-example path="getting-started/src/app/product-alerts/product-alerts.component.1.ts" region="input-decorator" header="src/app/product-alerts/product-alerts.component.ts"></code-example>

1. Define la vista del componente alerta de nuevo producto.

    1. Abre la plantilla `product-alerts.component.html` y reemplaza el párrafo con un botón "Notificarme" que aparece si el precio del producto es superior a $700.

    <code-example header="src/app/product-alerts/product-alerts.component.html" path="getting-started/src/app/product-alerts/product-alerts.component.1.html"></code-example>

1. Muestra el componente alerta de nuevo producto como un elemento hijo de la lista de productos.

    1. Abre `product-list.component.html`.

    1. Para incluir el nuevo componente, usa su selector, `app-product-alerts`, como lo haría con un elemento HTML.

    1. Pasa el producto actual como entrada al componente mediante el enlace de propiedad.

        <code-example header="src/app/product-list/product-list.component.html" path="getting-started/src/app/product-list/product-list.component.5.html" region="app-product-alerts"></code-example>

El componente alerta de nuevo producto toma un producto como entrada de la lista de productos. Con esa entrada, muestra u oculta el botón "Notificarme", según el precio del producto. El precio del Telefóno XL es de más de $700, por lo que el botón "Notificarme" aparece en ese producto.

<div class="lightbox">
  <img src="generated/images/guide/start/product-alert-button.png" alt="Product alert button added to products over $700">
</div>

<div class="alert is-helpful">

Visita [Interacción de componentes](guide/component-interaction "Componentes y plantillas> Interacción de componentes") para obtener más información sobre cómo pasar datos de un componente principal a secundario, interceptar y actuar sobre un valor del elemento principal y detectar y actuar sobre los cambios en valores de propiedad de entrada.

</div>


{@a output}
## Salida/Output

Para que el botón "Notificarme" funcione, debes configurar dos cosas:

  - the product alert component to emit an event when the user clicks "Notify Me"
  - the product list component to act on that event

1. Abre  `product-alerts.component.ts`.

1. Importa `Output` y `EventEmitter` desde `@angular/core`:

    <code-example header="src/app/product-alerts/product-alerts.component.ts" path="getting-started/src/app/product-alerts/product-alerts.component.ts" region="imports"></code-example>

1. En la clase de componente, define una propiedad llamada `notificar` con un decorador `@Output()` y una instancia de `EventEmitter()`. Esto permite que el componente alerta de producto emita un evento cuando cambie el valor de la propiedad de notificación.

<div class="alert is-helpful">

   Cuando Angular CLI genera un nuevo componente, incluye un constructor vacío, la interfaz `OnInit` y el método `ngOnInit()`.
   Dado que el siguiente ejemplo no los usa, se omiten por el momento.

</div>

    <code-example path="getting-started/src/app/product-alerts/product-alerts.component.ts" header="src/app/product-alerts/product-alerts.component.ts" region="input-output"></code-example>

1. En la plantilla de alerta de producto, `product-alerts.component.html`, actualiza el botón "Notificarme" con una vinculación de eventos para llamar al método `notify.emit()`.

    <code-example header="src/app/product-alerts/product-alerts.component.html" path="getting-started/src/app/product-alerts/product-alerts.component.html"></code-example>


1. A continuación, define el comportamiento que debería ocurrir cuando el usuario hace clic en el botón. Recuerda que es el componente padre, lista de productos&mdash;no el componente de alertas de productos&mdash;el que actúa cuando el hijo genera el evento. En `product-list.component.ts`, defina un método `onNotify()`, similar al método `share()`:

    <code-example header="src/app/product-list/product-list.component.ts" path="getting-started/src/app/product-list/product-list.component.ts" region="on-notify"></code-example>

1. Por último, actualiza el componente de lista de productos para recibir resultados del componente de alertas de productos.

    En `product-list.component.html`, vincula el componente `app-product-alerts` (que es lo que muestra el botón "Notificarme") al método `onNotify()` del componente de lista de productos.

    <code-example header="src/app/product-list/product-list.component.html" path="getting-started/src/app/product-list/product-list.component.6.html" region="on-notify"></code-example>

1. Prueba el botón "Notificarme":

    <div class="lightbox">
      <img src="generated/images/guide/start/product-alert-notification.png" alt="Product alert notification confirmation dialog">
    </div>


<div class="alert is-helpful">


Visita [Interacción de componentes](guide/component-interaction "Componentes y plantillas> Interacción de componentes") para obtener más información sobre cómo escuchar eventos de componentes hijos, leer propiedades hijas o invocar métodos hijos y utilizar un servicio para la comunicación bidireccional entre componentes.

</div>


{@a next-steps}
## Próximos pasos

¡Felicidades! ¡Has completado tu primera aplicación Angular!

Tienes un catálogo básico de la tienda en línea con una lista de productos, el botón "Compartir" y el botón "Notificarme".
Haz aprendido sobre la base de Angular: componentes y sintaxis de plantilla.
También haz aprendido cómo interactúan la clase de componente y la plantilla, y cómo los componentes se comunican entre sí.

Para continuar explorando Angular, elije cualquiera de las siguientes opciones:
* [Continue to the "In-app navigation" section](start/start-routing "Try it: In-app navigation") to create a product details page that can be accessed by clicking a product name and that has its own URL pattern.
* [Continua con la sección "Navegación en la aplicación"](start/start-routing "Pruébelo: navegación en la aplicación") para crear una página de detalles del producto a la que se puede acceder haciendo clic en el nombre de un producto y que tiene su propio patron URL.
* [Skip ahead to the "Deployment" section](start/start-deployment "Try it: Deployment") to move to local development, or deploy your app to Firebase or your own server.* [Salta a la sección "Despliegue"](start/start-deployment "Pruébelo: Despliegue") para pasar al desarrollo local o desplegar su aplicación en Firebase o en tu propio servidor.