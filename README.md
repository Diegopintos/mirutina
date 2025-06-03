# mirutina
# Guía de la Aplicación de Entrenamiento

## 1. Vistas / Secciones

- **Inicio** (`#inicio`)
  - Botones Día 1–5, estadísticas globales (días completados, tiempo total, ejercicios realizados, calorías totales, días perdidos).
- **Rutina** (`#rutina`)
  - Cronómetro, slideshow de imágenes, video lightbox, lista de ejercicios, barra de progreso, controles (Iniciar, Pausar, +1, Siguiente/Finalizar).
- **Catálogo de Ejercicios** (`#catalogo-ejercicios`)
  - Filtros por nivel y equipo.
- **Catálogo de Felicitaciones** (`#catalogo-felicitaciones`)
  - Galería de frases e imágenes motivacionales.
- **Multimedia** (`#multimedia`)
  - Galería de todos los vídeos e imágenes.
- **Resultados Off-canvas** (`#resultados-offcanvas`)
  - Historial mensual de tiempo, calorías y extras.
- **Opciones** (`#opciones`)
  - Switches de sonido, selector Dark/Light (guardado en localStorage), acceso a Logros.
- **Logros** (`#seccion-logros`)
  - 60 badges (diarios, semanales, acumulativos), con icono/imagen, título, fecha, tooltip, barra de progreso opcional.
- **Modal Motivacional** (`#modal-motivacional`)
  - Frase, imagen de felicitación, resumen del día.
- **Modal Logro** (`#modal-logro`)
  - Icono/imágen y título del logro desbloqueado.

## 2. Flujos y Funcionalidades

1. **Carga de CSV** con PapaParse.
2. **Persistencia** con LocalForage y localStorage.
3. **Helper** `ocultarTodasLasSecciones()` y funciones `mostrarSeccionX()`.
4. **Cronómetro** y lógica de ejercicio.
5. **Estadísticas** en tiempo real (XP, nivel, racha).
6. **Días rojos**: penalización visual y contador.
7. **Gamificación**:
   - Cálculo de XP, nivel, racha.
   - 60 logros, `checkLogros()`, `desbloquear()`.
   - Reseteos diarios y semanales.
8. **UIkit** para componentes, modals, tooltips.
9. **Lottie** para preload.
10. **Tema** Dark/Light.

## 3. Tecnologías

- HTML5, CSS3, Vanilla JS (ES6)
- UIkit
- PapaParse
- LocalForage
- Lottie

## 4. Guía de Uso

1. **Inicio**: navegar a día.
2. **Rutina**: controles y resumen.
3. **Ver estadísticas** y **Logros**.
4. **Opciones**: sonido y tema.
5. **Catálogos** y **Multimedia**.
6. **Prueba de logros** desde consola.

## 5. Manual de Funcionamiento

### 5.1 Instalación y despliegue
1. Coloca los archivos `index.html`, `styles.css`, `script.js` y las carpetas `/data` y `/media` en tu servidor.
2. Asegúrate de servir el sitio bajo HTTPS para evitar problemas de contenido mixto.
3. Incluye en tu HTML:
   ```html
   <link rel="stylesheet" href="styles.css">
   <script src="https://cdn.jsdelivr.net/npm/papaparse@5.4.1/papaparse.min.js"></script>
   <script src="https://cdn.jsdelivr.net/npm/localforage@1.10.0/dist/localforage.min.js"></script>
   <script src="script.js" defer></script>
   ```

### 5.2 Estructura de archivos
```
/index.html
/styles.css
/script.js
/data/ejercicios_bd_met.csv
/data/frases_motivacionales.csv
/media/images/...        (imágenes de ejercicios)
/media/videos/...        (videos de ejercicios)
/media/feliz/...         (banners de logros)
```

### 5.3 Configuración inicial
- Al cargar por primera vez, la app descarga los CSV y muestra un preloader.
- Se inicializa `gam` desde LocalForage (vacío si es la primera ejecución).
- Se cargan opciones de tema y sonidos desde localStorage.

### 5.4 Navegación principal
- **Botón Día X**: llama a `cargarRutina(X)`, que oculta todas las secciones y muestra `#rutina`.
- **Menú lateral**: enlaces con `onclick="mostrarSeccionLogros()"`, `mostrarCatalogoEjercicios()`, etc., que usan `ocultarTodasLasSecciones()`.

### 5.5 Uso de la rutina
- **Iniciar**: llama a `iniciarEjercicio()`, comienza cronómetro y rellena datos.
- **Pausar**: `pausarRutina()`, detiene el cronómetro.
- **+1**: `sumarRepeticion()`, añade repeticiones extra.
- **Siguiente**: `mostrarSiguiente(event)`, avanza ejercicios y renombra botón a “Finalizar” en el último.
- **Finalizar**: `finalizarRutina()`, consolida datos, guarda estado, muestra modal y desbloquea logros.

### 5.6 Gamificación y logros
- **consolidarGamificacion()**: suma XP, ajusta nivel y racha, actualiza UI y llama `checkLogros()`.
- **checkLogros()**: recorre `gam.logros` y llama `desbloquear(l)` si cumple meta.
- **desbloquear(l)**: marca `unlocked`, guarda fecha, muestra modal `#modal-logro` y actualiza galería.

### 5.7 Reseteos automáticos
- **Reset diario**: al llamar `resetLogrosDiarios()` (se invoca al cargar un nuevo día).
- **Reset semanal**: en `resetDiasSemana()`, cada lunes limpia logros semanales y días perdidos.

### 5.8 Personalización
- **Tema Dark/Light**: switch en Opciones, clases `theme-dark` y `theme-light` en `<body>`.
- **Sonidos**: toggles en Opciones guardados en un objeto `opcionesGlobales`.

### 5.9 Depuración y consola
- Pruébalo con la consola:
  ```js
  // Lista de logros actual
  console.table(gam.logros);
  // Desbloquear logros de prueba
  Object.values(gam.logros).forEach(l => { l.unlocked = true; l.date = hoyISO(); });
  renderGaleriaLogros(); mostrarSeccionLogros();
  ```

### 5.10 Buenas prácticas
- Recarga con **Ctrl+Shift+R** para limpiar caché y estados temporales.
- Mantén los CSV con la cabecera correcta y 12 columnas (incluyendo Video).
- Usa HTTPS para cargar multimedia y evitar Mixed Content.

---

*Fin del manual.*
