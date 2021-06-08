# Usando rutas de Angular en una aplicación de una sola página

Este tutorial explica cómo puedes construir una SPA que utilice multiples rutas en Angular.

En una SPA, todas las funcionalidades de la aplicación existen en una sola página de HTML.
A medida que los usuarios acceden a las funcionalidades de tu aplicación, el navegador necesita representar solo las partes que son importantes para el usuario, en lugar de cargar una nueva página por cada acción. Este patrón puede mejorar significativamente la experiencia de usuario de tu aplicación.

Utiliza las rutas para definir la manera en la que los usuarios navegan a través de tu aplicación.
Puedes agregar rutas para definir cómo los usuarios navegan en tu aplicación de una parte a otra. También puedes configurar rutas para protegerte contra comportamientos inesperados o no autorizados.

Para ver en funcionamiento una aplicación de ejemplo basado en este tutorial puedes consultar: <live-example></live-example>.

## Objetivos

- Organizar las funcionalidades de una aplicación ejemplo en módulos.
- Definir como se navega hacia un componente.
- Pasar información hacia un componente a través de parámetros.
- Estructurar rutas mediante el anidamiento de varias rutas
- Comprobar si los usuarios pueden acceder a cierta ruta en particular.
- Controlar si la aplicación puede descartar los cambios no guardados.
- Mejorar el desempeño por medio de la obtención previa de datos de ruta y módulos con funcionalidades de carga diferida.
- Requerir criterios específicos para cargar componentes.

## Pre-requisitos

- HTML
- CSS
- JavaScript
- [Angular CLI](/cli)

Puede que te sea útil el [tutorial de Tour de Héroes](/tutorial), pero es opcional.

## Pasos para crear una aplicación de ejemplo

Usando el Angular CLI crea una aplicación con el nombre: _angular-router-sample_.
Esta aplicación tendrá dos componentes:

- _crisis-list_
- _heroes-list_

1. Crea el proyecto con el nombre de _angular-router-sample_ usando el siguiente comando:

   <code-example language="sh">
   ng new angular-router-sample
   </code-example>

   - Cuando se te pregunte `Would you like to add Angular routing?`, selecciona `N`
   - Cuando se te pregunte `Which stylesheet format would you like to use?`, selecciona `CSS`
   - Después de esperar un momento, el proyecto nuevo `angular-router-sample` estará listo.

1. Desde tu terminal navega al directorio `angular-router-sample`.

 <code-example language="sh">
 ng generate component crisis-list
</code-example>

1. En tu editor de código localiza el archivo con el nombre `crisis-list.component.html` y remplaza el contenido de _placeholder_ con la siguiente linea de HTML:

   <code-example header="src/app/crisis-list/crisis-list.component.html" path="router-tutorial/src/app/crisis-list/crisis-list.component.html"></code-example>

1. Crea el segundo componente _heroes-list_ usando el siguiente comando:

 <code-example language="sh">
 ng generate component heroes-list
</code-example>

1. En tu editor de código localiza el archivo llamado `heroes-list.component.html` y remplaza el contenido de _placeholder_ con la siguiente linea de HTML:

   <code-example header="src/app/heroes-list/heroes-list.component.html" path="router-tutorial/src/app/heroes-list/heroes-list.component.html"></code-example>

1. En tu editor de código abre el archivo llamado `app.component.html` y remplaza su contenido con la siguiente linea de HTML:

   <code-example header="src/app/app.component.html" path="router-tutorial/src/app/app.component.html" region="setup"></code-example>

1. Verifica que al ejecutar el siguiente comando `ng serve` en la terminal, la aplicación se ejecute adecuadamente.

<code-example language="sh">
   ng serve
  </code-example>

1. Ahora abre el navegador en `http://localhost:4200`.
   Deberías ver una sola página web, que consta de un título y el HTML de sus dos componentes.

## Pasos para importar `RouterModule` from `@angular/router`

El enrutamiento te permite mostrar vistas específicas de su aplicación según la ruta de la URL.
Para agregar esta funcionalidad a tu aplicación de ejemplo, necesitas actualizar el archivo `app.module.ts` para usar el módulo ` RouterModule`.
Importa este módulo desde `@angular/router`.

1. Desde tu editor de código, abre el archivo `app.module.ts`.
1. Agrega la siguiente declaración de `import`
   <code-example header="src/app/app.module.ts" path="router-tutorial/src/app/app.module.ts" region="router-import"></code-example>

## Define tus rutas

En esta sección definirás dos rutas:

- La ruta `/crisis-center` que direcionará hacia el componente `crisis-center`.
- La ruta `/heroes-list` que direcionará hacia el componente `heroes-list`.

Una definición de ruta es un objeto JavaScript. Cada ruta tiene normalmente dos propiedades. La primera propiedad es `path` que es una cadena de caracteres que especifica la ruta URL de la ruta. La segunda propiedad es `component` que es una cadena de caracteres que especifica el componente que debe mostrar su aplicación para esa ruta.

1. Desde tu editor de código abre el archivo `app.modules.ts`
1. Localiza la sección de `@NgModule()`
1. Estando en la sección de `@NgModule()` remplaza el arreglo de `imports` con las siguientes líneas de código:

   <code-example header="src/app/app.module.ts" path="router-tutorial/src/app/app.module.ts" region="import-basic"></code-example>

El fragmento de código anterior añade `RouterModules` al arreglo de `imports`. A continuación, el código usa el método `forRoot()` del `RouterModule` para definir tus dos rutas. Este método toma un arreglo de objetos JavaScript, con cada objeto definiendo las propiedades de una ruta. El método `forRoot()` asegura que tu aplicación solo instancia un `RouterModule`. Para obtener más información consulta [Singleton Services](/guide/singleton-services#forroot-and-the-router).

## Actualiza tu componente con `router-outlet`

Hasta este punto, has definido dos rutas para la aplicación. Sin embargo, la aplicación todavía tiene los componentes `crisis-center` y `heroes-list` codificados en su plantilla `app.component.html`. Para que tus rutas funcionen, necesita actualizar tu plantilla para cargar dinámicamente un componente basado en la ruta URL.

Para implementar esta funcionalidad, agregua la directiva `router-outlet` a tu archivo de plantilla de acuerdo a los siguientes pasos:

1.  Desde tu editor de código abre el archivo `app.component.html`.
1.  Elimina las siguientes líneas de código:

    <code-example header="src/app/app.component.html" path="router-tutorial/src/app/app.component.html" region="components"></code-example>

1.  Agrega `router-outlet`:

    <code-example header="src/app/app.component.html" path="router-tutorial/src/app/app.component.html" region="router-outlet"></code-example>

Mira tu aplicación actualizada en tu navegador. Deberías ver solo el título de la aplicación. Para ver el componente `crisis-list`, agrega `crisis-list` al final de la ruta en la barra de direcciones de tu navegador. Por ejemplo:

<code-example language="none">
http://localhost:4200/crisis-list
</code-example>

Observa que se muestra el componente `crisis-list`. Angular está usando la ruta que definiste previamente para cargar dinámicamente el componente. Puedes cargar el componente `heroes-list` de la misma manera:

<code-example language="none">
http://localhost:4200/heroes-list
</code-example>

## Controla la navegación con elementos de la interfaz de usuario

Actualmente, tu aplicación admite dos rutas. Sin embargo, la única forma de utilizar esas rutas es que el usuario escriba manualmente la ruta en la barra de direcciones del navegador. En esta sección, agregarás dos enlaces en los que los usuarios pueden hacer clic para navegar entre los componentes `crisis-list` y `heroes-list`. También agregarás algunos estilos CSS. Si bien estos estilos no son obligatorios, facilitan la identificación del vínculo del componente que se muestra actualmente. Agregarás esa funcionalidad en la siguiente sección.

1. Abre el archivo `app.component.html` y agrega el siguiente HTML debajo del título:

   <code-example header="src/app/app.component.html" path="router-tutorial/src/app/app.component.html" region="nav"></code-example>

   Este HTML usa una directiva Angular, `routerLink`. Esta directiva conecta las rutas
   que definiste en tus archivos de plantilla.

1. Abre el archivo `app.component.css` y agrega los siguientes estilos:

   <code-example header="src/app/app.component.css" path="router-tutorial/src/app/app.component.css"></code-example>

Si ves tu aplicación en el navegador, deberías ver estos dos enlaces. Cuando haces clic en un enlace, aparece el componente correspondiente.

## Identifica la ruta activa

Si bien los usuarios pueden navegar por tu aplicación utilizando los enlaces que agregaste en la sección anterior, no tienen una manera fácil de identificar cuál es la ruta activa. Puedes agregar esta funcionalidad usando la directiva `routerLinkActive` de Angular.

1. Desde tu editor de código, abre el archivo `app.component.html`.

1. Actualiza las etiquetas de ancla `<a>` para incluir la directiva `routerLinkActive`:

   <code-example header="src/app/app.component.html" path="router-tutorial/src/app/app.component.html" region="routeractivelink"></code-example>

Mira tu aplicación nuevamente. Al hacer clic en uno de los botones, el estilo de ese botón se actualiza
automáticamente, identificando el componente activo al usuario. Añadiendo el `routerLinkActive`
directiva, le informa a su aplicación que aplique una clase CSS específica a la ruta activa. En este
tutorial, esa clase CSS es `activebutton`, pero puedes usar cualquier clase que desees.

## Agregar una redirección

En este paso del tutorial, agregarás una ruta que redirige al usuario para que muestre el componente `/heroes-list`.

1. Desde tu editor de código, abre el archivo `app.module.ts`.

1. En el arreglo de `imports` actualiza la sección `RouterModule` de la siguiente manera:

   <code-example header="src/app/app.module.ts" path="router-tutorial/src/app/app.module.ts" region="import-redirect"></code-example>

   Observa que esta nueva ruta usa una cadena de caracteres vacía como su ruta. Además, reemplaza la propiedad `component` por dos nuevas:

   - `redirectTo`: Esta propiedad indica a Angular que redirija desde una ruta vacía a la
     Ruta de la `heroes-list`.

   - `pathMatch`: Esta propiedad le indica a Angular la cantidad de URL que debe coincidir. Para este
     tutorial, debes establecer esta propiedad en `full`. Esta estrategia se recomienda cuando
     tienes una cadena de caracteres vacía para una ruta. Para obtener más información sobre esta propiedad,
     consulta la [documentación de la API de ruta](/api/router/Route).

Ahora, cuando abras tu aplicación, se mostrará el componente `heroes-list` por defecto.

## Agregar una página 404

Es posible que un usuario intente acceder a una ruta que no hayas definido. Para dar cuenta de este comportamiento, la mejor práctica es mostrar una página 404. En esta sección, crearás una página 404 y actualizarás la configuración de tu ruta para mostrar esa página para cualquier ruta no especificada.

1. Desde la terminal crea un nuevo componente llamado `PageNotFound` usando la siguiente linea de comando:

   <code-example language="sh">
   ng generate component page-not-found
   </code-example>

1. Desde tu editor de código, abre el archivo `page-not-found.component.html` y reemplaza su contenido
   con el siguiente HTML:

   <code-example header="src/app/page-not-found/page-not-found.component.html" path="router-tutorial/src/app/page-not-found/page-not-found.component.html"></code-example>

1. Abre el archivo `app.module.ts`. En el arreglo de `imports` actualiza la sección `RouterModule` de la siguiente manera:

   <code-example header="src/app/app.module.ts" path="router-tutorial/src/app/app.module.ts" region="import-wildcard"></code-example>

   La nueva ruta usa una ruta, `**`. Esta ruta es la forma en que Angular identifica una ruta comodín. Cualquier ruta que no coincida con una ruta existente en tu configuración, utilizará esta ruta.

   <div class = "alert is-important">
    Observa que la ruta del comodín se coloca al final de la matriz. El orden de tus
    rutas es importante, ya que Angular aplica rutas en orden y usa la primera coincidencia que encuentra.
   </div>

Intenta navegar a una ruta no existente en tu aplicación, como `http://localhost:4200/powers`. Esta ruta no coincide con nada definido en tu archivo `app.module.ts`. Sin embargo, debido a que definiste una ruta comodín, la aplicación muestra automáticamente su componente `PageNotFound`.

## Próximos pasos

En este punto, tienes una aplicación básica que usa la funcionalidad de enrutamiento de Angular para cambiar
qué componentes puede ver el usuario según la dirección URL. Has ampliado estas funcionalidades
para incluir una redirección, así como una ruta comodín para mostrar una página 404 personalizada.

Para obtener más información sobre el enrutamiento, consulta los siguientes temas:

- [Enrutamiento y navegación en la aplicación](/guide/router)
- [API de enrutador](/api/router)
