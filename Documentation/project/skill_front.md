# Skill Frontend: Buenas Prácticas y Arquitectura

## 1. Buenas Prácticas de Desarrollo (Guía Básica React/Vite)
El frontend de Mentora está desarrollado sobre la librería **React** empaquetada con **Vite** para maximizar el rendimiento. Las convenciones seguidas son:
- **Componentes Funcionales:** Se utilizan exclusivamente componentes funcionales (*Functional Components*) y *Hooks* (`useState`, `useEffect`). No se utilizan clases.
- **Clean Code (Nomenclatura):** Los componentes utilizan `PascalCase` (ej. `PostTutoringForm.jsx`). Los archivos CSS llevan el mismo nombre del componente al que estilizan para garantizar una organización modular.
- **Single Responsibility:** Cada componente debe tener una única responsabilidad. Si un formulario crece mucho, se debe subdividir en componentes más pequeños.
- **Gestión de Estado Simple:** Para mantener la agilidad del desarrollo monolítico, el estado se maneja a nivel de componente (`useState`) y se "eleva" (*lifting state up*) a `App.jsx` solo cuando es estrictamente necesario, evitando el uso prematuro de librerías complejas como Redux.

## 2. Definición y Aplicación de la Arquitectura
La arquitectura aplicada es **Monolítica (SPA - Single Page Application)**. Todo el cliente se renderiza desde un solo `index.html` y React controla las vistas dinámicamente mediante el DOM Virtual.

### Dependencias y Estructura
- **Capa de Servicios (`src/services/`):** Contiene la comunicación con el Backend. El archivo `api.js` abstrae y centraliza todos los `fetch()` (peticiones HTTP), separando la lógica de la red de la capa visual.
- **Capa de Componentes (`src/components/`):** Componentes aislados y reutilizables (formularios, listas). Importan directamente la API y manejan su propio ciclo de vida.
- **Estética Global (`src/index.css`):** Definición de variables CSS globales (design tokens) aplicando el concepto de *Glassmorphism* y gradientes fluidos para una experiencia *Premium*.

## 3. Revisión de Reglas: MUST y SHOULD Have

### MUST HAVE (Requisitos Críticos y Obligatorios)
- [x] **MUST** manejar promesas de forma asíncrona (`async/await`) en todas las interacciones con la API y utilizar bloques `try/catch` para capturar errores de red.
- [x] **MUST** aislar la lógica de conexión (endpoints) en la carpeta de servicios (`api.js`) y nunca quemar URLs directamente dentro de un componente `jsx`.
- [x] **MUST** implementar *Feedback* visual para el usuario (estados de carga `loading`, mensajes de éxito o error) en cada interacción.
- [x] **MUST** utilizar dependencias exactas en el array de dependencias de los `useEffect` para evitar renders infinitos.

### SHOULD HAVE (Recomendaciones Fuertes)
- [x] **SHOULD** utilizar variables CSS (`:root`) para manejar paletas de colores y espaciados, evitando "Magic Numbers" en los archivos CSS.
- [x] **SHOULD** usar validaciones nativas de HTML5 (`required`, `type="email"`, `min`, `max`) en los formularios antes de procesarlos en React.
- [x] **SHOULD** estructurar el JSX de manera semántica (`<main>`, `<section>`, `<form>`) para mejorar la accesibilidad (A11y).
