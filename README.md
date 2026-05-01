# 📱 Generador Profesional de Códigos QR

![Versión](https://img.shields.io/badge/versión-2.0-blue)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?logo=githubpages&logoColor=white)

> **Aplicación web profesional para generar códigos QR desde cualquier texto o URL. Incluye miniaturas en el historial, botones individuales de Recargar/Eliminar, descarga en PNG y SVG, y un diseño moderno de dos columnas.**

🔗 **Demo en vivo:** [https://tu-usuario.github.io/tu-repositorio](https://tu-usuario.github.io/tu-repositorio)

---

## 📋 Tabla de Contenidos

- [Capturas de pantalla](#-capturas-de-pantalla)
- [Características principales](#-características-principales)
- [Novedades v2.0](#-novedades-v20)
- [Tecnologías utilizadas](#️-tecnologías-utilizadas)
- [Instalación y uso local](#-instalación-y-uso-local)
- [Despliegue en GitHub Pages](#-despliegue-en-github-pages)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Hoja de ruta](#-hoja-de-ruta)
- [Contribuciones](#-contribuciones)
- [Licencia](#-licencia)
- [Autor](#-autor)

---

## 🖼️ Capturas de pantalla

| Generador principal | Historial con miniaturas |
|:---:|:---:|
| ![Generador QR v2](https://via.placeholder.com/450x350?text=Generador+con+PNG/SVG) | ![Historial con miniaturas](https://via.placeholder.com/350x450?text=Miniaturas+y+botones) |

*(Reemplaza los placeholders con capturas reales de tu aplicación)*

---

## ✨ Características principales

| Área | Funcionalidad |
|------|----------------|
| 🔲 **Generador QR** | Convierte cualquier texto o URL en código QR instantáneo |
| 🖼️ **Miniaturas en historial** | Vista previa visual de cada QR generado anteriormente |
| 🔄 **Recargar desde historial** | Un clic recupera cualquier código anterior y lo regenera |
| 🗑️ **Eliminación selectiva** | Borra elementos individuales del historial sin afectar al resto |
| 🧹 **Limpiar todo** | Vacía el historial completo con un solo botón |
| 📥 **Descarga dual** | Exporta tu QR en **PNG** (raster) o **SVG** (vectorial, ideal para impresión) |
| 🎨 **Diseño profesional** | Interfaz limpia, tipografía moderna, sin emojis, dos columnas responsive |
| 💾 **Persistencia local** | El historial se guarda automáticamente en tu navegador (localStorage) |
| ⌨️ **Productividad** | Atajo `Ctrl + Enter` para generar desde el teclado |
| 📱 **Responsive** | Funciona perfectamente en móviles, tablets y escritorio |
| 🌐 **Sin servidor** | 100% frontend – no requiere backend ni API keys |

---

## 🆕 Novedades v2.0

| Característica | Descripción |
|----------------|-------------|
| 🖼️ **Miniaturas en el historial** | Cada código guardado muestra una vista previa en miniatura del QR |
| ⚡ **Botones individuales** | Cada elemento del historial tiene botones "Recargar" (carga el texto) y "Eliminar" (borra solo ese elemento) |
| 📥 **Descarga en SVG** | Además de PNG, ahora puedes descargar el código en formato vectorial (escalable sin pérdida) |
| 🎨 **Diseño profesional** | Interfaz de dos columnas, tarjetas con sombras, tipografía moderna y sin emojis |
| 🧹 **Limpiar historial completo** | Botón independiente para vaciar todos los registros |
| ⌨️ **Atajo de teclado** | Presiona `Ctrl + Enter` para generar el QR rápidamente |
| 💾 **Persistencia mejorada** | El historial guarda hasta 12 códigos con metadatos (tipo, timestamp) |

---

## 🛠️ Tecnologías utilizadas

| Tecnología | Propósito |
|------------|-----------|
| **HTML5** | Estructura semántica de la aplicación |
| **CSS3** | Estilos modernos (gradientes, tarjetas, sombras, responsive) |
| **JavaScript (Vanilla)** | Lógica de generación, persistencia, miniaturas y eventos |
| **QRCode.js** | Biblioteca CDN para generar códigos QR |
| **html2canvas** | (Opcional) Para capturas de alta calidad |
| **localStorage** | Almacenamiento del historial en el navegador |
| **GitHub Pages** | Alojamiento gratuito y despliegue continuo |

---

## 💻 Instalación y uso local

Sigue estos pasos para ejecutar la aplicación en tu máquina:

### Prerrequisitos
- Un navegador web moderno (Chrome, Firefox, Edge, Safari)
- (Opcional) Git y un editor de código

### Pasos

1. **Clona el repositorio**
   ```bash
   git clone https://github.com/tu-usuario/tu-repositorio.git
   cd tu-repositorio
