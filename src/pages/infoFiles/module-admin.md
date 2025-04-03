---
    title: 'Contenido Pagina'
    layout: '../../layouts/layout.astro'
---

# Módulo de Administración

## Importar Spatie Laravel

1. **Instalar el paquete de permisos**  
   Ejecuta el siguiente comando para añadir el paquete de permisos de Spatie:

    ```bash
    composer require spatie/laravel-permission
    ```

2. **Publicar el proveedor**  
   Publica los archivos de configuración del paquete:

    ```bash
    php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
    ```

3. **Ejecutar migraciones**  
   Aplica las migraciones y si es necesario, inicializa los datos de ejemplo:

    ```bash
    php artisan migrate:fresh --seed
    ```

4. **Modificar las migraciones**  
   Si necesitas habilitar el borrado suave, agrega la siguiente línea en la migración de permisos:

    ```php
    // En el archivo de migración correspondiente, línea 46
    $table->softDeletes();
    ```

## Crear el Módulo

5. **Generar el módulo**  
   Crea el módulo de administración con el siguiente comando:

    ```bash
    php artisan module:make Administration
    ```

6. **Crear enlace simbólico para almacenamiento**  
   Si es necesario, ejecuta el siguiente comando para crear un enlace simbólico a la carpeta de almacenamiento:

    ```bash
    php artisan storage:link
    ```
