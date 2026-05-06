# SimplexOptimizer - Investigación de Operaciones I

## 🎓 Integrantes
*   **Sebastian Jaramillo Hernandez** - 20241020002
*   **Jhoe Miranda Alvarez** - 20241020022

Este proyecto es un sistema interactivo diseñado para modelar y resolver problemas reales de optimización lineal utilizando el método **Simplex de la Gran M**. Desarrollado como parte del curso de Investigación de Operaciones I.

---

## 🚀 Características
- **Método de la Gran M**: Resolución simbólica de problemas con restricciones `<=`, `>=` e `=`.
- **Análisis de Sensibilidad**: Cálculo de precios sombra y rangos de estabilidad.
- **Interfaz Premium**: Desarrollado con React, Tailwind CSS y Framer Motion para una experiencia de usuario fluida y profesional.
- **Visualización Educativa**: Muestra cada iteración del tableau, incluyendo filas $Z_j$ y $C_j - Z_j$.

---

## 🛠️ Mejoras Propuestas con Patrones de Diseño

A continuación se describen las secciones del código que podrían mejorarse aplicando patrones de diseño de software (GoF) para elevar la calidad arquitectónica:

### 1. Patrón Strategy (Estrategia)
*   **Sección:** `src/logic/simplex.js` (Método `runSimplex`).
*   **Por qué:** Actualmente, la regla de selección de la columna pivote está fija (valor más negativo/positivo). 
*   **Mejora:** Implementar una interfaz de estrategia para permitir diferentes reglas de selección (Regla de Bland, Dantzig, etc.) sin modificar el núcleo del algoritmo.

### 2. Patrón Command (Comando)
*   **Sección:** `src/logic/simplex.js` (Operaciones de pivoteo).
*   **Por qué:** Las mutaciones de la matriz ocurren de forma imperativa.
*   **Mejora:** Encapsular cada iteración de pivoteo como un comando. Esto permitiría implementar funciones de **Undo/Redo** o ejecución **paso a paso** real en la interfaz.

### 3. Patrón Memento (Recuerdo)
*   **Sección:** `src/logic/simplex.js` (Método `captureTableau`).
*   **Por qué:** El historial de tablas se gestiona mediante copias manuales de arrays.
*   **Mejora:** Permitir que el solver genere mementos de su estado interno, desacoplando la lógica de historial de la lógica de cálculo.

### 4. Patrón Facade (Fachada)
*   **Sección:** `src/App.jsx` (Integración con el solver).
*   **Por qué:** El componente React interactúa directamente con múltiples métodos del solver y maneja transformaciones de datos.
*   **Mejora:** Crear una fachada que simplifique la API del solver para la UI, recibiendo datos planos y entregando resultados ya formateados.

### 5. Patrón Factory (Fábrica)
*   **Sección:** `src/logic/simplex.js` (Inicialización de variables).
*   **Por qué:** La creación de variables de holgura, exceso y artificiales está mezclada con la construcción de la matriz.
*   **Mejora:** Una fábrica de variables que instancie el tipo correcto según el operador de la restricción, facilitando la extensión a nuevos tipos de variables.

---

## ⚠️ Análisis de Antipatrones

Se han identificado los siguientes antipatrones en la implementación actual que deben ser abordados en futuras refactorizaciones:

1.  **God Object (Objeto Todopoderoso):** `App.jsx` contiene más de 600 líneas manejando UI, estado, lógica de negocio y validaciones. Debe dividirse en componentes más pequeños.
2.  **Spaghetti Code:** Existe una alta mezcla entre la lógica de presentación (React) y la lógica de transformación de datos para el algoritmo.
3.  **Hardcoding (Código a Fuego):** Valores como el coeficiente 'M' (`1e10`) o los placeholders de autores están escritos directamente en el código en lugar de ser configurables.
4.  **Copy-Paste Programming:** La lógica de cálculo de las filas $Z_j$ se repite en varias partes del solver en lugar de estar centralizada en una función auxiliar pura.
5.  **Golden Hammer (Martillo de Oro):** Uso excesivo de `useState` para gestionar estados altamente complejos que se beneficiarían de un `useReducer` o una máquina de estados.
6.  **Magic Numbers:** Uso de literales numéricos (como tolerancias `1e-9`) en múltiples archivos sin estar definidos en un archivo de constantes centralizado.
7.  **Feature Envy (Envidia de Funcionalidades):** El componente de resultados accede y manipula excesivamente las propiedades internas del objeto `solution`, en lugar de que este entregue la información lista para mostrar.
8.  **Long Method (Método Largo):** El método `solve` y el renderizado principal de `App` son excesivamente largos, dificultando su testeo unitario.
9.  **Shotgun Surgery (Cirugía de Escopeta):** Cambiar un detalle del formato de salida del algoritmo requiere modificar múltiples puntos tanto en el solver como en la UI.
10. **Error Hiding (Ocultamiento de Errores):** Se capturan excepciones de forma genérica en el solver, lo que podría ocultar bugs lógicos reales bajo mensajes de error genéricos para el usuario.
11. **Lack of Documentation:** Ausencia de comentarios JSDoc en las clases de lógica, lo que complica la comprensión del propósito de cada método para nuevos desarrolladores.
12. **Inconsistent Naming:** Mezcla de nomenclaturas en inglés (código) y español (variables de interfaz) que puede confundir en equipos de desarrollo internacionales.
13. **Tight Coupling (Acoplamiento Fuerte):** El solver está diseñado para trabajar específicamente con el formato que React le envía, dificultando su uso en otros entornos (ej. CLI o Backend).
14. **Dead Code (Código Muerto):** Existen pequeñas funciones y variables auxiliares que quedaron de versiones previas y no se están utilizando en la versión final.
15. **Boat Anchor (Ancla de Bote):** El método `getDual()` está definido pero no se utiliza plenamente en la visualización, ocupando espacio y recursos innecesariamente.

---

## 🛠️ Instalación y Uso

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/yoesitooo/SimplexOptimizer.git
   ```
2. Instalar dependencias:
   ```bash
   npm install
   ```
3. Ejecutar en desarrollo:
   ```bash
   npm run dev
   ```

---

## 🎓 Créditos
Desarrollado para la asignatura de **Investigación de Operaciones I**.
**2026**
