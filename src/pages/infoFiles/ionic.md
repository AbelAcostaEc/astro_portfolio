---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---

# 📱 Ionic

A continuación, se presentan los comandos más utilizados en **Ionic** para la creación y gestión de proyectos.

## 🚀 1. Creación y ejecución de una aplicación

```sh
ionic start my-app
```

> Crea una nueva aplicación en Ionic.

```sh
ionic serve
```

> Inicia un servidor de desarrollo y ejecuta la aplicación en el navegador.

---

## 📌 2. Generación de módulos, componentes y más

| Comando                                   | Descripción                                                          |
| ----------------------------------------- | -------------------------------------------------------------------- |
| `ionic g m my-module --routing`           | Crea un **módulo** con configuración de rutas.                       |
| `ionic g c my-component --spec=false`     | Genera un **componente**, omitiendo el archivo de pruebas (`.spec`). |
| `ionic g i models/model`                  | Crea una **interfaz** en la carpeta `models/`.                       |
| `ionic g s services/test`                 | Genera un **servicio** en la carpeta `services/`.                    |
| `ionic g p shared/pipes/miPipe`           | Crea un **pipe** en la carpeta `shared/pipes/`.                      |
| `ionic g d shared/directives/miDirective` | Genera una **directiva** en `shared/directives/`.                    |
| `ionic g g shared/guards/myGuard`         | Crea un **guard** en `shared/guards/`.                               |
| `ionic g page pages/myPage`               | Crea una **pagina** con su ruta y modulo                             |

---

## 📂 3. Estructura del Proyecto en Ionic

```plaintext
src/
│── app/
│   ├── module/          # Módulos principales de la aplicación
│   │   ├── components/  # Componentes reutilizables
│   │   ├── pages/       # Páginas de la aplicación
│   ├── models/          # Interfaces y modelos de datos
│   ├── services/        # Servicios para manejo de datos y lógica de negocio
│   ├── shared/          # Recursos compartidos
│   │   ├── component/   # Componentes compartidos
│   │   ├── pages/       # Páginas generales
│   │   │   ├── not-found/  # Página 404 (No encontrada)
```

## ⚡ 4. Creación de Módulo, Página y Componente en Ionic

### 🛠️ 1. Creación de un Módulo

1. Genera un módulo con el siguiente comando:
    ```sh
    ionic g m nombre-del-modulo --routing
    ```
2. Importa `IonContent` en el módulo si lo necesitas:

    ```ts
    import { IonContent } from "@ionic/angular/standalone";
    ```

### 📄 2. Creación de una Página (Componente)

3. Genera la página con el siguiente comando:
    ```sh
    ionic g page pages/home --standalone=false
    ```
4. **Declarar la página en el módulo** correspondiente:

    ```ts
    import { HomeComponent } from "./home.component";

    @NgModule({
    	declarations: [HomeComponent],
    	imports: [CommonModule, IonicModule],
    })
    export class HomeModule {}
    ```

5. **Registrar la ruta de la página en su módulo de enrutamiento** (`home-routing.module.ts`):

    ```ts
    import { NgModule } from "@angular/core";
    import { RouterModule, Routes } from "@angular/router";
    import { HomeComponent } from "./home.component";

    const routes: Routes = [{ path: "", component: HomeComponent }];

    @NgModule({
    	imports: [RouterModule.forChild(routes)],
    	exports: [RouterModule],
    })
    export class HomeRoutingModule {}
    ```

6. **Registrar la ruta en `app.routes.ts` para cargar el módulo dinámicamente**:
    ```ts
    {
        path: 'home',
        loadChildren: () => import('./pages/home/home.module').then(m => m.HomeModule),
    },
    ```
7. **Vista de la página (`home.page.html`)**  
   Se usa `<ion-content>` como contenedor principal:
    ```html
    <ion-content>
    	<h1>Bienvenido a Home</h1>
    </ion-content>
    ```

## 🌍 5. **Shared Module en Ionic** (Módulo Compartido)

### 🏗️ **1. Creación del Shared Module**

1. Genera el módulo sin `--routing`:
    ```sh
    ionic g m shared/shared
    ```
2. **Estructura recomendada**:
    ```plaintext
    src/
    ├── app/
    │   ├── shared/
    │   │   ├── shared.module.ts  # Módulo compartido
    │   │   ├── components/        # Componentes reutilizables
    │   │   ├── pages/             # Páginas compartidas
    │   │   │   ├── not-found/     # Página 404
    ```

---

### 🧩 **2. Creación de un Componente Compartido**

1. Genera el componente:
    ```sh
    ionic g c shared/components/mi-componente --standalone=false
    ```
2. **Declararlo y exportarlo en `shared.module.ts`**:

    ```ts
    import { NgModule } from "@angular/core";
    import { CommonModule } from "@angular/common";
    import { IonicModule } from "@ionic/angular";
    import { MiComponenteComponent } from "./components/mi-componente.component";

    @NgModule({
    	declarations: [MiComponenteComponent],
    	imports: [CommonModule, IonicModule],
    	exports: [MiComponenteComponent], // Exportarlo para usarlo en otros módulos
    })
    export class SharedModule {}
    ```

---

### 📌 **3. Uso del Componente Compartido en otro Módulo**

Para usar los **componentes del Shared Module**, impórtalo en el módulo donde los necesites.

```ts
import { SharedModule } from "../shared/shared.module";

@NgModule({
	imports: [SharedModule],
})
export class HomeModule {}
```

Luego, puedes usar el componente en la vista:

```html
<app-mi-componente></app-mi-componente>
```

---

### 🚫 **4. Creación de Página Not Found (Standalone Component)**

1. Genera el componente standalone:
    ```sh
    ionic g c shared/pages/not-found --standalone=true
    ```
2. **Modificar `not-found.component.ts`** para importar `IonContent` y `RouterModule`:

    ```ts
    import { Component } from "@angular/core";
    import { IonContent } from "@ionic/angular";
    import { RouterModule } from "@angular/router";

    @Component({
    	selector: "app-not-found",
    	standalone: true,
    	imports: [IonContent, RouterModule],
    	templateUrl: "./not-found.component.html",
    	styleUrls: ["./not-found.component.scss"],
    })
    export class NotFoundComponent {}
    ```

3. **Registrar la ruta en `app.routes.ts`** para manejar rutas inexistentes:

    ```ts
    {
        path: '**', // Cualquier ruta no existente
        loadComponent: () => import('./shared/pages/not-found/not-found.component').then(m => m.NotFoundComponent)
    }
    ```

4. **Vista de `not-found.component.html` con redirección al home**:
    ```html
    <ion-content>
    	<h1>Página no encontrada</h1>
    	<button class="button-back" [routerLink]="['/']">Volver al inicio</button>
    </ion-content>
    ```

## 🚀 6. **Navegación entre Componentes (Pages) en Ionic**

En Ionic, la navegación se maneja con `RouterModule`, `routerLink` y `Router.navigate()`.

---

### 📌 **1. Configuración Inicial**

### a) **Si usas Módulos (No Standalone)**

Debes importar el `RouterModule` en el módulo de la página.

```ts
import { NgModule } from "@angular/core";
import { RouterModule } from "@angular/router";

@NgModule({
	imports: [RouterModule],
})
export class HomeModule {}
```

### b) **Si es un Componente Standalone**

Importa `RouterModule` en el `@Component` de la página:

```ts
import { Component } from "@angular/core";
import { RouterModule } from "@angular/router";

@Component({
	selector: "app-home",
	standalone: true,
	imports: [RouterModule],
	templateUrl: "./home.component.html",
})
export class HomeComponent {}
```

---

### 📌 **2. Métodos para Navegar**

### 🔗 **a) Navegación en el Template con `routerLink`**

✅ **Ejemplo de botón para navegar a `/store/home`**

```html
<button [routerLink]="['/store/home']">Ir a Store</button>
```

✅ **Ejemplo de navegación dinámica a `/home/article/{id}`**

```html
<button [routerLink]="['/home/article', article.id]">Ver Artículo</button>
```

---

### 📌 **b) Navegación en TypeScript con `Router.navigate()`**

Si necesitas redirigir desde el código, usa `Router.navigate()`:

```ts
import { Component } from "@angular/core";
import { Router } from "@angular/router";

@Component({
	selector: "app-example",
	templateUrl: "./example.component.html",
})
export class ExampleComponent {
	constructor(private router: Router) {}

	goToStore() {
		this.router.navigate(["/store/home"]);
	}

	goToArticle(articleId: number) {
		this.router.navigate(["/home/article", articleId]);
	}
}
```

✅ **Ejemplo de uso en el template:**

```html
<button (click)="goToStore()">Ir a Store</button> <button (click)="goToArticle(123)">Ver Artículo</button>
```

## 🛠️ 7. **Permitir Declarar Variables sin Inicializar en TypeScript**

Por defecto, TypeScript impone reglas estrictas para evitar errores relacionados con variables no inicializadas. Sin embargo, puedes deshabilitar esta verificación en el archivo `tsconfig.json`.

---

### 📌 **1. Modificar `tsconfig.json`**

Abre el archivo `tsconfig.json` y en la sección `compilerOptions`, agrega o modifica las siguientes opciones:

```json
{
	"compilerOptions": {
		"strictPropertyInitialization": false,
		"strictNullChecks": false
	}
}
```


## 📌 8. **Uso de Interfaces en Ionic con TypeScript**  

Las interfaces ayudan a definir la estructura de los datos en TypeScript y permiten escribir código más limpio y seguro.  

---

### 🚀 **1. Crear Interfaces**  

### 🔹 **a) Interface Específica en `models/home.models.ts`**  

Aquí agrupamos las interfaces dentro de un **namespace**, lo que facilita la organización y reutilización.  

```ts
export namespace ModelsHome {
  export interface Article {
    id?: string;
    title: string;
    description: string;
    image: {
      url: string;
      alt: string;
    };
  }
}
```

---

### 🔹 **b) Interface Principal en `models/models.ts`**  

En este archivo, agrupamos todas las interfaces en un **namespace global**, lo que nos permite importar una sola vez en los componentes.  

```ts
import { ModelsHome } from 'src/app/models/home.models';

export namespace Models {
  export import Home = ModelsHome;
}
```

---

### 🚀 **2. Uso de la Interface en un Componente**  

Ahora podemos utilizar la interfaz en cualquier componente.  

### 🔹 **a) Importar la Interface en el Componente**  

```ts
import { Component } from '@angular/core';
import { Models } from 'src/app/models/models';

@Component({
  selector: 'app-article',
  templateUrl: './article.component.html',
})
export class ArticleComponent {
  article: Models.Home.Article = {
    id: '1',
    title: 'Ionic 7: Guía Completa',
    description: 'Aprende todo sobre Ionic 7 con esta guía detallada.',
    image: {
      url: 'https://example.com/ionic.jpg',
      alt: 'Guía de Ionic'
    }
  };
}
```

---

### 🔹 **b) Mostrar en la Vista (HTML)**  

Podemos usar la variable `article` en la plantilla.  

```html
<ng-container *ngIf="article">
  <h2>{{ article.title }}</h2>
  <p>{{ article.description }}</p>
  <img [src]="article.image.url" [alt]="article.image.alt">
</ng-container>
```

## 🎯 9. **Manejo de Eventos en Ionic**  

Los eventos en Ionic permiten capturar interacciones del usuario, como clics en botones, cambios en formularios o desplazamientos.  

---

### 📌 **1. Capturar un Evento en la Vista (HTML)**  

Para capturar un evento como un `click`, se usa la sintaxis `(evento)="función()"`.  
Si se necesita el objeto del evento, se pasa `$event` como argumento.  

```html
<button (click)="viewItem(item)">Ver</button>
```

---

### 📌 **2. Definir la Función en el Componente (TypeScript)**  

En el archivo **`component.ts`**, se crea la función para manejar el evento.  

```ts
import { Component } from '@angular/core';
import { Models } from 'src/app/models/models';

@Component({
  selector: 'app-article',
  templateUrl: './article.component.html',
})
export class ArticleComponent {
  viewItem(item: Models.Home.Article): void {
    console.log('View Item:', item);
  }
}
```

---

### 📌 **3. Capturar un Evento con `$event`**  

Si necesitas capturar información del evento, usa `$event` en la vista y **tipa** el evento en la función.  

### 🔹 **Vista (HTML)**
```html
<input type="text" (input)="onInputChange($event)">
```

### 🔹 **Componente (TypeScript)**
```ts
onInputChange(event: Event): void {
  const inputElement = event.target as HTMLInputElement;
  console.log('Valor ingresado:', inputElement.value);
}
```


## 🗣️ 10. **Comunicación entre Componentes con `@Input()` y `@Output()`**  

En Ionic (y Angular), la comunicación entre componentes se logra principalmente mediante los **decoradores `@Input()`** y **`@Output()`**.  

---

### 📌 **1. Uso de `@Input()` (Pasar Variables de un Componente Padre a un Componente Hijo)**  

### 🔹 **a) Definir la variable en el Componente Hijo**  

Primero, en el **componente hijo**, se importa `@Input()` y se declara la variable que se recibirá del componente padre.  

```ts
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
})
export class HeaderComponent {
  @Input() title: string;
}
```

### 🔹 **b) Pasar la variable desde el Componente Padre**  

En el **componente padre**, puedes pasar la variable al componente hijo de dos maneras:

- **Pasar un valor fijo:**
```html
<app-header title="hola"></app-header>
```

- **Pasar una variable (usando corchetes para enlazar dinámicamente):**
```html
<app-header [title]="title"></app-header>
```

---

### 📌 **2. Uso de `@Output()` (Enviar Eventos del Componente Hijo al Padre)**  

### 🔹 **a) Definir el `@Output()` en el Componente Hijo**  

En el **componente hijo**, definimos un `EventEmitter` que emitirá el evento hacia el componente padre.  

```ts
import { Component, EventEmitter, OnInit, Output } from '@angular/core';

@Component({
  selector: 'app-article',
  templateUrl: './article.component.html',
})
export class ArticleComponent {
  @Output() clickEvent = new EventEmitter<any>();

  triggerEvent(item: any): void {
    this.clickEvent.emit(item);
  }
}
```

### 🔹 **b) Capturar el Evento en el Componente Padre**  

En el **componente padre**, escuchamos el evento con `(clickEvent)` y ejecutamos una función cuando se emita el evento.  

```html
<app-article (clickEvent)="customFunction($event)"></app-article>
```

Y en el **componente padre** (TypeScript), definimos la función que manejará el evento.  

```ts
export class ParentComponent {
  customFunction(item: any) {
    console.log('Item recibido:', item);
  }
}
```


# 11. Servicios en **Ionic** con **Angular** y Consumir una **API Externa**

## **1. Crear un Servicio en Ionic/Angular**

En **Ionic/Angular**, los servicios se utilizan para gestionar la lógica de negocio y las solicitudes HTTP. Vamos a crear un servicio que realice peticiones a una API externa.

### **Paso 1: Crear el Servicio**

Abre la terminal en tu proyecto Ionic y ejecuta el siguiente comando para crear un servicio:

```bash
ionic generate service services/api
```

Esto creará el archivo `src/app/services/api.service.ts`, donde implementarás la lógica del servicio.

### **Paso 2: Modificar el Servicio para Realizar Peticiones HTTP**

En el archivo `api.service.ts`, importa el `HttpClient` de Angular para realizar las solicitudes HTTP. A continuación, un ejemplo de cómo hacerlo:

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {

  private apiUrl = 'https://jsonplaceholder.typicode.com';  // Cambia esta URL por la de tu API externa

  constructor(private http: HttpClient) { }

  // Método para obtener datos de la API
  getPosts(): Observable<any> {
    return this.http.get(`${this.apiUrl}/posts`);
  }

  // Método para obtener un post por ID
  getPostById(id: number): Observable<any> {
    return this.http.get(`${this.apiUrl}/posts/${id}`);
  }

  // Método para crear un post
  createPost(data: any): Observable<any> {
    return this.http.post(`${this.apiUrl}/posts`, data);
  }
}
```

### **Paso 3: Usar el Servicio en un Componente**

Para consumir el servicio en un componente, sigue estos pasos:

1. Crea un componente nuevo (si es necesario):

    ```bash
    ionic generate page home
    ```

2. En el archivo `home.page.ts`, importa el servicio y consúmelo en el `ngOnInit`:

```typescript
import { Component, OnInit } from '@angular/core';
import { ApiService } from '../services/api.service';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage implements OnInit {

  posts: any[] = [];

  constructor(private apiService: ApiService) {}

  ngOnInit() {
    this.loadPosts();
  }

  loadPosts() {
    this.apiService.getPosts().subscribe((data) => {
      this.posts = data;
    });
  }
}
```

3. En `home.page.html`, muestra los datos obtenidos:

```html
<ion-header>
  <ion-toolbar>
    <ion-title>
      Posts
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <ion-list>
    <ion-item *ngFor="let post of posts">
      <ion-label>
        <h2>{{ post.title }}</h2>
        <p>{{ post.body }}</p>
      </ion-label>
    </ion-item>
  </ion-list>
</ion-content>
```

---

## **2. Consumir una API Externa en Ionic/Angular**

En el ejemplo anterior, ya hemos creado un servicio que consume una API externa usando el servicio `HttpClient`. A continuación, desglosamos el proceso para consumir cualquier API externa en Ionic/Angular.

### **Paso 1: Importar y Configurar `HttpClientModule`**

Asegúrate de que `HttpClientModule` esté importado en tu `app.module.ts`:

```typescript
import { HttpClientModule } from '@angular/common/http';
import { NgModule } from '@angular/core';
import { IonicModule } from '@ionic/angular';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    IonicModule.forRoot(),
    HttpClientModule, // Agrega HttpClientModule aquí
  ],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

### **Paso 2: Realizar una Solicitud `GET` a una API Externa**

El método `get` de `HttpClient` se utiliza para obtener datos desde una API externa. Ejemplo:

```typescript
getPosts(): Observable<any> {
  return this.http.get('https://jsonplaceholder.typicode.com/posts');
}
```

Este método devuelve un `Observable` que contiene los datos de la API externa. En este ejemplo, estamos utilizando `https://jsonplaceholder.typicode.com/posts`, pero puedes reemplazar esta URL por la de tu propia API.

### **Paso 3: Realizar una Solicitud `POST` a una API Externa**

Si necesitas enviar datos a la API, puedes usar el método `post` de `HttpClient`:

```typescript
createPost(data: any): Observable<any> {
  return this.http.post('https://jsonplaceholder.typicode.com/posts', data);
}
```

---

## **3. Otros Métodos HTTP**

Además de los métodos `GET` y `POST`, **`HttpClient`** ofrece otros métodos útiles para manejar solicitudes HTTP:

| Método  | Descripción                              | Ejemplo                                        |
|---------|------------------------------------------|------------------------------------------------|
| `put()` | Para actualizar datos completos.        | `this.http.put(url, data)`                    |
| `delete()` | Para eliminar datos.                  | `this.http.delete(url)`                       |
| `patch()` | Para actualizaciones parciales.        | `this.http.patch(url, data)`                  |

---

## **4. Manejar Errores en las Solicitudes HTTP**

Es importante manejar los errores que puedan ocurrir durante las solicitudes HTTP. Puedes hacerlo utilizando el operador `catchError` de **RxJS**.

Ejemplo:

```typescript
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

getPosts(): Observable<any> {
  return this.http.get(`${this.apiUrl}/posts`).pipe(
    catchError(error => {
      console.error('Error en la solicitud', error);
      return throwError(error);
    })
  );
}
```

---

## 🎨 12. **Uso de Clases y Estilos Dinámicos en Ionic**

Con Angular (e Ionic), es fácil manipular las clases CSS y los estilos de los elementos de manera dinámica en función de las variables y condiciones que definas en tu componente.

---

### 📌 **1. Uso de `ngClass` (Asignar Clases Condicionalmente)**  

### 🔹 **a) Añadir una Clase Condicional en el HTML**  

Puedes usar **`[ngClass]`** para añadir clases a un elemento basado en una condición.  
Por ejemplo, si `qtyCart` es igual a 0, se añade la clase `empty-cart`.

```html
<div [ngClass]="{'empty-cart': qtyCart == 0}">
  Carrito de compras
</div>
```

- **Explicación:**  
  La clase **`empty-cart`** solo se aplicará si `qtyCart == 0`. Si la condición no es verdadera, la clase no se añadirá.

---

### 📌 **2. Uso de `ngStyle` (Aplicar Estilos Condicionalmente)**  

### 🔹 **a) Añadir Estilos Condicionales en el HTML**  

De manera similar, puedes usar **`[ngStyle]`** para aplicar estilos dinámicamente a los elementos.

```html
<div [ngStyle]="{ background: color, 'border-radius': '10px' }">
  Caja con estilo dinámico
</div>
```

- **Explicación:**  
  El estilo **`background`** se aplica desde la variable `color`, mientras que el borde tendrá un radio de **10px** independientemente del valor de `color`.

---


## 🛠️ 13. **Uso de Condicionales y Bucles en Angular**

En Ionic (y Angular), las estructuras de control como los condicionales y bucles son fundamentales para manejar dinámicamente el contenido en la interfaz. Aquí te muestro cómo usarlas correctamente.

---

### 📌 **1. Uso de `*ngIf` (Condicionales)**

### 🔹 **a) Condicional con `*ngIf`**

`*ngIf` se usa para renderizar un bloque de HTML solo si se cumple una condición.

```html
<div *ngIf="article; else articleNotFound">
  <div class="">
    <h2>{{ article.title }}</h2>
  </div>
</div>
<ng-template #articleNotFound>
  <div>
    NO HAY ARTÍCULOS
  </div>
</ng-template>
```

- **Explicación:**
  - **`*ngIf="article"`**: Si `article` existe (no es `null` ni `undefined`), se renderiza el contenido dentro del `div`.
  - **`else articleNotFound`**: Si la condición no se cumple, se renderiza el contenido del `ng-template` llamado `articleNotFound`.

---

### 🔹 **b) Condicional con `@if` (Nuevo)**

Este formato de condicional es más fácil de usar en plantillas con la nueva sintaxis de Angular, ideal para la validación más sencilla.

```html
@if(article) {
  <div class="">
    <h2>{{ article.title }}</h2>
  </div>
}
```

---

### 📌 **2. Uso de `*ngFor` (Bucles)**

### 🔹 **a) Bucle con `*ngFor`**

Usamos `*ngFor` para iterar sobre una lista de elementos. Es perfecto para renderizar colecciones de componentes o elementos dinámicamente.

```html
<div *ngFor="let article of articles; index as i">
  <app-article [article]="article"></app-article>
</div>
```

- **Explicación:**
  - **`let article of articles`**: Esto iterará sobre la lista `articles`, y por cada elemento se asignará a `article`.
  - **`index as i`**: Se puede usar la variable `i` para acceder al índice de la iteración.

---

### 🔹 **b) Bucle con `@for` (Nuevo)**

Al igual que el condicional `@if`, la nueva sintaxis de Angular también soporta el uso de bucles más simplificados en las plantillas.

```html
@for (article of articles; track $index) {
  <app-article [article]="article"></app-article>
}
@empty {
  <p>No hay artículos</p>
}
```


# 🚀 14. **Rutas con Parámetros en Angular (e Ionic)**

El manejo de rutas con parámetros es un aspecto clave para navegar y mostrar contenido dinámico en tu aplicación. A continuación, te muestro cómo generar y redirigir a rutas con parámetros de forma efectiva.

---

## 📌 **1. Generar Componente Página**

Primero, generamos el componente que será la página a la cual queremos navegar. Usaremos el comando de Ionic para crear una página.

```bash
ionic generate page article
```

---

## 📌 **2. Definir la Ruta en el Routing del Módulo**

En el archivo `app-routing.module.ts` o el archivo de rutas correspondiente, debes definir la ruta con el parámetro que recibirás. En este caso, estamos pasando un `id` como parámetro.

```typescript
const routes: Routes = [
  {
    path: 'article/:id', 
    component: ArticlePageComponent
  }
];
```

- **Explicación:**  
  La sintaxis `:id` indica que esta ruta espera un parámetro llamado `id`.

---

## 📌 **3. Redirigir a la Ruta con Parámetros**

### 🔹 **a) Usando `routerLink`**

Una forma sencilla de redirigir a una ruta con parámetros es usar el atributo `routerLink` en el HTML:

```html
<button [routerLink]="['/home/article/' + article.id]">Ver Artículo</button>
```

- **Explicación:**  
  Este código redirige a la ruta `article/:id`, donde `article.id` es el valor dinámico que pasa como parámetro en la URL.

---

### 🔹 **b) Usando el `Router` en el Componente**

También puedes redirigir programáticamente utilizando el servicio `Router` de Angular.

1. **Definir `Router` en el Constructor del Componente**

```typescript
import { Router } from '@angular/router';

constructor(private router: Router) {}
```

2. **Redirigir en una Función**

Luego, puedes redirigir a la ruta con un `id` específico dentro de una función que se ejecuta, por ejemplo, al hacer clic en un botón:

```html
<button (click)="goToArticle()">Ver Artículo</button>
```

```typescript
goToArticle() {
  this.router.navigate(['/home/article', this.article.id]);
}
```

- **Explicación:**  
  La función `goToArticle` navega a la ruta `/home/article` pasando `article.id` como parámetro.

---

## 📌 **4. Obtener el Parámetro en la Página (Componente)**

### 🔹 **a) Definir `ActivatedRoute` en el Constructor**

Para obtener los parámetros de la ruta, necesitamos inyectar `ActivatedRoute` en el constructor de nuestro componente.

```typescript
import { ActivatedRoute } from '@angular/router';
import { inject } from '@angular/core';

private route = inject(ActivatedRoute);
```

### 🔹 **b) Obtener los Parámetros de la Ruta**

Dentro del constructor o en el método `ngOnInit()`, suscribimos a los parámetros de la ruta para obtener los valores dinámicos.

```typescript
this.route.params.subscribe(params => {
  console.log(params);  // Muestra todos los parámetros
  if (params.id) {
    // Aquí puedes usar el parámetro 'id' para obtener datos
    this.articleId = params.id;
  }
});
```


# 🚀 15. **Uso de Rutas con Query Params en Angular (e Ionic)**

Los **query params** son útiles cuando necesitamos enviar información adicional a través de la URL sin que forme parte de la ruta principal. A continuación, te guío paso a paso sobre cómo utilizar los query params en Angular.

---

## 📌 **1. Enviar Query Params en una Ruta**

Para enviar datos a través de query params, puedes hacerlo utilizando el atributo `routerLink` en el HTML.

### 🔹 **a) Usando `routerLink` con Query Params**

```html
<button [routerLink]="['/home/article']"
        [queryParams]="{ title: 'dsadas', other: 'other' }">
  Ver
</button>
```

- **Explicación:**
  - `[queryParams]`: Se utiliza para pasar un objeto con los parámetros que queremos incluir en la URL.
  - En este ejemplo, se pasan dos parámetros: `title` y `other`.

---

## 📌 **2. Redirigir con Query Params en el Componente**

Si prefieres redirigir programáticamente usando el servicio `Router`, puedes hacerlo de la siguiente forma:

### 🔹 **a) Usando el `Router` para Redirigir con Query Params**

1. **Definir `Router` en el Constructor del Componente:**

```typescript
import { Router } from '@angular/router';

constructor(private router: Router) {}
```

2. **Función para Redirigir con Query Params:**

```typescript
goToArticle() {
  this.router.navigate(['/home/article', this.article.id], { queryParams: { title: this.article.title } });
}
```

- **Explicación:**
  - **`this.router.navigate`**: Utiliza el servicio `Router` para navegar a una ruta específica.
  - **`{ queryParams: { title: this.article.title } }`**: Los parámetros de consulta (`queryParams`) se pasan como un objeto. En este caso, el parámetro `title` es el título del artículo.

---

## 📌 **3. Obtener Query Params en la Página (Componente)**

Una vez que has redirigido con query params, puedes acceder a esos parámetros en el componente de destino usando el servicio `ActivatedRoute`.

### 🔹 **a) Obtener Query Params en el Componente**

1. **Definir `ActivatedRoute` en el Constructor del Componente:**

```typescript
import { ActivatedRoute } from '@angular/router';
import { inject } from '@angular/core';

private route = inject(ActivatedRoute);
```

2. **Suscribirse a los `queryParams` para Obtener los Valores:**

```typescript
this.route.queryParams.subscribe(queryParams => {
  console.log(queryParams);  // Muestra todos los query params
  if (queryParams.title) {
    // Utiliza el parámetro 'title' para realizar alguna acción
    this.articleTitle = queryParams.title;
  }
});
```

- **Explicación:**
  - **`this.route.queryParams`**: Se utiliza para acceder a los parámetros de consulta de la URL.
  - **`queryParams.title`**: Accede al valor del parámetro `title` desde la URL.

---

### 📝 **Ejemplo Completo:**

1. **Redirigir con un Botón en HTML**:

```html
<button [routerLink]="['/home/article']" [queryParams]="{ title: 'Angular Basics', other: 'extraInfo' }">Ver Artículo</button>
```

2. **En el Componente Destino, Obtener los Query Params**:

```typescript
import { ActivatedRoute } from '@angular/router';
import { inject } from '@angular/core';

export class ArticleComponent {
  private route = inject(ActivatedRoute);

  articleTitle: string;
  
  ngOnInit() {
    this.route.queryParams.subscribe(queryParams => {
      console.log(queryParams);  // Imprime { title: 'Angular Basics', other: 'extraInfo' }
      this.articleTitle = queryParams.title;
    });
  }
}
```


# 🚀 16. **Uso de `NgModel` en Angular (Similar a `wire:model` de Livewire)**

El uso de `NgModel` en Angular te permite enlazar los valores de los formularios de manera bidireccional, es decir, cuando el valor cambia en el formulario, automáticamente se actualiza en el componente y viceversa.

---

## 📌 **1. Importar `FormsModule` en el Módulo**

Para usar `NgModel`, primero necesitas importar el módulo `FormsModule` en el archivo del módulo donde se va a utilizar.

### 🔹 **a) Importar `FormsModule` en el Módulo**

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [/* tus componentes */],
  imports: [
    FormsModule
    // otros módulos
  ]
})
export class YourModule { }
```

- **Explicación:**
  - Se importa el `FormsModule` desde `@angular/forms` para habilitar el uso de formularios y `NgModel`.

---

## 📌 **2. Definir la Interfaz o la Variable en el Componente**

Es recomendable utilizar una interfaz para definir la estructura de los datos que manejará el formulario.

### 🔹 **a) Definir la Interfaz o Variable en el Componente**

```typescript
import { Component } from '@angular/core';

export namespace Models {
  export interface Contact {
    FormContactI: {
      email: string;
      name: string;
      phone: string;
    };
  }
}

@Component({
  selector: 'app-contact-form',
  templateUrl: './contact-form.component.html',
  styleUrls: ['./contact-form.component.css']
})
export class ContactFormComponent {
  form: Models.Contact.FormContactI = {
    email: '',
    name: '',
    phone: '',
  };
  
  error: string;
  
  sendForm() {
    if (!this.form.name) {
      this.error = 'Name is required';
      return;
    }
    console.log('Form submitted');
    console.log("Form: ", this.form);
  }
}
```

- **Explicación:**
  - La interfaz `FormContactI` define las propiedades del formulario.
  - La variable `form` está inicializada con valores vacíos y se usará para enlazar los inputs del formulario.
  - La función `sendForm()` valida que el campo `name` esté lleno antes de enviar el formulario.

---

## 📌 **3. Enlazar los Campos del Formulario con `NgModel`**

Usamos `[(ngModel)]` para enlazar los valores de los campos del formulario con las propiedades de la interfaz o variables del componente.

### 🔹 **a) Usar `NgModel` en los Inputs del Formulario**

```html
<form (ngSubmit)="sendForm()">
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" [(ngModel)]="form.name" name="name">
  </div>
  <div>
    <label for="email">Email:</label>
    <input type="email" id="email" [(ngModel)]="form.email" name="email">
  </div>
  <div>
    <label for="phone">Phone:</label>
    <input type="text" id="phone" [(ngModel)]="form.phone" name="phone">
  </div>
  <div *ngIf="error" style="color: red;">
    {{ error }}
  </div>
  <button type="submit">Submit</button>
</form>
```

- **Explicación:**
  - `[(ngModel)]="form.name"` establece una vinculación bidireccional entre el input y la propiedad `name` de la variable `form`. Cualquier cambio en el input actualizará automáticamente la propiedad `form.name` y viceversa.
  - El `name="name"` es necesario para que el formulario funcione correctamente en Angular y se pueda identificar cada campo.

---

## 📌 **4. Validación del Formulario al Enviar**

En la función `sendForm()`, puedes validar los valores de los campos antes de enviar los datos.

### 🔹 **a) Validación y Envío del Formulario**

```typescript
sendForm() {
  if (!this.form.name) {
    this.error = 'Name is required';
    return;
  }
  console.log('Form submitted');
  console.log("Form: ", this.form);
}
```


# 🚀 17. **Uso de Formularios Reactivos en Angular**

Los formularios reactivos en Angular permiten un mayor control sobre el estado y la validación del formulario mediante programación. A continuación, te explico cómo configurarlo paso a paso.

---

## 📌 **1. Importar `ReactiveFormsModule` en el Módulo**

Primero, necesitas importar el módulo `ReactiveFormsModule` en tu archivo de módulo (`app.module.ts` o el módulo correspondiente).

### 🔹 **Importar `ReactiveFormsModule`**

```typescript
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    ReactiveFormsModule
    // otros módulos
  ]
})
export class YourModule { }
```

- **Explicación:**  
  Se importa `ReactiveFormsModule` para habilitar los formularios reactivos en tu aplicación.

---

## 📌 **2. Inyectar `FormBuilder` en el Componente**

Angular proporciona el `FormBuilder`, que facilita la creación de formularios reactivos. Debes inyectarlo en el constructor del componente.

### 🔹 **Inyectar `FormBuilder`**

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, Validators, FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-contact-form',
  templateUrl: './contact-form.component.html',
  styleUrls: ['./contact-form.component.css']
})
export class ContactFormComponent implements OnInit {

  formData: FormGroup;

  constructor(private fb: FormBuilder) { }

  ngOnInit(): void {
    // Inicialización del formulario
    this.formData = this.fb.group({
      email: ['', Validators.required],
      name: ['', Validators.required],
      phone: ['']
    });
  }

  sendFormReactive() {
    if (this.formData.valid) {
      console.log('Form submitted:', this.formData.value);
    }
  }
}
```

- **Explicación:**  
  - Se inyecta `FormBuilder` en el constructor y se usa para crear el formulario reactivo `formData`.
  - Se define la estructura del formulario con `FormGroup` y se aplican validadores como `Validators.required`.

---

## 📌 **3. Crear el Formulario en la Vista**

En la vista, utilizamos `formGroup` y `formControlName` para vincular el formulario y los campos del componente.

### 🔹 **Formulario con evento `submit`**

```html
<form [formGroup]="formData" (ngSubmit)="sendFormReactive()">
  <div>
    <label for="email">Email:</label>
    <input type="email" formControlName="email" placeholder="Email">
    <span *ngIf="formData.controls['email'].hasError('required')">Este campo es requerido</span>
  </div>

  <div>
    <label for="name">Name:</label>
    <input type="text" formControlName="name" placeholder="Name">
  </div>

  <div>
    <label for="phone">Phone:</label>
    <input type="text" formControlName="phone" placeholder="Phone">
  </div>

  <button type="submit" [disabled]="!formData.valid">
    Enviar
  </button>
</form>
```

- **Explicación:**  
  - `formGroup` vincula el formulario de la vista con la variable `formData`.
  - `formControlName` vincula cada campo con su respectiva propiedad en el formulario.
  - Se utiliza la validación y se desactiva el botón si el formulario no es válido.

---

## 📌 **4. Mostrar Errores de Validación**

Puedes mostrar errores de validación debajo de cada campo utilizando `*ngIf` en la plantilla.

### 🔹 **Mostrar Errores de Validación**

```html
<span *ngIf="formData.controls['email'].hasError('required')">
  <i>Este campo es <strong>requerido</strong></i>
</span>
```

- **Explicación:**  
  - Utilizas `hasError('required')` para verificar si el campo tiene un error de tipo `required` y mostrar un mensaje personalizado.

---

## 📌 **5. Validadores Personalizados**

Si necesitas una validación más avanzada, puedes crear un validador personalizado.

### 🔹 **Definir un Validador Personalizado**

```typescript
customValidator(input: FormControl) {
  console.log('input: ', input.value);
  if (input.value.length !== 10) {
    return { customLength: true };
  }
  return null;
}
```

- **Explicación:**  
  - El validador personalizado verifica si el valor del campo tiene exactamente 10 caracteres. Si no, devuelve un objeto con un error (`customLength: true`).

### 🔹 **Añadir el Validador al Campo**

```typescript
this.formData = this.fb.group({
  email: ['', Validators.required],
  name: ['', Validators.required],
  phone: ['', [Validators.required, this.customValidator]]
});
```

### 🔹 **Mostrar el Error de Validación Personalizado**

```html
<span *ngIf="formData.controls['phone'].hasError('customLength')">
  <i>El teléfono debe tener <strong>10 caracteres</strong></i>
</span>
```

- **Explicación:**  
  - Aquí estamos aplicando el validador personalizado a un campo (`phone`).
  - Si el campo no tiene la longitud correcta, se muestra un mensaje de error personalizado.

---

## 📌 **6. Escuchar Cambios en un Campo de Entrada**

Puedes suscribirte a los cambios de un campo específico para realizar alguna acción cuando el valor cambie.

### 🔹 **Suscribirse a Cambios de un Campo**

```typescript
ngOnInit() { 
  this.formData.controls['email'].valueChanges.subscribe((value) => {
    console.log('Email value: ', value);
  });
}
```

- **Explicación:**  
  - Con `valueChanges`, puedes escuchar cualquier cambio en el valor de un campo (`email` en este caso).

---

## 📌 **7. Asignar Valor a un Campo**

Puedes asignar un valor a un campo programáticamente.

### 🔹 **Asignar Valor a un Campo**

```typescript
this.formData.controls['phone'].setValue('1234567890');
```

- **Explicación:**  
  - Utilizas `setValue` para cambiar el valor de un campo en el formulario de forma dinámica.

---

## ✅ **Resumen de los Pasos para Usar Formularios Reactivos**

| Acción | Código | Descripción |
|--------|--------|-------------|
| **Importar `ReactiveFormsModule`** | `import { ReactiveFormsModule } from '@angular/forms';` | Se importa en el módulo para habilitar formularios reactivos. |
| **Inyectar `FormBuilder`** | `constructor(private fb: FormBuilder)` | Se inyecta el `FormBuilder` para crear formularios reactivos. |
| **Definir el Formulario** | `formData = this.fb.group({ ... })` | Se define un formulario con reglas de validación usando `FormBuilder`. |
| **Formulario en la Vista** | `<form [formGroup]="formData" (ngSubmit)="sendFormReactive()">` | Se vincula el formulario con `formGroup` y se maneja el evento `ngSubmit`. |
| **Mostrar Errores de Validación** | `<span *ngIf="formData.controls['email'].hasError('required')">` | Mostrar errores cuando un campo no pase la validación. |
| **Crear Validador Personalizado** | `customValidator(input: FormControl)` | Crear un validador para reglas personalizadas en los campos. |
| **Escuchar Cambios de un Campo** | `this.formData.controls['email'].valueChanges.subscribe(...)` | Escuchar cambios en los valores de un campo. |
| **Asignar Valor a un Campo** | `this.formData.controls['phone'].setValue('1234567890')` | Asignar un valor dinámico a un campo del formulario. |

---



<<<<<<< HEAD:src/pages/infoFiles/Ionic.md
# 🚀 18. **Pipes en Angular**

Los **Pipes** en Angular son transformadores de valores que puedes utilizar directamente en las plantillas para mostrar datos de una manera más amigable o formateada. Los pipes se usan para transformar datos en la vista, sin modificar la lógica del componente.

---

## 📌 **Uso Básico de Pipes**

Angular incluye varios pipes integrados para formatear datos de manera común. Aquí te muestro algunos ejemplos:

### 🔹 **Ejemplos de Pipes Integrados**

1. **Date Pipe**: Para formatear fechas.

   ```html
   <p>{{ article.time | date: 'yyyy' }}</p>
   ```

2. **Uppercase Pipe**: Convierte el texto a mayúsculas.

   ```html
   <h2>{{ article.title | uppercase }}</h2>
   ```

3. **Currency Pipe**: Para formatear valores monetarios.

   ```html
   <p>{{ item.price | currency }}</p>
   ```

4. **Json Pipe**: Para mostrar el objeto como una cadena JSON.

   ```html
   {{ article | json }}
   ```

5. **Combinar Pipes**: Puedes combinar varios pipes para transformar un valor en múltiples pasos.

   ```html
   <p>{{ article.body | uppercase | currency }}</p>
   ```

---

## 📌 **Creación de un Pipe Personalizado**

Si necesitas un pipe específico para tu aplicación, puedes crear uno. A continuación te muestro cómo crear un pipe personalizado en Angular.

### 🔹 **Pasos para Crear un Pipe Personalizado**

1. **Crear el Pipe**: Usa el CLI de Angular para generar un nuevo pipe.

   ```bash
   ng generate pipe resume
   ```

   Esto creará un archivo `resume.pipe.ts` y lo registrará en el módulo.

2. **Definir `standalone: true`**: Esto permite que el pipe sea usado sin necesidad de importar explícitamente en otros módulos.

3. **Implementar el `transform()`**: El método `transform()` define cómo se transformarán los datos.

### 🔹 **Ejemplo de Pipe Personalizado (Resume Pipe)**

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'resume',
  standalone: true  // Este pipe será autónomo
})
export class ResumePipe implements PipeTransform {

  transform(value: string, ...args: unknown[]): string {
    let maxLength = args[0] as number;  // Primer argumento es la longitud máxima
    if (value.length > maxLength) {
      return value.slice(0, maxLength) + '...';  // Corta y agrega '...'
    }
    return value;
  }
}
```

### 🔹 **Explicación del Pipe Personalizado**:
- **`transform(value: string, ...args: unknown[]): unknown`**: Esta función recibe el valor que será transformado (en este caso, una cadena) y un número variable de argumentos (por ejemplo, `maxLength`).
- Si el valor supera la longitud máxima proporcionada como argumento, corta el texto y le añade "…".
- Si el valor no supera la longitud máxima, simplemente retorna el valor original.

---

## 📌 **Registrar y Usar el Pipe**

1. **Importar en el Módulo**: Para poder usar el pipe en tu aplicación, asegúrate de importarlo en el módulo donde lo vayas a utilizar.

```typescript
import { ResumePipe } from './pipes/resume.pipe';

@NgModule({
  declarations: [
    ResumePipe
    // otros componentes, pipes, etc.
  ]
})
export class YourModule { }
```

2. **Usar el Pipe en la Vista**: Ahora puedes usar el pipe en cualquier plantilla del módulo, pasando el parámetro necesario.

```html
<p>{{ article.body | resume: 30 }}</p>
```

- **Explicación**:
  - El primer parámetro (`article.body`) es el valor que será transformado.
  - El segundo parámetro (`30`) es el argumento que le indicará al pipe cuántos caracteres debe mostrar del texto original.

---


# 🚀 19. **Directivas en Angular**

Las **directivas** en Angular son clases que permiten manipular el DOM o el comportamiento de los elementos en la vista. Puedes usar directivas para cambiar el estilo, manejar eventos o agregar funcionalidades a los elementos HTML.

---

## 📌 **Pasos para Crear y Usar una Directiva Personalizada**

### 1. **Crear una Directiva Standalone**

En primer lugar, debes crear una nueva directiva usando el **CLI de Angular**. Una directiva puede ser standalone, lo que significa que no necesita estar vinculada a un componente en particular.

#### Ejemplo: Crear una directiva para resaltar un elemento al pasar el mouse.

```bash
ng generate directive highlight
```

Esto generará el archivo `highlight.directive.ts` en el directorio de directivas.

### 2. **Importar la Directiva en el Módulo**

Para usar la directiva en tu aplicación, debes importarla en el módulo que la utilizará.

```typescript
import { HighlightDirective } from './directives/highlight.directive';

@NgModule({
  declarations: [
    // otros componentes y directivas
    HighlightDirective
  ]
})
export class AppModule { }
```

### 3. **Definir el Comportamiento de la Directiva**

Usa la inyección de dependencias en el constructor de la directiva para manipular el DOM. Utiliza el `ElementRef` para acceder al elemento HTML al que se aplica la directiva.

#### Ejemplo:

```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'  // El selector que usaremos en el HTML
})
export class HighlightDirective {

  @Input() appHighlight = ''; // El color que se pasará al HTML
  @Input() nameCustom = 'default'; // Otro parámetro personalizado

  constructor(private el: ElementRef) {
    this.el.nativeElement.style.backgroundColor = 'yellow';  // Establece un color por defecto
  }

  @HostListener('mouseleave') 
  onMouseLeave() {
    this.highlight('');  // Eliminar el color de fondo cuando el mouse sale
  }

  // Método para cambiar el color de fondo
  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }

  @HostListener('click') 
  registerEvent() {
    console.log('click event');
    console.log('name -> ', this.nameCustom);
  }
}
```

#### Explicación:
- **`ElementRef`**: Permite acceder al elemento del DOM en el que se aplica la directiva.
- **`@HostListener`**: Escucha los eventos del DOM y ejecuta métodos cuando estos ocurren. En este caso, cuando el mouse sale (`mouseleave`) o se hace clic en el elemento (`click`).
- **`@Input()`**: Recibe valores desde el componente para personalizar el comportamiento de la directiva (como el color de fondo).

### 4. **Usar la Directiva en el HTML**

En el archivo HTML, puedes usar la directiva como un atributo de cualquier elemento HTML. También puedes pasarle valores a través de los inputs definidos.

#### Ejemplo:

```html
<h2 appHighlight="red" nameCustom="pepito">Este es un título</h2>
```

- **`appHighlight="red"`**: Cambia el color de fondo al rojo cuando el mouse pasa sobre el elemento.
- **`nameCustom="pepito"`**: Este valor será accesible en la directiva y se puede usar en los eventos, como el click.

### 5. **Personalizar Comportamiento con Eventos**

También puedes agregar eventos personalizados a la directiva. Por ejemplo, cuando el usuario hace clic en el elemento, puedes realizar acciones específicas.

#### Ejemplo:

```typescript
@HostListener('click') 
registerEvent() {
  console.log('click event');
  console.log('name -> ', this.nameCustom);
}
```


## 📌 20. **Pasos para Implementar un Guard - `CanActivate`**

### 1. **Crear el Guard**

Puedes crear un guard utilizando el CLI de Angular. Por ejemplo, para crear un `CanActivate` guard:

```bash
ng generate guard guards/is-admin
```

Esto generará un archivo llamado `is-admin.guard.ts`.

### 2. **Importar el Guard en el Módulo de Rutas**

Una vez creado el guard, debes incluirlo en el **routing** de tu aplicación para que Angular pueda utilizarlo al acceder a las rutas protegidas.

#### Ejemplo:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { NotificationsComponent } from './notifications/notifications.component';
import { isAdminGuard } from './guards/is-admin.guard';

const routes: Routes = [
  { path: '', component: NotificationsComponent, canActivate: [isAdminGuard] },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

- Aquí estamos utilizando el `isAdminGuard` para proteger la ruta que lleva al componente `NotificationsComponent`. Si el guard no devuelve `true`, el acceso será bloqueado.

### 3. **Definir el Guard con Funciones Personalizadas**

El guard puede contener una función `canActivate` que contiene la lógica para permitir o denegar el acceso a una ruta. Este método debe retornar un **booleano** o un **Promise<boolean>** para indicar si se permite el acceso.

#### Ejemplo de un Guard con una Función Personalizada:

```typescript
import { Injectable } from '@angular/core';
import { ActivatedRouteSnapshot, RouterStateSnapshot, CanActivate } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class isAdminGuard implements CanActivate {

  async canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Promise<boolean> {
    const is = await this.isAdmin();
    return is; // Si es admin, devuelve true, si no, false.
  }

  // Función personalizada para verificar si el usuario es administrador.
  isAdmin() {
    return new Promise<boolean>((resolve, reject) => {
      setTimeout(() => {
        resolve(true); // Simula una verificación asincrónica (por ejemplo, una llamada a una API)
      }, 1000);
    });
  }
}
```

- **`canActivate`**: Es un método que se ejecuta antes de que la ruta sea activada. Si retorna `true`, la navegación continúa; si retorna `false`, la navegación es bloqueada.
- **`isAdmin()`**: Simula una llamada a un servicio o verificación asincrónica (como si el usuario fuera o no administrador).

### 4. **Forma Nueva - `CanActivateFn` (Angular 14+)**

En Angular 14 y versiones posteriores, se introdujo la posibilidad de escribir guards como funciones **en lugar de clases**. Esto simplifica la sintaxis, y te permite escribir un guard de manera más declarativa.

#### Ejemplo usando `CanActivateFn`:

```typescript
import { inject } from '@angular/core';
import { CanActivateFn } from '@angular/router';
import { WebService } from './services/web.service';

export const isAdminGuard: CanActivateFn = async (
  next: ActivatedRouteSnapshot,
  state: RouterStateSnapshot
) => {

  const webService = inject(WebService);
  
  // Aquí puedes llamar a un servicio que realice la validación de admin
  const isAdmin = await webService.checkIfAdmin();
  return isAdmin; // Si retorna true, permitirá la navegación; si es false, no.
};
```

- **`inject()`**: Angular usa la función `inject()` para inyectar servicios directamente dentro de la función.
- **`CanActivateFn`**: Es el tipo utilizado en la nueva forma de escribir guards como funciones.

---

## 📌 **Pasos para Ejecutar Capacitor en Android**

### **1. Ejecutar el comando de construcción en Ionic**

Primero, asegúrate de haber completado tu aplicación en Ionic antes de agregarle soporte para Capacitor:

```bash
ionic build --prod
```

Este comando construye tu aplicación en modo de producción, generando los archivos de la aplicación web que luego se usarán en la aplicación nativa.

---

### **2. Instalar Capacitor para Android**

Asegúrate de tener instaladas las dependencias de Capacitor para Android:

```bash
npm install @capacitor/android
```

Este comando instalará los paquetes necesarios para poder construir y ejecutar tu aplicación en Android.

---

### **3. Modificar `capacitor.config.ts`**

Luego, debes configurar las propiedades de tu aplicación en el archivo `capacitor.config.ts`. Aquí defines el **ID de la aplicación** y el **nombre de la aplicación**:

```typescript
const config: CapacitorConfig = {
  appId: 'capacitor.demo.app',   // El ID de la aplicación (puede ser cualquier identificador único)
  appName: 'Capacitor-demo',      // El nombre que tendrá la aplicación
  webDir: 'www',                  // La carpeta que contiene la aplicación web generada
};
```

Este archivo es esencial para la configuración básica de Capacitor.

---

### **4. Agregar Android a tu Proyecto**

Una vez configurado Capacitor, puedes agregar Android a tu proyecto ejecutando:

```bash
npx cap add android
```

Este comando agrega la plataforma Android a tu proyecto, creando una carpeta `android` en tu directorio del proyecto con todos los archivos y configuraciones necesarias para ejecutar la aplicación en Android.

---

### **5. Abrir el Proyecto Android en Android Studio**

Para abrir tu proyecto Android en Android Studio y comenzar a probarlo o realizar modificaciones, utiliza el siguiente comando:

```bash
npx cap open android
```

Esto abrirá Android Studio con el proyecto Android.

---

### **6. Sincronizar los Cambios con la Plataforma Android**

Siempre que hagas cambios en tu código web o en el proyecto Ionic, necesitas sincronizar esos cambios con la plataforma Android para que se reflejen correctamente. Utiliza este comando para hacerlo:

```bash
npx cap sync android
```

Este comando sincroniza los archivos generados por Ionic con la plataforma Android, asegurando que todo esté actualizado.

---