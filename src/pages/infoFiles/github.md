---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---



# 🚀 Git Cheat Sheet - Guía Visual y Práctica

> **Tu mapa interactivo para dominar Git sin dolor**  
> Desde conceptos básicos hasta flujos avanzados, con ejemplos claros y tips profesionales.

---

## ⚙️ Configuración Esencial (Primera Vez)

```bash
# Identidad
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Configuraciones útiles
git config --global core.editor "code --wait"  # Usa VSCode
git config --global init.defaultBranch main    # Rama principal por defecto
```

🔍 **Ver tu configuración:**  
```bash
git config --list
```

---

## 🔄 Flujo Básico de Trabajo

### 📍 1. Estado del Proyecto
```bash
git status  # Muestra archivos modificados/creados/eliminados
```

### ➕ 2. Añadir Cambios al Stage
```bash
git add archivo.txt      # Añade un archivo específico
git add .                # Añade TODOS los cambios
git add -p               # Modo interactivo (elige qué cambios añadir)
```
🎯 **¿Qué es el Stage?**  
Es el "área de preparación" donde seleccionas qué cambios irán en el próximo commit.

### 💾 3. Guardar Cambios (Commit)
```bash
git commit -m "Mensaje claro y descriptivo"
git commit -am "Commit rápido (añade y commitea archivos trackeados)"
```

### ☁️ 4. Sincronizar con Remoto
```bash
git push origin main     # Enviar tus commits al servidor
git pull origin main     # Traer cambios del servidor
```

---

## 🧳 Stash: Tu Salvavidas Temporal

**Cuando necesitas cambiar de rama pero no quieres commitear:**
```bash
git stash                     # Guarda cambios temporalmente
git stash -u                  # Incluye archivos no trackeados
git stash save "Nombre"       # Guarda con descripción
```

**Recuperar cambios:**
```bash
git stash pop                 # Recupera y ELIMINA el último stash
git stash apply               # Recupera pero MANTIENE el stash
```

**Gestionar múltiples stashes:**
```bash
git stash list                # Ver todos los stashes
git stash drop stash@{0}      # Eliminar stash específico
git stash clear               # Borrar TODOS los stashes
```

---

## 🌿 Manejo de Ramas (Branches)

### Crear y Cambiar entre Ramas
```bash
git branch                    # Lista todas las ramas
git checkout -b nueva-rama    # Crea y cambia a nueva rama
git checkout main             # Volver a la rama principal
```

### 🔀 Tres Formas de Integrar Cambios entre Ramas

#### 1. Merge (Fusión)
```bash
git checkout dev
git merge main
```
✅ **Ventaja:** Mantiene historial completo  
⚠️ **Resultado:** Crea un commit de merge

#### 2. Rebase (Reescritura)
```bash
git checkout dev
git rebase main
```
✅ **Ventaja:** Historial lineal y limpio  
⚠️ **Precaución:** No usar en ramas públicas

#### 3. Cherry-Pick (Selección)
```bash
git checkout main
git cherry-pick abc123       # Trae UN commit específico
```
💡 **Perfecto para:** Hotfixes o cambios puntuales

---

## 📦 Empaquetar tu Proyecto
```bash
git archive -o proyecto.zip HEAD   # Crea ZIP del estado actual
```
🔹 **Solo incluye** archivos commiteados (no cambios pendientes)

---

## 🕵️‍♂️ Inspeccionar Cambios

```bash
git diff                   # Ver cambios no añadidos
git diff --staged          # Ver cambios en stage
git log --oneline --graph  # Historial compacto con gráfico
git show abc123            # Detalles de un commit específico
```

---

## ⏪ Deshacer Cosas

```bash
git restore archivo.txt          # Descarta cambios no añadidos
git restore --staged archivo.txt # Saca del stage (pero mantiene cambios)
git commit --amend               # Corrige el último commit
```

---

## 🎯 Comandos Clave Resumidos

| Comando               | Acción                                  | Uso Típico                          |
|-----------------------|-----------------------------------------|-------------------------------------|
| `git status`          | Ver estado actual                      | Antes de cualquier operación        |
| `git add -p`          | Añadir cambios interactivamente        | Para commits limpios y organizados  |
| `git commit -am`      | Add + commit rápido                    | Cambios menores en archivos conocidos|
| `git stash pop`       | Recuperar cambios temporales           | Al volver a una tarea interrumpida  |
| `git log --oneline`   | Ver historial simplificado             | Buscar un commit específico         |

---

## 📅 Ritmo de Trabajo Recomendado

1. **Empieza fresco:**  
   ```bash
   git pull origin main
   ```
2. **Trabaja en tu feature:**  
   ```bash
   git checkout -b mi-feature
   ```
3. **Guarda progreso:**  
   ```bash
   git add -p
   git commit -m "Mensaje descriptivo"
   ```
4. **Sincroniza:**  
   ```bash
   git push origin mi-feature
   ```
5. **Mergea cuando esté listo** (vía PR/MR en GitHub/GitLab)

---

## 💡 Consejos Pro

- **Commits atómicos:** Cada commit debe ser un cambio lógico único
- **Mensajes claros:** Usa el formato: "Tipo: Descripción" (ej: "Feat: Add user login")
- **`.gitignore`:** Configúralo bien para no trackear archivos innecesarios
- **GUI opcional:** Usa GitKraken o GitHub Desktop para visualizar mejor el historial

---

🚀 **¡Ahora estás listo para trabajar como un profesional con Git!**  
¿Dudas? Recuerda que `git help <comando>` es tu amigo.