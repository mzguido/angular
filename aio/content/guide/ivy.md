# Angular Ivy

Ivy es el nombre en clave para la [siguiente generación de pipeline de compilación y renderizado](https://blog.angular.io/a-plan-for-version-8-0-and-ivy-b3318dfc19f7).
Con el lanzamiento de la versión 9 de Angular, el nuevo compilador y entorno de ejecución de instrucciones se utiliza por defecto en lugar de View Engine, el viejo compilador y entorno de ejecución de instrucciones.

<div class="alert is-helpful">

Puedes aprender mas acerca del [Compilador](https://www.youtube.com/watch?v=anphffaCZrQ) y [Entorno de ejecución de instrucciones](https://www.youtube.com/watch?v=S0o-4yc2n-8) en estos videos de nuestro equipo.

</div>

{@a aot-and-ivy}

## AOT y Ivy

La compilación AOT con Ivy es mas rápida y debe ser usada por defecto.
En el archivo de configuración de tu espacio de trabajo (`angular.json`), establece las opciones de compilación predeterminadas para que tu proyecto use siempre la compilación AOT.
Cuando uses una aplicación de internacionalización (i18n) con Ivy, la [fusión de traducciones](guide/i18n#merge) también requiere el uso de compilación AOT.

<code-example language="json" header="angular.json">

{
  "projects": {
    "my-existing-project": {
      "architect": {
        "build": {
          "options": {
            ...
            "aot": true,
          }
        }
      }
    }
  }
}
</code-example>

## Ivy y las librerías

Las aplicaciones Ivy se pueden crear con librerías creadas con el compilador View Engine.
Esta compatibilidad es proporcionada por una herramienta de Angular conocida como compilador de compatibilidad (`ngcc`).
Los comandos de la CLI ejecutar `ngcc` según es necesario en la compilación de Angular.

Para mas información sobre cómo publicar una librería, puedes leer [Publicando tu Librería](guide/creating-libraries#publishing-your-library).

{@a maintaining-library-compatibility}

### Manteniendo la compatibilidad de una librería

Si tu eres el autor de una librería, puedes seguir usando el compilador de View Engine a partir de la version 9.
Haciendo que todas las librerías continúen usando View Engine, mantendrás la compatibilidad con las aplicaciones v9 predeterminadas que usan Ivy, así como con las aplicaciones que han optado por seguir usando View Engine.

Puedes leer la guía [Creando Librerías](guide/creating-libraries) para más información sobre cómo compilar o agrupar tu Librería Angular.
Cuando utilizas las herramientas integradas en el Angular CLI o `ng-packagr`, tu biblioteca siempre se construirá de la manera correcta automáticamente.

{@a ivy-and-universal-app-shell}

## Ivy y Universal/Armazón de la aplicación

En la versión 9, el constructor del servidor que se usa para [App shell](guide/app-shell) y [Angular Universal](guide/universal) tiene la opción `bundleDependencies` habilitada por defecto.
Si optas por no agrupar dependencias, deberás ejecutar el compilador de compatibilidad Angular independiente (`ngcc`). Esto es necesario porque, de lo contrario, Node no podrá resolver la versión de Ivy de los paquetes.

Puedes ejecutar el comando `ngcc` después de cada instalación de node_modules agregando el [npm script](https://docs.npmjs.com/misc/scripts) llamado `postinstall` :

<code-example language="json" header="package.json">
{
  "scripts": {
    "postinstall": "ngcc"
  }
}
</code-example>

<div class="alert is-important">

- El script `postinstall` puede correr después de cada instalación de `node_modules`, incluido las ejecutadas por `ng update` y `ng add`.
- No uses `--create-ivy-entry-points` con este propósito, Node no resuelve la version de Ivy de los paquetes de forma correcta.

</div>

{@a opting-out-of-angular-ivy}

## Deshabilitando Ivy en la version 9

A partir de la versión 9 de Angular, Ivy es la opción por defecto.
Para compatibilidad con los flujos de trabajo actuales durante el proceso de actualización, puedes elegir deshabilitar Ivy y continuar usando el compilar anterior (View Engine).

<div class="alert is-helpful">

Antes de deshabilitar Ivy, asegúrate de chequear las recomendaciones de debugging en [guía de compatibilidad con Ivy](guide/ivy-compatibility#debugging).

</div>

Para deshabilitar Ivy, cambia la opción `angularCompilerOptions` en la configuración de tu proyecto TypeScript, la cual, se encuentra comúnmente en el archivo `tsconfig.app.json` en la raíz de tu espacio de trabajo.

El valor de la bandera `enableIvy` se establece en `true` por defecto, en la version 9.

En el siguiente ejemplo puedes ver como establecer la opción `enableIvy` en `false` para no usar Ivy.

<code-example language="json" header="tsconfig.app.json">
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/app",
    "types": []
  },
  "files": [
    "src/main.ts",
    "src/polyfills.ts"
  ],
  "include": [
    "src/**/*.d.ts"
  ],
  "angularCompilerOptions": {
    "enableIvy": false
  }
}
</code-example>

<div class="alert is-important">

Si deshabilitas Ivy, es posible que también desees reconsiderar si hacer compilación AOT para el desarrollo de tu aplicación, como se describe [arriba](#aot-and-ivy).

Para revertir la compilación por defecto, puedes establecer la opción de build `aot: false` en tu archivo de configuración `angular.json`.

</div>

Si deshabilitas Ivy y el proyecto usa internalización, también puedes remover el componente en tiempo de ejecución `@angular/localize` del archivo de polyfills del proyecto, este se encuentra localizado por defecto en `src/polyfills.ts`.

Para removerlo, elimina la línea `import '@angular/localize/init';` del archivo de polyfills.

<code-example language="typescript" header="polyfills.ts">
/***************************************************************************************************
 * Carga `$localize` en un scope global - usando el tag i18n en el Angular templates.
 */
import '@angular/localize/init';
</code-example>

{@a using-ssr-without-angular-ivy}

### Using SSR without Ivy

Si optas por no usar Ivy y tu usas [Angular Universal](guide/universal) para renderizar tu aplicación Angular en el servidor, también debe cambiar la forma en que funciona el server bootstrapping.

El siguiente ejemplo muestra cómo debes modificar el archivo `server.ts` para proveer el `AppServerModuleNgFactory` en el módulo de arranque.

- Importar `AppServerModuleNgFactory` desde el archivo virtual `app.server.module.ngfactory`.
- Establecer `bootstrap: AppServerModuleNgFactory` en la llamada a `ngExpressEngine`.

<code-example language="typescript" header="server.ts">
import 'zone.js/dist/zone-node';

import { ngExpressEngine } from '@nguniversal/express-engine';
import * as express from 'express';
import { join } from 'path';

import { APP_BASE_HREF } from '@angular/common';

import { AppServerModuleNgFactory } from './src/app/app.server.module.ngfactory';

// La aplicación Express se exporta para que pueda ser utilizada por funciones sin servidor.
export function app() {
const server = express();
const distFolder = join(process.cwd(), 'dist/ivy-test/browser');

// Our Universal express-engine (found @ https://github.com/angular/universal/tree/master/modules/express-engine)
server.engine('html', ngExpressEngine({
bootstrap: AppServerModuleNgFactory,
}));

server.set('view engine', 'html');
server.set('views', distFolder);

// Ejemplo URLs de la API Express Rest
// app.get('/api/**', (req, res) => { });
// Serve static files from /browser
server.get('*.*', express.static(distFolder, {
maxAge: '1y'
}));

// Todas las rutas regulares utilizan el motor Universal
server.get('*', (req, res) => {
res.render('index', { req, providers: [{ provide: APP_BASE_HREF, useValue: req.baseUrl }] });
});

return server;
}

function run() {
const port = process.env.PORT || 4000;

// Inicia el servidor de Node
const server = app();
server.listen(port, () => {
console.log(`Node Express server listening on http://localhost:${port}`);
});
}

// Webpack remplazará 'require' con '**webpack_require**'
// '**non_webpack_require**' es un proxy para Node 'require'
// El siguiente código es para garantizar que el servidor se ejecute solo cuando no se requiera el paquete.
declare const **non_webpack_require**: NodeRequire;
const mainModule = **non_webpack_require**.main;
if (mainModule && mainModule.filename === __filename) {
    run();
}

export * from './src/main.server';
</code-example>
