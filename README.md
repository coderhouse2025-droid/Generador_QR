# 📱 Generador QR — Web App para creación de códigos QR

[![Demo en vivo](https://img.shields.io/badge/Demo-Live-brightgreen?style=for-the-badge)](https://coderhouse2025-droid.github.io/Generador_QR/)
[![Sin backend](https://img.shields.io/badge/Backend-Ninguno-lightgrey?style=for-the-badge)](#)
[![PNG/SVG](https://img.shields.io/badge/Export-PNG_/_SVG-orange?style=for-the-badge)](#3-descarga-en-png-y-svg--no-solo-png)

> Aplicación web para generar códigos QR a partir de texto o URLs, con previsualización en miniatura, historial de códigos generados, descarga en PNG y SVG, y diseño moderno. Sin backend, corre 100% en el navegador.

🔗 **Demo:** https://coderhouse2025-droid.github.io/Generador_QR/

---

## 📋 Índice

- [Descripción](#-descripción)
- [Caso de negocio](#-caso-de-negocio)
- [Decisiones técnicas y su justificación](#-decisiones-técnicas-y-su-justificación)
- [Arquitectura del sistema](#-arquitectura-del-sistema)
- [Pipeline de entrada: del texto crudo al QR válido](#-pipeline-de-entrada-del-texto-crudo-al-qr-válido)
- [¿Por qué este camino y no otro?](#-por-qué-este-camino-y-no-otro)
- [Relación con el proyecto Stock](#-relación-con-el-proyecto-stock)
- [Funcionalidades](#-funcionalidades)
- [Cómo usar](#-cómo-usar)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Limitaciones conocidas y roadmap](#-limitaciones-conocidas-y-roadmap)

---

## 📋 Descripción

**Generador QR** es una aplicación web que convierte texto, URLs, emails o cualquier cadena de caracteres en un código QR descargable. Genera una miniatura de previsualización en tiempo real mientras el usuario escribe, mantiene un historial de los últimos códigos generados en la sesión, y permite exportar el resultado en PNG (para uso digital e impresión) o SVG (para uso en diseño escalable).

---

## 💼 Caso de negocio

### El problema que resuelve

Generar un código QR debería ser una operación de 10 segundos. Sin embargo, la mayoría de las herramientas online disponibles tienen uno o varios de estos problemas:

- **Publicidad invasiva** que interrumpe el flujo de trabajo
- **Formularios de registro** obligatorios antes de poder usar la herramienta
- **Límites de uso** en el tier gratuito (X QRs por día, resolución máxima limitada)
- **Descarga solo en un formato** (generalmente PNG de baja resolución)
- **Sin historial** — si se cierra la pestaña, el QR generado se pierde
- **Interfaz lenta** que requiere esperar la respuesta de un servidor para mostrar el QR

Este generador resuelve todos esos puntos de una vez: sin publicidad, sin registro, sin límites, con historial, con múltiples formatos de descarga, y con generación instantánea en el browser sin esperar ningún servidor.

### Los usuarios del sistema

El perfil es amplio, pero los casos de uso más frecuentes son:

- **Comerciantes** que necesitan generar QRs para menús, redes sociales, o links de pago (Mercado Pago, WhatsApp Business)
- **Docentes y estudiantes** que generan QRs para materiales educativos, presentaciones o trabajos
- **Diseñadores y comunicadores** que necesitan el QR en SVG para insertarlo en Adobe Illustrator o Canva sin pérdida de calidad
- **Desarrolladores** que prueban cómo se ve un QR antes de implementar la generación en su propio sistema (como en el proyecto Stock)

### La conexión con el ecosistema de proyectos

Este generador no es un proyecto aislado. Es la herramienta de apoyo al proyecto [Stock](https://github.com/coderhouse2025-droid/Stock): cuando el sistema de inventario necesita etiquetar un producto nuevo con QR, la generación y descarga de esa etiqueta sigue exactamente el mismo flujo que este generador. Tener la herramienta como proyecto independiente permite usarla fuera del contexto del inventario, para cualquier necesidad de generación de QR.

---

## 🧠 Decisiones técnicas y su justificación

### 1. HTML + JavaScript Vanilla — no React, no framework

**¿Por qué Vanilla?**

Un generador de QR tiene un estado extremadamente simple: un campo de texto, un output visual, y una lista de historial. No hay routing, no hay estado compartido entre componentes, no hay efectos secundarios complejos. Introducir React para este caso sería añadir ~45KB de bundle, un paso de build, y una abstracción que no resuelve ningún problema real de este proyecto.

La generación de QR en el browser es una operación puramente funcional: `texto → QR`. Vanilla JS modela esto de forma directa y sin overhead.

**Performance como funcionalidad:** la generación en tiempo real mientras el usuario escribe (previsualización instantánea) requiere que el código de renderizado sea lo más rápido posible. Vanilla JS sin capa de abstracción de Virtual DOM es la opción de menor latencia.

---

### 2. QRCode.js — no qrcode-generator, no API externa

**¿Por qué QRCode.js?**

QRCode.js es la librería más establecida para generación de QR en el browser: sin dependencias, output en Canvas o SVG, soporte para todos los niveles de corrección de errores, y ~15KB minificada.

La alternativa de usar una API externa (Google Charts API, QR Server API) implicaría:
- Latencia de red en cada generación (300-800ms vs. <5ms local)
- Dependencia de un servicio externo que puede caer o cambiar su API
- Imposibilidad de funcionamiento offline
- Potencial logging de los contenidos generados por el servidor externo

Para un generador donde la privacidad del contenido importa (el usuario puede estar generando QRs con datos personales, URLs privadas, o información comercial), la generación local es la única opción correcta.

**Comparativa:**

| Opción | Latencia | Offline | Privacidad | Formato output |
|--------|----------|---------|-----------|----------------|
| **QRCode.js** | <5ms | ✅ | ✅ Total | Canvas + SVG |
| qrcode-generator | <5ms | ✅ | ✅ Total | Solo tabla HTML |
| Google Charts API | 300-800ms | ❌ | ❌ | Solo PNG |
| QR Server API | 300-800ms | ❌ | ❌ | PNG/SVG |

---

### 3. Descarga en PNG y SVG — no solo PNG

**¿Por qué ofrecer SVG además de PNG?**

PNG y SVG resuelven necesidades distintas:

- **PNG** es el formato correcto para uso digital (compartir por WhatsApp, insertar en Google Slides, publicar en redes sociales). Es compatible con cualquier aplicación o sistema.

- **SVG** es el formato correcto para diseño. Un QR en SVG puede escalarse a cualquier tamaño — desde un sticker de 2cm hasta un banner de 2 metros — sin pérdida de calidad. Para un diseñador que va a insertar el QR en un cartel para imprimir en alta resolución, un PNG de 512px es completamente insuficiente.

La implementación de la descarga en cada formato tiene lógica diferente:

**PNG — via canvas:**
```javascript
function descargarPNG() {
  const canvas = document.querySelector('#qr-output canvas');
  const link = document.createElement('a');
  link.download = `qr-${Date.now()}.png`;
  link.href = canvas.toDataURL('image/png');
  link.click();
}
```

**SVG — via serialización del DOM:**
```javascript
function descargarSVG() {
  const svg = document.querySelector('#qr-output svg');
  const serializer = new XMLSerializer();
  const svgString = serializer.serializeToString(svg);
  const blob = new Blob([svgString], { type: 'image/svg+xml' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.download = `qr-${Date.now()}.svg`;
  link.href = url;
  link.click();
  URL.revokeObjectURL(url); // liberar memoria
}
```

**¿Por qué `URL.revokeObjectURL` después de la descarga?**

`createObjectURL` crea una referencia en memoria que no se libera automáticamente. Si el usuario genera y descarga 50 QRs en una sesión sin `revokeObjectURL`, la memoria del browser crece indefinidamente. La revocación inmediata después del click garantiza que cada descarga no deja residuos en memoria.

---

### 4. Historial con miniaturas — sessionStorage + canvas thumbnail

**¿Por qué mantener un historial y no solo mostrar el QR actual?**

El flujo de trabajo real de un usuario que genera múltiples QRs es: generar → descargar → generar otro → descargar → eventualmente querer volver al primero. Sin historial, el QR anterior se pierde en cuanto se escribe algo nuevo en el input.

El historial resuelve esto sin fricciones: cada QR generado queda como miniatura clickeable. El usuario puede volver a cualquier QR anterior, previsualizarlo en grande, y descargarlo nuevamente.

**Implementación de las miniaturas:**

```javascript
function agregarAlHistorial(texto, canvasOriginal) {
  // Crear miniatura reducida para el historial
  const miniCanvas = document.createElement('canvas');
  miniCanvas.width = 80;
  miniCanvas.height = 80;
  const ctx = miniCanvas.getContext('2d');
  ctx.drawImage(canvasOriginal, 0, 0, 80, 80);

  const entrada = {
    texto: texto.slice(0, 50) + (texto.length > 50 ? '...' : ''),
    miniatura: miniCanvas.toDataURL('image/png'),
    timestamp: new Date().toLocaleTimeString('es-AR'),
    textoCompleto: texto
  };

  historial.unshift(entrada); // más reciente primero
  if (historial.length > 10) historial.pop(); // máximo 10 entradas
  renderizarHistorial();
}
```

**¿Por qué máximo 10 entradas?**

Cada miniatura en base64 ocupa ~2-4KB en memoria. Con 10 entradas, el historial consume ~40KB — negligible. A partir de 50+ entradas, el panel de historial se vuelve inutilizable visualmente y el consumo de memoria empieza a ser perceptible. 10 es el balance entre utilidad práctica y performance.

**¿Por qué sessionStorage y no localStorage?**

El historial de QRs generados es relevante solo durante la sesión actual de uso. Si el usuario cierra el browser y vuelve mañana, el contexto de por qué generó esos QRs se perdió — mostrar un historial de sesiones anteriores generaría confusión más que valor. sessionStorage vive exactamente lo que dura la pestaña abierta, que es el scope correcto para este historial.

---

### 5. Nivel de corrección de errores: M (15%) por defecto

**¿Qué es el nivel de corrección de errores en QR?**

Los códigos QR incorporan redundancia de datos que permite reconstruir el contenido aunque parte del código esté dañado o tapado. Hay cuatro niveles:

| Nivel | Recuperación | Densidad visual | Caso de uso |
|-------|-------------|-----------------|-------------|
| L | 7% | Menor (QR más limpio) | Digital, sin riesgo de daño |
| **M** | **15%** | **Medio** | **Uso general — balance óptimo** |
| Q | 25% | Mayor | Entornos industriales |
| H | 30% | Mayor (QR más denso) | Con logo superpuesto al centro |

**¿Por qué M como default y no L?**

L genera QRs más limpios y fáciles de escanear en condiciones perfectas. Sin embargo, un QR impreso en papel puede ensuciarse, arrugarse, o tener parte tapada. Con corrección L, una pequeña mancha puede hacer el QR irrecuperable. M ofrece un 15% de tolerancia a daño sin aumentar significativamente la densidad del código — es el balance correcto para uso general.

El nivel H se reserva para casos donde se va a superponer un logo al centro del QR (práctica común en marketing de marca). En esos casos, el logo tapa intencionalmente parte del código, y la alta corrección permite que siga siendo escaneable.

---

### 6. TailwindCSS via CDN — diseño moderno sin CSS propio

**¿Por qué Tailwind?**

El valor diferencial de esta app frente a otras herramientas de QR disponibles online incluye el **diseño visual**. La descripción del proyecto lo menciona explícitamente: "diseño moderno". Tailwind permite construir una interfaz visualmente pulida — con gradientes, sombras, transiciones, modo oscuro — directamente en el markup sin escribir ni mantener hojas de estilo propias.

Para un proyecto donde la apariencia es parte de la propuesta de valor (frente a herramientas de QR genéricas y cargadas de publicidad), Tailwind es la elección que maximiza la calidad visual con el mínimo tiempo de desarrollo.

---

## 🏗️ Arquitectura del sistema

```
Usuario
    │
    ├── Escribe texto / URL en el input
    │       └── Evento input → debounce 300ms → generarQR()
    │               └── QRCode.js → render en Canvas + render en SVG
    │                       └── agregarAlHistorial() → miniatura en panel
    │
    ├── Hace clic en "Descargar PNG"
    │       └── canvas.toDataURL() → link.click() → descarga
    │
    ├── Hace clic en "Descargar SVG"
    │       └── XMLSerializer → Blob → createObjectURL → link.click() → revokeObjectURL
    │
    └── Hace clic en miniatura del historial
            └── Restaurar texto en input → regenerar QR en tamaño completo
```

**Debounce en la generación:**

La generación se activa con un debounce de 300ms sobre el evento `input`. Sin debounce, cada keystroke dispara una regeneración completa del QR — para un texto de 50 caracteres, eso son 50 renderizados mientras el usuario escribe. Con debounce, la generación ocurre solo cuando el usuario deja de escribir por 300ms.

```javascript
let debounceTimer;
inputTexto.addEventListener('input', () => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(() => generarQR(), 300);
});
```

---

## 🔄 Pipeline de entrada: del texto crudo al QR válido

A diferencia de los otros proyectos del portfolio (que procesan datasets CSV con problemas de calidad), el Generador QR procesa input del usuario en tiempo real. El "dataset" aquí es el texto que escribe el usuario, que puede tener sus propios problemas.

---

### Problema 1: Input vacío o solo espacios

Si el usuario borra todo el texto y el input queda vacío, QRCode.js lanza un error silencioso o genera un QR en blanco.

**Transformación aplicada:**

```javascript
function generarQR() {
  const texto = inputTexto.value.trim();
  if (!texto) {
    ocultarQR();
    return;
  }
  // Proceder con la generación
}
```

`.trim()` elimina espacios al inicio y al final. Si después del trim el string está vacío, se oculta el QR y se muestra el estado vacío en lugar de un error.

---

### Problema 2: URLs sin protocolo

Si el usuario ingresa `www.google.com` sin `https://`, el QR se genera correctamente como texto pero al escanearlo el celular lo interpreta como texto plano, no como URL cliqueable. El usuario espera que al escanear se abra el navegador.

**Transformación aplicada:**

```javascript
function normalizarURL(texto) {
  const esURL = /^(www\.|[a-zA-Z0-9-]+\.[a-zA-Z]{2,})/.test(texto);
  if (esURL && !texto.startsWith('http')) {
    return 'https://' + texto;
  }
  return texto;
}
```

Si el texto parece una URL (empieza con `www.` o tiene la forma `dominio.tld`) y no tiene protocolo, se agrega `https://` automáticamente. El usuario ve el texto normalizado en el input para que sepa qué se codificó en el QR.

---

### Problema 3: Texto excesivamente largo

El estándar QR puede codificar hasta 4.296 caracteres en modo alfanumérico. Pero a medida que el contenido crece, el código QR se vuelve más denso y más difícil de escanear con cámaras de baja calidad.

**Transformación aplicada — indicador de capacidad:**

```javascript
const LIMITE_RECOMENDADO = 500; // caracteres
const LIMITE_MAXIMO = 2000;

function actualizarIndicadorCapacidad(texto) {
  const longitud = texto.length;
  const porcentaje = Math.min((longitud / LIMITE_RECOMENDADO) * 100, 100);

  if (longitud > LIMITE_MAXIMO) {
    mostrarAlerta('El texto es demasiado largo. El QR puede no ser escaneable.');
  } else if (longitud > LIMITE_RECOMENDADO) {
    mostrarAlerta('Texto largo — el QR será más denso y difícil de escanear.');
  }

  barraCapacidad.style.width = porcentaje + '%';
}
```

No se bloquea al usuario — puede generar un QR con 2.000 caracteres si lo necesita. Pero se le informa visualmente del trade-off: más texto = QR más denso = peor escaneabilidad en dispositivos básicos.

---

### Problema 4: Caracteres especiales y encodings

Los QRs pueden codificarse en distintos modos: numérico (solo dígitos), alfanumérico (dígitos + mayúsculas + símbolos básicos), byte (cualquier caracter ISO-8859-1), y Kanji. QRCode.js selecciona automáticamente el modo más eficiente según el contenido, pero hay caracteres que pueden causar problemas de visualización si el dispositivo de escaneo no soporta el encoding.

**Transformación aplicada — detección de caracteres fuera del rango seguro:**

```javascript
function tieneCaracteresUnicode(texto) {
  return /[^\x00-\xFF]/.test(texto); // caracteres fuera de Latin-1
}

if (tieneCaracteresUnicode(texto)) {
  mostrarAviso('El texto contiene caracteres especiales (emojis, chino, árabe). ' +
    'Algunos lectores de QR pueden no decodificarlos correctamente.');
}
```

No se bloquea la generación — el QR se crea igual. El aviso informa al usuario de un posible problema de compatibilidad antes de que lo imprima y lo pegue en un producto.

---

## 🤔 ¿Por qué este camino y no otro?

### Alternativa descartada: API externa (Google Charts, QR Server)

Ya cubierto en la sección de tecnologías: latencia, dependencia, privacidad, y funcionamiento offline son razones suficientes para descartar APIs externas para un generador de uso frecuente.

### Alternativa descartada: generar QR en el servidor (Node.js + sharp)

Un backend con Node.js y la librería `qrcode` de npm puede generar QRs de alta calidad con más opciones de personalización (colores, logo al centro, bordes redondeados). El trade-off: requiere servidor, costos de hosting, y latencia de red. Para un generador de uso diario donde la velocidad es parte de la experiencia, la generación local es irreemplazable.

La generación server-side sería la elección correcta si se necesitara: QRs con logo de empresa superpuesto, integración con un flujo de trabajo empresarial (generación masiva desde una base de datos), o personalización visual avanzada. Para uso individual y frecuente, el browser es suficiente y más rápido.

### Alternativa descartada: solo PNG sin SVG

PNG-only es la solución más simple. SVG se agregó porque resuelve un problema real para un segmento específico de usuarios (diseñadores, imprenta) que el PNG no puede resolver: la escalabilidad vectorial. El costo de implementar SVG fue bajo (la misma librería lo soporta); el valor agregado para ese segmento es alto.

---

## 🔗 Relación con el proyecto Stock

El [Sistema de Control de Inventarios (Stock)](https://github.com/coderhouse2025-droid/Stock) usa la misma lógica de generación QR internamente para etiquetar productos nuevos. La diferencia:

- En **Stock**, el QR se genera automáticamente al crear un producto y se imprime como etiqueta — el contenido está predefinido por el sistema (código interno del producto).
- En el **Generador QR**, el usuario controla completamente el contenido — puede ser una URL, un número de teléfono, un mensaje de WhatsApp, un email, o cualquier texto.

Tener el generador como herramienta independiente permite que sea útil fuera del contexto del inventario, para cualquier necesidad de generación de QR sin instalar nada ni crear una cuenta.

---

## ✨ Funcionalidades

- ✍️ **Generación en tiempo real** — el QR se actualiza mientras el usuario escribe (con debounce)
- 🔗 **Normalización automática de URLs** — agrega `https://` si falta el protocolo
- 🖼️ **Previsualización de alta calidad** — render en Canvas y SVG simultáneamente
- 📥 **Descarga en PNG** — para uso digital, redes sociales, presentaciones
- 📥 **Descarga en SVG** — para diseño escalable, impresión en alta resolución
- 🕐 **Historial con miniaturas** — los últimos 10 QRs generados en la sesión
- ↩️ **Restaurar desde historial** — clic en miniatura para recuperar un QR anterior
- ⚠️ **Indicador de capacidad** — alerta visual cuando el texto es demasiado largo
- 🌐 **Funciona offline** — sin llamadas de red durante la operación
- 📱 **Responsive** — optimizado para móvil, tablet y desktop

---

## 🚀 Cómo usar

1. Escribir el texto, URL, email o número de teléfono en el campo de entrada
2. El QR se genera automáticamente en tiempo real
3. Hacer clic en **Descargar PNG** para uso digital o **Descargar SVG** para diseño
4. Los QRs generados aparecen como miniaturas en el historial — hacer clic para recuperar cualquiera

**Formatos de input compatibles:**

| Tipo | Ejemplo | Comportamiento |
|------|---------|----------------|
| URL | `https://mi-sitio.com` | QR abre el navegador al escanear |
| URL sin protocolo | `www.mi-sitio.com` | Se normaliza automáticamente a `https://` |
| WhatsApp | `https://wa.me/5491112345678` | QR abre WhatsApp al escanear |
| Email | `mailto:nombre@email.com` | QR abre el cliente de email |
| Teléfono | `tel:+5491112345678` | QR llama al número |
| Texto libre | `Cualquier mensaje` | QR muestra el texto al escanear |

---

## 📁 Estructura del proyecto

```
/
├── index.html     # Aplicación completa: markup + estilos (Tailwind) + lógica JS
│                  # QRCode.js cargado desde CDN
└── README.md
```

**¿Por qué un único archivo?**

Consistencia con la arquitectura del resto del ecosistema de proyectos: portabilidad total, sin build step, sin dependencias de Node.js. El usuario puede guardar el archivo y usarlo localmente sin servidor.

---

## ⚠️ Limitaciones conocidas y roadmap

| Limitación | Impacto | Solución futura |
|------------|---------|----------------|
| **Sin personalización de colores** | El QR siempre es negro sobre blanco | Selector de color de módulos y fondo |
| **Sin logo superpuesto** | No se puede poner logo de empresa en el centro | Generación server-side con nivel H de corrección |
| **Historial solo en sesión** | Se pierde al cerrar el browser | Historial persistente con localStorage y opción de exportar |
| **Sin generación masiva** | Solo un QR a la vez | Modo batch: subir CSV de textos → descargar ZIP de QRs |
| **Sin analytics de escaneo** | No se sabe cuántas veces se escaneó un QR | Integración con URLs de tracking (requiere backend) |

---

## 👨‍💻 Autor

**Juan Manuel Orellana**

---

## 📄 Licencia

MIT License — libre para uso, adaptación y distribución.
