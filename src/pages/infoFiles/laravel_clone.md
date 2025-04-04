---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---

# Clonación de GitHub

Para configurar el proyecto, sigue estos pasos:
Aquí tienes los pasos para configurar un proyecto clonado desde GitHub:

---

## 1. **Instalar Dependencias**

Primero, asegúrate de instalar todas las dependencias necesarias para el proyecto ejecutando el siguiente comando:

```bash
composer install
```

---

## 2. **Actualizar Dependencias**

Si ya has instalado las dependencias previamente pero necesitas actualizarlas, ejecuta:

```bash
composer update
```

---

## 3. **Configurar el Archivo de Entorno**

Luego, debes configurar el archivo de entorno del proyecto. Copia el archivo de ejemplo a `.env`:

```bash
cp .env.example .env
```

---

## 4. **Generar la Clave de la Aplicación**

Genera una nueva clave para tu aplicación con el siguiente comando:

```bash
php artisan key:generate
```

Esto establecerá una clave única para la aplicación en el archivo `.env`.

---

## 5. **Configurar la Base de Datos**

Abre el archivo `.env` y configura los detalles de la base de datos. Modifica las siguientes líneas para reflejar los datos correctos de tu base de datos:

```plaintext
DB_CONNECTION=mysql
DB_HOST=nombre_del_host
DB_PORT=puerto_de_la_base_de_datos
DB_DATABASE=nombre_de_la_base_de_datos
DB_USERNAME=nombre_de_usuario_de_la_base_de_datos
DB_PASSWORD=contraseña_de_la_base_de_datos
```

---

## 6. **Ejecutar Migraciones**

Para crear las tablas necesarias en tu base de datos, ejecuta las migraciones:

```bash
php artisan migrate
```
