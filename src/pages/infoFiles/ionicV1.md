---
    title: 'Contenido Pagina'
    layout: '../../layouts/Layout.astro'
---

# Ionic Mini Manual

Este manual interactivo te ayudará a comprender y utilizar los comandos y la estructura básica de un proyecto Ionic. ¡Explora cada sección para aprender a crear y gestionar tu aplicación!

---

## Tabla de Contenidos

-   [Comandos Principales](#comandos-principales)
-   [Estructura del Proyecto](#estructura-del-proyecto)
-   [Creación de Módulo, Componente y Page](#creación-de-módulo-componente-y-page)
-   [Shared Module](#shared-module)
-   [Navegación entre Componentes](#navegación-entre-componentes)
-   [Configuración de TypeScript](#configuración-de-typescript)
-   [Conclusión](#conclusión)

---

## Comandos Principales

Utiliza los siguientes comandos para iniciar y gestionar tu aplicación Ionic:

```bash
# Crear una nueva aplicación
ionic start my-app

# Ejecutar la aplicación
ionic serve

# Crear un módulo con soporte para rutas
ionic g m my-module --routing

# Crear un componente (sin archivo de pruebas)
ionic g c my-component --spec=false
```

_Notas:_

-   `ionic start` crea una nueva aplicación.
-   `ionic serve` inicia el servidor de desarrollo.
-   `ionic g m` genera un módulo; al usar `--routing`, se añade soporte para rutas.
-   `ionic g c` crea un componente; la opción `--spec=false` omite la creación del archivo de pruebas.

---

## Estructura del Proyecto

A continuación, se muestra una estructura de carpetas recomendada para organizar tu aplicación:

```
src/
├── app/
│   ├── module/
│   │   ├── components/
│   │   └── pages/
│   ├── models/        # Interfaces y tipos
│   ├── services/
│   └── shared/
│       ├── component/
│       └── pages/
│           └── not-found/
```

> **Tip:** Utiliza esta estructura como guía para mantener tu proyecto limpio y organizado.

---

## Creación de Módulo, Componente y Page

### 1. Generar un Módulo

-   **Importar IonContent** en el módulo:
    ```typescript
    import { IonContent } from "@ionic/angular/standalone";
    ```

### 2. Generar una Página (Componente)

-   **Configuración:**
    -   La página se crea como _no standalone_ (standalone: `false`).
    -   Debe declararse en el módulo correspondiente.
-   **Registrar la Ruta en el Routing Module:**
    ```typescript
    { path: '', component: HomeComponent },
    ```
-   **Registrar la Ruta en App Routes** (para carga perezosa):
    ```typescript
    {
      path: 'home',
      loadChildren: () => import('./home/home.module').then(module => module.HomeModule),
    }
    ```
-   **Vista:** Utiliza `<ion-content>` como contenedor principal:
    ```html
    <ion-content>
    	<!-- Contenido de la página -->
    </ion-content>
    ```

### 3. Generar un Componente (para usar en el Módulo)

-   **Configuración:**
    -   El componente se crea como _no standalone_.
    -   Debe declararse en el módulo correspondiente.

<details>
  <summary><strong>Ver ejemplo interactivo</strong></summary>

![Ejemplo de Componente](https://via.placeholder.com/300?text=Ejemplo+de+Componente)

</details>

---

## Shared Module

Los componentes compartidos que se utilizarán en toda la aplicación se organizan en un _Shared Module_:

1. **Generar el Módulo** (sin routing).
2. **Crear un Componente**:
    - Se crea como _no standalone_.
    - Se declara y se exporta en el módulo.
3. **Uso del Componente:**
    - Importa el `SharedModule` en el módulo donde necesites el componente.
4. **Página NotFound** (componente standalone):
    - _Nota:_ Un componente standalone funciona como un módulo.
    - Genera el componente.
    - En el decorador `@Component`, importa `IonContent` y `RouterModule`.
    - **Registrar la Ruta en App Routes** para manejar rutas no existentes:
        ```typescript
        {
          path: '**', // cualquier ruta que no exista
          loadComponent: () => import('./shared/pages/not-found/not-found.component')
                             .then(module => module.NotFoundComponent)
        }
        ```
    - **Vista:** Redirecciona utilizando `routerLink`:
        ```html
        <ion-content>
        	<button class="button-back" [routerLink]="['/']">Back</button>
        </ion-content>
        ```

---

## Navegación entre Componentes

Para navegar entre diferentes páginas o componentes:

1. **Configuración:**
    - En el módulo correspondiente (o en un componente standalone), importa `RouterModule`.
2. **Uso de [routerLink]:**
    - Ejemplo:
        ```html
        <a [routerLink]="['/store/home']">Ir a Home</a>
        ```

---

## Configuración de TypeScript

Para permitir la declaración de variables sin inicializarlas previamente, ajusta las opciones en el archivo `tsconfig.json` dentro de `compilerOptions`:

```json
{
	"compilerOptions": {
		"strictPropertyInitialization": false,
		"strictNullChecks": false
	}
}
```

---

## Conclusión

Este manual interactivo te ofrece una guía clara y estructurada para trabajar con Ionic, desde la creación de la aplicación hasta la organización de módulos y navegación. ¡Explora, experimenta y sigue creando aplicaciones impresionantes!

_Manual creado y optimizado para mejorar la experiencia visual e interactiva del desarrollador._

```

---

Puedes copiar este contenido en un archivo con extensión `.md` para disponer de un mini manual más atractivo y funcional. ¿Te gustaría agregar o modificar algún otro detalle?
```
