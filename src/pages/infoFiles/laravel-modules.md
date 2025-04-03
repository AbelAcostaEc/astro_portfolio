---
    title: 'Contenido Pagina'
    layout: '../../layouts/Layout.astro'
---

# Laravel-Modules

## Crear un proyecto Laravel

1. **Crear el proyecto**  
   Ejecuta el siguiente comando para crear un nuevo proyecto Laravel:

    ```bash
    composer create-project laravel/laravel your-project-name 9.*
    ```

## Instalar laravel-modules

2. **Instalar el paquete**  
   Añade el paquete de módulos:

    ```bash
    composer require nwidart/laravel-modules 9.*
    ```

3. **Publicar el proveedor**  
   Ejecuta el siguiente comando para publicar los archivos del proveedor:

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

5. **Actualizar el autoload**  
   Ejecuta el siguiente comando para regenerar el autoload:

    ```bash
    composer dump-autoload
    ```

## Instalar Livewire-modules

6. **Instalar Livewire**  
   Agrega el paquete de Livewire:

    ```bash
    composer require livewire/livewire
    ```

7. **Instalar el paquete de Livewire para módulos**  
   Añade el paquete específico para Livewire y módulos:

    ```bash
    composer require mhmiton/laravel-modules-livewire
    ```

8. **Publicar la configuración de Livewire**  
   Ejecuta el siguiente comando:

    ```bash
    php artisan vendor:publish --tag=modules-livewire-config
    ```

## Instalar dependencias adicionales (opcional)

9. **Instalar Laravel Collective**  
   Si deseas usar formularios, ejecuta:

    ```bash
    composer require laravelcollective/html
    ```

## Crear un módulo

10. **Crear un nuevo módulo**  
    Ejecuta el siguiente comando para crear un módulo:

    ```bash
    php artisan module:make module-name
    ```

11. **Crear un módulo Livewire**  
    Ejecuta el siguiente comando para crear un módulo Livewire:

    ```bash
    php artisan module:make-livewire Pages/AboutPage
    ```

## Información sobre Livewire

### Rutas

Configura las rutas para tu módulo:

```php
Route::prefix('inventory')->group(function() {
    Route::view('/', 'inventory::livewire.administration.product.index');
});
```

### Plantilla básica de Livewire

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

### Añadir componente Livewire

Para añadir un componente Livewire, utiliza el siguiente código:

```blade
@livewire('module::administration.product.product')
```

### Plantilla del módulo

Crea la plantilla del módulo con el siguiente contenido:

```blade
@extends('layouts.master')

@section('title', 'Título')

@section('content')
    @livewire('module::administration.product.product')
@endsection
```
