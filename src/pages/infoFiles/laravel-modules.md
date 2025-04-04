---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---

## **Crear un Proyecto Laravel**

1. **Crear el Proyecto**  
   Ejecuta el siguiente comando para crear un nuevo proyecto Laravel:

    ```bash
    composer create-project laravel/laravel your-project-name 9.*
    ```

---

## **Instalar Laravel-Modules**

2. **Instalar el Paquete**  
   Añade el paquete `laravel-modules` a tu proyecto:

    ```bash
    composer require nwidart/laravel-modules 9.*
    ```

3. **Publicar el Proveedor**  
   Publica los archivos del proveedor con este comando:

    ```bash
    php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"
    ```

4. **Configurar `composer.json`**  
   Añade la línea `"Modules"` en la sección `autoload` de tu archivo `composer.json`:

    ```json
    {
    	"autoload": {
    		"psr-4": {
    			"App\\": "app/",
    			"Modules\\": "Modules/"
    		}
    	}
    }
    ```

5. **Actualizar el Autoload**  
   Regenera el autoload con el siguiente comando:

    ```bash
    composer dump-autoload
    ```

---

## **Instalar Livewire-modules**

6. **Instalar Livewire**  
   Agrega Livewire a tu proyecto:

    ```bash
    composer require livewire/livewire
    ```

7. **Instalar el Paquete de Livewire para Módulos**  
   Instala el paquete que permite usar Livewire en los módulos:

    ```bash
    composer require mhmiton/laravel-modules-livewire
    ```

8. **Publicar la Configuración de Livewire**  
   Publica la configuración de Livewire para los módulos con:

    ```bash
    php artisan vendor:publish --tag=modules-livewire-config
    ```

---

## **Instalar Dependencias Adicionales (Opcional)**

9. **Instalar Laravel Collective**  
   Si deseas usar formularios en Laravel, ejecuta:

    ```bash
    composer require laravelcollective/html
    ```

---

## **Crear un Módulo**

10. **Crear un Nuevo Módulo**  
   Para crear un módulo nuevo, usa el siguiente comando:

    ```bash
    php artisan module:make module-name
    ```

11. **Crear un Módulo Livewire**  
   Para crear un módulo Livewire, ejecuta:

    ```bash
    php artisan module:make-livewire Pages/AboutPage
    ```

---

## **Información sobre Livewire**

### **Rutas**

Configura las rutas para tu módulo de la siguiente manera:

```php
Route::prefix('inventory')->group(function() {
    Route::view('/', 'inventory::livewire.administration.product.index');
});
```

### **Plantilla Básica de Livewire**

Crea el archivo `view/layouts/master.blade.php` con el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="es">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>@yield('title')</title>
		<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/5.1.1/css/bootstrap.min.css" />
		@livewireStyles
	</head>
	<body>
		<div class="container mt-4">@yield('content')</div>

		@livewireScripts
		<script src="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/5.1.1/js/bootstrap.min.js"></script>
	</body>
</html>
```

### **Añadir Componente Livewire**

Para añadir un componente Livewire, usa el siguiente código en tu vista:

```blade
@livewire('module::administration.product.product')
```

### **Plantilla del Módulo**

Crea la plantilla del módulo con el siguiente contenido:

```blade
@extends('layouts.master')

@section('title', 'Título')

@section('content')
    @livewire('module::administration.product.product')
@endsection
```

---

Con estos pasos, tu proyecto Laravel estará completamente configurado para usar módulos y Livewire. Puedes comenzar a crear y organizar tu aplicación usando estas herramientas poderosas.