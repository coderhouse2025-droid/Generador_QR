# 📱 Generador de Códigos QR / CR

![Versión](https://img.shields.io/badge/versión-1.0.0-blue)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?logo=githubpages&logoColor=white)

> **Aplicación web ligera y sin servidor para generar códigos QR desde cualquier texto o URL. Con historial automático, descarga de imágenes y despliegue instantáneo en GitHub Pages.**

🔗 **Demo en vivo:**  https://coderhouse2025-droid.github.io/Generador_QR/

---

## 📋 Tabla de Contenidos

- [Capturas de pantalla](#-capturas-de-pantalla)
- [Características principales](#-características-principales)
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

| Vista principal | Generando QR | Historial |
|:---:|:---:|:---:|
| ![Vista principal](https://via.placeholder.com/300x200?text=Interfaz+principal) | ![Generando QR](https://via.placeholder.com/300x200?text=QR+generado) | ![Historial](https://via.placeholder.com/300x200?text=Historial) |

*(Reemplaza los placeholders con capturas reales de tu app)*

---

## ✨ Características principales

| Funcionalidad | Descripción |
|---------------|-------------|
| 🔲 **Generación instantánea** | Crea códigos QR desde cualquier texto o URL en milisegundos |
| 💾 **Descarga de imágenes** | Guarda el QR como PNG con nombre único (`codigo_qr_timestamp.png`) |
| 📜 **Historial persistente** | Almacena automáticamente los últimos 10 códigos generados (usando `localStorage`) |
| 🖱️ **Historial clickable** | Recupera cualquier código anterior con un solo clic |
| ⌨️ **Atajo de teclado** | Presiona `Ctrl + Enter` desde el área de texto para generar rápidamente |
| 📱 **Diseño responsive** | Funciona perfectamente en móviles, tablets y escritorio |
| 🌐 **Sin servidor** | 100% frontend – no requiere backend ni API keys |
| 🚀 **Cero instalación** | Abre el archivo `index.html` y funciona inmediatamente |

---

## 🛠️ Tecnologías utilizadas

| Tecnología | Propósito |
|------------|-----------|
| **HTML5** | Estructura semántica de la aplicación |
| **CSS3** | Estilos modernos (gradientes, animaciones, responsive) |
| **JavaScript (Vanilla)** | Lógica de generación, persistencia y eventos |
| **QRCode.js** | Biblioteca CDN para generar códigos QR |
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
