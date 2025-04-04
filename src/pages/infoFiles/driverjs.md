---
    title: 'Contenido Pagina'
    layout: '../../layouts/LayoutMarkdown.astro'
---
Aquí tienes una guía detallada sobre cómo integrar **Driver.js** en tu proyecto web para crear tours interactivos:

---

## 📦 **Instalación de Driver.js**

Puedes incluir **Driver.js** en tu proyecto a través de un CDN. Solo necesitas agregar las siguientes líneas en tu archivo HTML:

```html
<p>hola</p>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/driver.js/0.9.6/driver.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/driver.js/0.9.6/driver.min.js"></script>
```

---

## 🔑 **Ejemplo Básico**

Este es un ejemplo simple de cómo usar Driver.js en un archivo HTML:

```html
<!DOCTYPE html>
<html lang="es">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Ejemplo de Driver.js</title>
		<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/driver.js/0.9.6/driver.min.css" />
		<style>
			body {
				font-family: Arial, sans-serif;
			}
			.highlight {
				margin: 20px 0;
			}
			#input-field {
				margin-top: 10px;
			}
		</style>
	</head>
	<body>
		<h1>Bienvenido a mi aplicación</h1>
		<div class="highlight">
			<button id="start-tour">Iniciar Tour</button>
		</div>

		<div class="highlight">
			<label for="input-field">Ingrese su nombre:</label>
			<input type="text" id="input-field" placeholder="Nombre" />
		</div>

		<script>
			const driver = new Driver();

			const steps = [
				{
					element: "#start-tour",
					popover: {
						title: "¡Hola!",
						description: "Este botón inicia el tour.",
						position: "bottom",
					},
				},
				{
					element: "#input-field",
					popover: {
						title: "Campo de entrada",
						description: "Aquí puedes ingresar tu nombre.",
						position: "top",
					},
				},
			];

			document.getElementById("start-tour").addEventListener("click", () => {
				driver.defineSteps(steps);
				driver.start();
			});
		</script>
	</body>
</html>
```

---

## 🎨 **Personalización de los Popovers**

Puedes personalizar el estilo de los popovers utilizando clases CSS. Por ejemplo, para cambiar el fondo y el color del texto:

```css
.custom-popover {
	background-color: #4a90e2; /* Cambia el color de fondo */
	color: white; /* Cambia el color del texto */
	border-radius: 8px; /* Bordes redondeados */
}
```

Luego, para aplicar esta clase personalizada a los popovers, simplemente agrega la propiedad `className` en los pasos del tour:

```javascript
const steps = [
	{
		element: "#start-tour",
		popover: {
			title: "¡Hola!",
			description: "Este botón inicia el tour.",
			position: "bottom",
			className: "custom-popover", // Clase personalizada
		},
	},
	{
		element: "#input-field",
		popover: {
			title: "Campo de entrada",
			description: "Aquí puedes ingresar tu nombre.",
			position: "top",
			className: "custom-popover", // Clase personalizada
		},
	},
];
```

---

## ⚙️ **Opciones de Configuración**

**Driver.js** ofrece varias configuraciones que puedes usar para personalizar la experiencia del tour:

- **position:** Define la posición del popover (`top`, `bottom`, `left`, `right`).
- **className:** Permite agregar clases CSS personalizadas al popover.
- **title:** Título del popover.
- **description:** Descripción que aparecerá en el popover.

---

## 📝 **Conclusión**

**Driver.js** es una herramienta muy útil para crear tours interactivos en tu aplicación web. Con su fácil integración y amplias opciones de personalización, puedes mejorar la experiencia del usuario proporcionándole guías interactivas para interactuar con tu aplicación.

Para obtener más detalles y ejemplos avanzados, puedes consultar la [documentación oficial de Driver.js](https://github.com/kamranahmedfo/driver.js).
