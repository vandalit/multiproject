## Despliegue multiple de varios proyectos en un proyecto firebase.

Para ayudarte a desplegar tus tres proyectos en Firebase en un solo proyecto "multiprojectfirebase", voy a proporcionarte una guía detallada de configuración y despliegue. A continuación, detallo los pasos necesarios, desde la configuración de Firebase hasta el uso de los targets de hosting.

### Mapa de Jerarquía del Proyecto

**MULTIPROJECT**

- **main-project/**
- second-project/
  - .vscode/
  - node_modules/
  - public/
  - src/
  - .gitignore
  - bun.lockb
  - index.html
  - jsconfig.json
  - package.json
  - README.md
  - vite.config.js
- **third-project/**

------

### Pasos para Desplegar Múltiples Proyectos en Firebase

#### 1. **Configuración Inicial de Firebase**

1. **Instala Firebase CLI** (si aún no lo has hecho):

   ```
   npm install -g firebase-tools
   ```

2. **Inicia sesión en Firebase**:

   ```
   firebase login
   ```

3. **Inicializa el proyecto Firebase en el directorio raíz de MULTIPROJECT**: Desde la raíz de MULTIPROJECT, ejecuta:

   ```
   firebase init
   ```

   - Elige **Hosting** cuando te pregunte qué servicio deseas configurar.
   - Selecciona **Usar un proyecto existente** y elige **multiprojectfirebase**.
   - Cuando pregunte por la **carpeta pública**, solo coloca un nombre temporal (por ejemplo, `public`), ya que lo configuraremos manualmente para cada proyecto después.
   - Di "no" a las opciones de SPA si usas Vue Router en modo `hash` o según tu configuración.
   - Finaliza la configuración inicial.

#### 2. **Configura Firebase Hosting para Múltiples Proyectos**

Firebase te permite utilizar "targets" para desplegar múltiples sitios dentro de un solo proyecto. Configuraremos un target de hosting para cada subproyecto.

1. **Define los targets**: Ejecuta el siguiente comando para agregar un target para cada proyecto. Reemplaza `target-name` con nombres personalizados para cada subproyecto (por ejemplo, `mainProject`, `secondProject`, `thirdProject`).

   ```
   firebase target:apply hosting mainProject multiprojectfirebase-main
   firebase target:apply hosting secondProject multiprojectfirebase-second
   firebase target:apply hosting thirdProject multiprojectfirebase-third
   ```

   Esto asigna un "target" de Firebase Hosting para cada subproyecto en el mismo proyecto de Firebase.

#### 3. **Configura el Archivo `firebase.json`**

Edita el archivo `firebase.json` en la raíz de MULTIPROJECT para configurar múltiples "hosting sites" con los targets definidos.

```
{
  "hosting": [
    {
      "target": "mainProject",
      "public": "main-project/dist",
      "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
      "rewrites": [
        {
          "source": "**",
          "destination": "/index.html"
        }
      ]
    },
    {
      "target": "secondProject",
      "public": "second-project/dist",
      "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
      "rewrites": [
        {
          "source": "**",
          "destination": "/index.html"
        }
      ]
    },
    {
      "target": "thirdProject",
      "public": "third-project/dist",
      "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
      "rewrites": [
        {
          "source": "**",
          "destination": "/index.html"
        }
      ]
    }
  ]
}
```

En esta configuración:

- `"target"` es el nombre asignado al target de cada proyecto.
- `"public"` apunta al directorio `dist` de cada proyecto, que es donde se generará el build.

#### 4. **Configura Cada Proyecto Vue para el Build**

Para que Firebase sirva cada proyecto correctamente, es necesario que cada uno tenga un directorio `dist` en el cual se compilarán los archivos.

1. En el archivo `vite.config.js` de cada subproyecto (`main-project`, `second-project`, `third-project`), asegúrate de que el `build` genere la salida en la carpeta `dist`.

   ```
   import { defineConfig } from 'vite'
   import vue from '@vitejs/plugin-vue'
   
   export default defineConfig({
     plugins: [vue()],
     build: {
       outDir: 'dist'
     }
   })
   ```

2. Luego, desde la raíz de cada subproyecto, ejecuta:

   ```
   npm install
   npm run build
   ```

   Esto generará los archivos estáticos en `dist` para cada subproyecto.

#### 5. **Despliega en Firebase**

Una vez configurados los builds de cada proyecto y editado `firebase.json`, despliega cada proyecto en Firebase.

```
firebase deploy --only hosting:mainProject
firebase deploy --only hosting:secondProject
firebase deploy --only hosting:thirdProject
```

Alternativamente, para desplegar todos los proyectos en una sola línea:

```
firebase deploy --only hosting
```

#### 6. **Verifica los Despliegues**

Firebase generará URLs para cada subproyecto. En la consola de Firebase, deberías ver tres sitios de hosting separados, cada uno con una URL única para `mainProject`, `secondProject`, y `thirdProject`.

------

### Resumen

- Configuramos un target de hosting en Firebase para cada subproyecto.
- Editamos `firebase.json` para especificar las configuraciones de cada subproyecto.
- Aseguramos que cada proyecto Vue esté configurado para generar su build en `dist`.
- Desplegamos los proyectos usando Firebase CLI.

Con estos pasos, deberías poder desplegar exitosamente cada proyecto en su propio sitio de Firebase Hosting dentro de tu proyecto "multiprojectfirebase".








Error: Request to https://firebasehosting.googleapis.com/v1beta1/projects/-/sites/multiprojectfirebase-main/versions had HTTP Error: 404, Requested entity was not found.


### Solución: Verificar y Crear los Sitios en Firebase Hosting

1. **Confirma que los sitios existen en Firebase Hosting**: Abre la consola de Firebase y sigue estos pasos:

   - Ve a **Hosting** en tu proyecto `multiprojectfirebase`.
   - Asegúrate de que tienes configurados sitios separados para `multiprojectfirebase-main`, `multiprojectfirebase-second`, y `multiprojectfirebase-third`.

   Si no los ves, es probable que necesites crearlos manualmente.

2. **Crea los sitios adicionales (si es necesario)**: Para crear múltiples sitios dentro del mismo proyecto, sigue estos pasos:

   - En la consola de Firebase, ve a **Hosting** > **Agregar sitio**.
   - Crea un nuevo sitio con el nombre que usaste en tus targets de hosting, como `multiprojectfirebase-main`, `multiprojectfirebase-second`, y `multiprojectfirebase-third`.
   - Repite este proceso para cada subproyecto.

3. **Actualiza los Targets en Firebase CLI** (si es necesario): Una vez que hayas creado los sitios en la consola, vuelve a aplicar los targets para asegurar que están configurados correctamente. Desde el directorio raíz de tu proyecto `MULTIPROJECT`, ejecuta:

   ```
   firebase target:apply hosting mainProject multiprojectfirebase-main
   firebase target:apply hosting secondProject multiprojectfirebase-second
   firebase target:apply hosting thirdProject multiprojectfirebase-third
   ```

4. **Despliega de nuevo**: Ahora que los sitios deberían estar configurados correctamente en Firebase, intenta realizar el despliegue nuevamente:

   ```
   firebase deploy --only hosting
   ```

Esto debería solucionar el error, ya que ahora Firebase puede encontrar cada uno de los sitios (`multiprojectfirebase-main`, `multiprojectfirebase-second`, `multiprojectfirebase-third`).



<div align="center">
  <table>
    <tr>
      <td align="center"><h3>🎨</h3></td>
      <td align="center"><h3>💻</h3></td>
      <td align="center"><h3>📱</h3></td>
    </tr>
    <tr>
      <td align="center"><b>Diseño</b></td>
      <td align="center"><b>Desarrollo</b></td>
      <td align="center"><b>Mobile</b></td>
    </tr>
    <tr>
      <td align="center"><a href="https://multiprojectfirebase-main.web.app/">Servicios de diseño creativos</a></td>
      <td align="center"><a href="https://multiprojectfirebase-second.web.app/">Soluciones web modernas</a></td>
      <td align="center"><a href="https://multiprojectfirebase-third.web.app/">Apps multiplataforma</a></td>
    </tr>
  </table>
</div>


## finalmente:


Para agregar el directorio `dashboard-multiproject` y desplegarlo en la ruta principal `multiprojectfirebase.web.app` sin afectar tus configuraciones existentes, puedes seguir estos pasos:

1. **Configurar Firebase Hosting para múltiples sitios y el sitio raíz:** Vamos a agregar el nuevo directorio `dashboard-multiproject` como la raíz principal de tu proyecto de Firebase Hosting, y mantener los otros proyectos (`main-project`, `second-project`, `third-project`) en sus subdominios.

2. **Estructura del Proyecto:** Asegúrate de tener una estructura de carpetas similar a esta:

   ```
   MULTIPROJECT
   ├── dashboard-multiproject
   │   └── src
   │   └── dist (generado en el build)
   │   └── vite.config.js
   ├── main-project
   │   └── src
   │   └── dist (generado en el build)
   │   └── vite.config.js
   ├── second-project
   │   └── src
   │   └── dist (generado en el build)
   │   └── vite.config.js
   ├── third-project
   │   └── src
   │   └── dist (generado en el build)
   │   └── vite.config.js
   └── firebase.json
   ```

3. **Configurar `vite.config.js` para `dashboard-multiproject`:** En el archivo `vite.config.js` de `dashboard-multiproject`, configura `outDir` para que los archivos generados se almacenen en la carpeta `dist`:

   ```
   import { fileURLToPath, URL } from 'node:url'
   import { defineConfig } from 'vite'
   import vue from '@vitejs/plugin-vue'
   
   export default defineConfig({
     plugins: [vue()],
     resolve: {
       alias: {
         '@': fileURLToPath(new URL('./src', import.meta.url))
       }
     },
     build: {
       outDir: 'dist'
     }
   })
   ```

4. **Modificar `firebase.json` para incluir `dashboard-multiproject` en la ruta principal:** Configura `firebase.json` para que Firebase despliegue `dashboard-multiproject` en la ruta raíz (`multiprojectfirebase.web.app`) y mantenga los otros subproyectos en sus subdominios. El archivo `firebase.json` debería verse así:

   ```
   {
     "hosting": [
       {
         "target": "dashboard",
         "public": "dashboard-multiproject/dist",
         "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
         "rewrites": [
           { "source": "**", "destination": "/index.html" }
         ]
       },
       {
         "target": "mainProject",
         "public": "main-project/dist",
         "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
         "rewrites": [
           { "source": "**", "destination": "/index.html" }
         ]
       },
       {
         "target": "secondProject",
         "public": "second-project/dist",
         "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
         "rewrites": [
           { "source": "**", "destination": "/index.html" }
         ]
       },
       {
         "target": "thirdProject",
         "public": "third-project/dist",
         "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
         "rewrites": [
           { "source": "**", "destination": "/index.html" }
         ]
       }
     ]
   }
   ```

5. **Aplicar el Target de Hosting para `dashboard-multiproject`:** Define un nuevo target para `dashboard-multiproject` y aplícalo como el sitio raíz.

   ```
   firebase target:apply hosting dashboard multiprojectfirebase
   firebase target:apply hosting mainProject multiprojectfirebase-main
   firebase target:apply hosting secondProject multiprojectfirebase-second
   firebase target:apply hosting thirdProject multiprojectfirebase-third
   ```

   - Aquí, `multiprojectfirebase` es el nombre principal de tu proyecto, y `dashboard` es el target para el nuevo directorio.

6. **Desplegar a Firebase:** Ejecuta el comando de despliegue para publicar todas las configuraciones de hosting.

   ```
   firebase deploy --only hosting
   ```

### Explicación de la Configuración

- **`target: "dashboard"`:** Define `dashboard` como el target que apunta al sitio raíz `multiprojectfirebase.web.app`, mientras los demás targets (`mainProject`, `secondProject`, `thirdProject`) apuntan a sus subdominios.
- **`public: "dashboard-multiproject/dist"`:** Configura `dashboard-multiproject` para que se sirva en la ruta raíz.
- **`rewrites` en cada sección:** Asegura que todas las rutas dentro de cada proyecto redirijan a `index.html`, lo cual es importante para aplicaciones Vue de una sola página.

### Consideraciones Finales

- Esta configuración permitirá que `dashboard-multiproject` esté disponible en `multiprojectfirebase.web.app`, mientras que los demás proyectos seguirán en sus respectivos subdominios.
- Asegúrate de ejecutar `npm run build` en cada subproyecto antes de desplegar para que los archivos `dist` estén actualizados.
- Si haces cambios en cualquiera de los proyectos, tendrás que volver a hacer `build` y `deploy`.

Siguiendo estos pasos deberías poder desplegar los cuatro proyectos, manteniendo `dashboard-multiproject` en la ruta principal y los otros en sus subdominios.



<div align="center">
  <table>
    <tr>
      <td align="center"><h3>💻</h3></td>
    </tr>
    <tr>
      <td align="center"><b>Web</b></td>
    </tr>
    <tr>
      <td align="center"><a href="https://multiprojectfirebase.web.app/">Dashboard</a></td>
    </tr>
  </table>
</div>