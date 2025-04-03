---
    title: 'Contenido Pagina'
    layout: '../../layouts/layout.astro'
---

# 🚀 Guía Rápida de Ionic

¡Bienvenido a esta guía práctica de Ionic! Aquí encontrarás los conceptos esenciales, comandos útiles y mejores prácticas para desarrollar aplicaciones con Ionic.

## 📂 **Estructura de Proyecto**

```plaintext
|src/
    |app/
        |modules/
            |components/
            |pages/
        |models/      # Interfaces
        |services/
        |shared/
            |components/
            |pages/
                |not-found/
```

## 🛠 **Comandos Principales**

| Comando                               | Descripción                                |
| ------------------------------------- | ------------------------------------------ |
| `ionic start my-app`                  | Crea una nueva aplicación Ionic.           |
| `ionic serve`                         | Inicia el servidor de desarrollo.          |
| `ionic g m my-module --routing`       | Genera un módulo con sistema de rutas.     |
| `ionic g c my-component --spec=false` | Crea un componente sin archivo de pruebas. |

## 🧩 **Módulos, Componentes y Páginas**

### 1. **Generar un Módulo**

```typescript
import { IonContent } from "@ionic/angular/standalone";

// Ejemplo de módulo con rutas
@NgModule({
	imports: [IonContent],
})
export class HomeModule {}
```

### 2. **Crear una Página**

-   **Página no standalone**: Declárala en el módulo y registra la ruta:
    ```typescript
    // En el routing.module.ts
    { path: '', component: HomeComponent }
    ```
-   **Vista**:
    ```html
    <ion-content>
    	<!-- Contenido aquí -->
    </ion-content>
    ```

### 3. **Componentes Compartidos**

1. Genera un módulo compartido:  
   `ionic g m shared --spec=false`
2. Crea componentes dentro del módulo y expórtalos:
    ```typescript
    @NgModule({
    	declarations: [SharedComponent],
    	exports: [SharedComponent],
    })
    export class SharedModule {}
    ```

---

## 🔄 **Navegación entre Páginas**

### Enrutamiento Básico

```typescript
// app-routing.module.ts
{
  path: 'home',
  loadChildren: () => import('./home/home.module').then(m => m.HomeModule),
}
```

### Uso de `routerLink`

```html
<ion-button [routerLink]="['/home']">Ir a Home</ion-button>
```

### **Página 404 (NotFound)**

-   **Componente Standalone**:
    ```typescript
    @Component({
    	standalone: true,
    	imports: [IonContent, RouterModule],
    })
    export class NotFoundComponent {}
    ```
-   **Registrar ruta**:
    ```typescript
    { path: '**', loadComponent: () => import('./not-found.component').then(m => m.NotFoundComponent) }
    ```

---

## ⚙ **Configuración Avanzada**

### Deshabilitar Inicialización Estricta

En `tsconfig.json`:

```json
{
	"compilerOptions": {
		"strictPropertyInitialization": false,
		"strictNullChecks": false
	}
}
```

---

## 💡 **Consejos**

-   Usa `--spec=false` para omitir archivos de prueba al generar componentes.
-   Los componentes `standalone` actúan como módulos (no necesitan ser declarados en uno).
-   Organiza servicios y modelos en carpetas dedicadas para mantener limpio el proyecto.

---
