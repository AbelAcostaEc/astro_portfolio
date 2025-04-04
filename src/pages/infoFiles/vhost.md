---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---

# Configuración de vHosts en XAMPP - Windows

Aquí tienes los pasos completos para configurar un Virtual Host en XAMPP en Windows:

---

### **1. Modificar el archivo `hosts`**

1. Abre el archivo `hosts` que se encuentra en:

    ```
    C:\Windows\System32\drivers\etc
    ```

2. Abre el archivo `hosts` con un editor de texto **como administrador** (por ejemplo, Notepad++ o el Bloc de Notas). Asegúrate de ejecutar el editor con privilegios de administrador.

3. Añade la siguiente línea al final del archivo:

    ```plaintext
    127.0.0.1 miproyecto.local.com
    ```

   Esto redirige la URL `miproyecto.local.com` a `127.0.0.1` (tu servidor local).

---

### **2. Configurar el Virtual Host en Apache**

1. Accede al archivo de configuración de Apache en la siguiente ruta:

    ```
    C:\xampp\apache\conf\extra
    ```

2. Abre el archivo `httpd-vhosts.conf` en un editor de texto.

3. Agrega la siguiente configuración al final del archivo:

    ```apache
    <VirtualHost *:80>
        DocumentRoot "C:\miproyecto\httpdocs"
        ServerName miproyecto.local.com
        <Directory "C:\miproyecto\httpdocs">
            Require all granted
        </Directory>
    </VirtualHost>
    ```

   Asegúrate de reemplazar `"C:\miproyecto\httpdocs"` con la ruta correcta a la carpeta donde se encuentra tu proyecto.

---

### **3. Reiniciar Apache**
