---
    title: 'Contenido Pagina'
    layout: '../../layouts/Layout.astro'
---

# Clonación de GitHub

Para configurar el proyecto, sigue estos pasos:

1. **Instalar dependencias**  
   Ejecuta el siguiente comando para instalar las dependencias del proyecto:

    ```bash
    composer install
    ```

2. **Actualizar dependencias**  
   Si necesitas actualizar las dependencias, usa:

    ```bash
    composer update
    ```

3. **Configurar el archivo de entorno**  
   Copia el archivo de ejemplo a tu archivo de configuración:

    ```bash
    cp .env.example .env
    ```

4. **Generar la clave de la aplicación**  
   Genera una nueva clave para tu aplicación:

    ```bash
    php artisan key:generate
    ```

5. **Configurar la base de datos**  
   Abre el archivo `.env` y ajusta las siguientes configuraciones de la base de datos:

    ```plaintext
    DB_CONNECTION=mysql
    DB_HOST=nombre_del_host
    DB_PORT=puerto_de_la_base_de_datos
    DB_DATABASE=nombre_de_la_base_de_datos
    DB_USERNAME=nombre_de_usuario_de_la_base_de_datos
    DB_PASSWORD=contraseña_de_la_base_de_datos
    ```

6. **Ejecutar migraciones**  
   Aplica las migraciones para crear las tablas en la base de datos:

    ```bash
    php artisan migrate
    ```

¡Y eso es todo! Tu proyecto debería estar listo para usarse.
