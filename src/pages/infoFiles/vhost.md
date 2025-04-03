---
    title: 'Contenido Pagina'
    layout: '../../layouts/Layout.astro'
---

# Configuración de vHosts en XAMPP - Windows

Para configurar un virtual host en XAMPP, sigue estos pasos:

## 1. Modificar el archivo `hosts`

Accede al archivo `hosts` en la siguiente ubicación:

```
C:\Windows\System32\drivers\etc
```

Agrega la siguiente línea al final del archivo:

```plaintext
127.0.0.1 miproyecto.local.com
```

## 2. Configurar el Virtual Host en Apache

Abre el archivo de configuración de Apache en la siguiente ruta:

```
C:\xampp\apache\conf\extra
```

Edita el archivo `httpd-vhosts.conf` (o crea uno nuevo) y añade la siguiente configuración:

```apache
<VirtualHost *:80>
    DocumentRoot "C:\miproyecto\httpdocs"
    ServerName miproyecto.local.com
    <Directory "C:\miproyecto\httpdocs">
        Require all granted
    </Directory>
</VirtualHost>
```

## 3. Reiniciar Apache

Después de realizar estos cambios, asegúrate de reiniciar el servidor Apache desde el panel de control de XAMPP para que la configuración surta efecto.
