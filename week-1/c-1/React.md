# Clase: Introducción a React

---

## Agenda

1. ¿Qué es React?
2. ¿Qué tan usado es React?
3. ¿Cómo inyectar JavaScript en HTML?
4. ¿Qué es un Componente?
5. Virtual DOM
6. Reconciliación
7. Props — Comunicación entre componentes
8. Hooks
9. Temas adicionales
10. Demostración de código

---

## 1. ¿Qué es React?

React es una **biblioteca de JavaScript** creada por **Meta (Facebook)** en 2013, diseñada para construir interfaces de usuario (UI) de forma declarativa, eficiente y basada en componentes reutilizables.

> "React te permite construir UIs complejas a partir de piezas pequeñas e independientes llamadas componentes."

### Características clave:
- **Declarativo:** Describes *qué* quieres mostrar, no *cómo* hacerlo paso a paso.
- **Basado en componentes:** La UI se divide en piezas reutilizables.
- **Unidireccional:** El flujo de datos va de padre a hijo (one-way data flow).
- **Ecosistema enorme:** Tiene librerías para routing, estado global, fetching, etc.

---

## 2. ¿Qué tan usado es React?

React es la **biblioteca frontend más utilizada en el mundo** actualmente.

| Métrica | Dato |
|---|---|
| Descargado por npm | +24 millones de veces por semana |
| GitHub Stars | +220,000 ⭐ |
| Empresas que lo usan | Meta, Netflix, Airbnb, Uber, Twitter, LinkedIn |
| Ofertas de empleo | Domina el mercado frontend global |

### Frameworks que se construyen sobre React:
- **Next.js** — SSR, SSG, App Router
- **Remix** — Full-stack con enfoque en web fundamentals
- **Expo / React Native** — Apps móviles con React

> Aprender React hoy = abrir muchas puertas en el mercado laboral.

---

## 3. ¿Cómo inyectar JavaScript en HTML?

Antes de React, la forma de usar JavaScript en el navegador era incluyéndolo en el HTML con la etiqueta `<script>`.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>JS en HTML</title>
  </head>
  <body>
    <h1 id="titulo">Hola mundo</h1>

    <script>
      // JavaScript puro manipulando el DOM
      const titulo = document.getElementById("titulo");
      titulo.innerText = "¡Hola desde JavaScript!";
      titulo.style.color = "blue";
    </script>
  </body>
</html>
```

### El problema con este enfoque (DOM imperativo):
- Debes indicar *cómo* cambiar cada elemento manualmente.
- Difícil de mantener cuando la app crece.
- Propenso a errores con múltiples archivos JS.

React resuelve esto con un modelo **declarativo** usando JSX.

---

## 4. ¿Qué es un Componente?

Un componente es una **función de JavaScript que retorna JSX** (una sintaxis parecida a HTML dentro de JS).

```jsx
// Componente funcional básico
function Saludo() {
  return <h1>¡Hola, estudiantes de RIWI!</h1>;
}
```

### Reglas de los componentes:
- El nombre **debe empezar con mayúscula** (`Saludo`, no `saludo`).
- Siempre retorna **un solo elemento raíz** (o un Fragment `<> </>`).
- Son **reutilizables**: puedes usarlo múltiples veces en la app.

```jsx
function App() {
  return (
    <>
      <Saludo />
      <Saludo />
      <Saludo />
    </>
  );
}
```

### Tipos de componentes:
| Tipo | Descripción |
|---|---|
| **Funcional** | Función JS que retorna JSX — el estándar hoy |
| **De clase** | Basado en `class`, obsoleto desde React Hooks (2019) |

---

## 5. Virtual DOM

El **Virtual DOM (VDOM)** es una representación ligera en memoria del DOM real del navegador.

### ¿Cómo funciona?

```
Estado cambia
     ↓
React genera un nuevo Virtual DOM
     ↓
Compara el nuevo VDOM con el anterior (diffing)
     ↓
Calcula los cambios mínimos necesarios
     ↓
Aplica solo esos cambios al DOM real
```

### ¿Por qué es importante?
Manipular el DOM real es **costoso en rendimiento**. El Virtual DOM permite que React calcule el mínimo número de cambios necesarios antes de tocar el navegador.

```
DOM Real       → Lento de actualizar
Virtual DOM    → Rápido, vive en memoria
Reconciliación → Algoritmo que los sincroniza
```

---

## 6. Reconciliación

La **reconciliación** es el proceso por el cual React compara el Virtual DOM anterior con el nuevo y decide qué partes del DOM real necesita actualizar.

### Algoritmo de React (Fiber):
- React asigna una **"key"** a cada elemento para identificarlo.
- Si el tipo de elemento cambia, React destruye el nodo y crea uno nuevo.
- Si el tipo es igual, React actualiza solo los atributos que cambiaron.

```jsx
// ❌ Sin key — React no puede optimizar bien listas
function ListaMala({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li>{item}</li>
      ))}
    </ul>
  );
}

// ✅ Con key — React sabe qué elemento es cada uno
function ListaBuena({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.nombre}</li>
      ))}
    </ul>
  );
}
```

> La prop `key` es fundamental en listas para ayudar al algoritmo de reconciliación.

---

## 7. Props — Comunicación entre Componentes

Las **props** (propiedades) son la forma en que los componentes se comunican. Un componente **padre** puede enviar datos a un componente **hijo** a través de props.

> Las props fluyen **de padre a hijo** — son de solo lectura dentro del hijo.

### Sintaxis básica:

```jsx
// Componente hijo que recibe props
function TarjetaEstudiante({ nombre, lenguaje, nivel }) {
  return (
    <div className="tarjeta">
      <h2>{nombre}</h2>
      <p>Aprendiendo: {lenguaje}</p>
      <p>Nivel: {nivel}</p>
    </div>
  );
}

// Componente padre que envía props
function App() {
  return (
    <div>
      <TarjetaEstudiante nombre="Camila" lenguaje="React" nivel="Principiante" />
      <TarjetaEstudiante nombre="Andrés" lenguaje="Next.js" nivel="Intermedio" />
      <TarjetaEstudiante nombre="Valentina" lenguaje="TypeScript" nivel="Avanzado" />
    </div>
  );
}
```

### Props con valores dinámicos:

```jsx
function Boton({ texto, color, onClick }) {
  return (
    <button style={{ backgroundColor: color }} onClick={onClick}>
      {texto}
    </button>
  );
}

function App() {
  const manejarClick = () => alert("¡Botón presionado!");

  return (
    <Boton texto="Guardar" color="green" onClick={manejarClick} />
  );
}
```

### Props especiales — `children`:

```jsx
// children es todo lo que va entre las etiquetas del componente
function Tarjeta({ children }) {
  return <div className="tarjeta">{children}</div>;
}

function App() {
  return (
    <Tarjeta>
      <h2>Título desde el padre</h2>
      <p>Contenido personalizado</p>
    </Tarjeta>
  );
}
```

### Reglas de las props:
| Regla | Descripción |
|---|---|
| Solo lectura | El hijo **no puede modificar** las props que recibe |
| Cualquier tipo | Pueden ser strings, números, arrays, objetos, funciones |
| Desestructuración | Es buena práctica desestructurarlas en los parámetros |
| Default values | Puedes definir valores por defecto con `= valor` |

```jsx
// Props con valores por defecto
function Saludo({ nombre = "Estudiante", rol = "Visitante" }) {
  return <p>Hola, {nombre} — {rol}</p>;
}
```

---

## 8. Hooks

Los **Hooks** son funciones especiales de React que permiten usar características de React (estado, ciclo de vida, contexto, etc.) dentro de componentes funcionales.

> Introducidos en React 16.8 (2019). Cambiaron completamente la forma de escribir React.

### Reglas de los Hooks:
- Solo se llaman en el **nivel superior** del componente (no dentro de `if`, loops, etc.).
- Solo se llaman dentro de **componentes funcionales** o **custom hooks**.

---

### `useState` — Estado local

```jsx
import { useState } from "react";

function Contador() {
  const [contador, setContador] = useState(0); // [valor, función para cambiarlo]

  return (
    <div>
      <p>Clicks: {contador}</p>
      <button onClick={() => setContador(contador + 1)}>Incrementar</button>
      <button onClick={() => setContador(0)}>Resetear</button>
    </div>
  );
}
```

---

### `useEffect` — Efectos secundarios

```jsx
import { useState, useEffect } from "react";

function RelojEnVivo() {
  const [hora, setHora] = useState(new Date().toLocaleTimeString());

  useEffect(() => {
    const intervalo = setInterval(() => {
      setHora(new Date().toLocaleTimeString());
    }, 1000);

    // Cleanup: limpia el intervalo cuando el componente se desmonta
    return () => clearInterval(intervalo);
  }, []); // [] = solo se ejecuta una vez al montar

  return <p>Hora actual: {hora}</p>;
}
```

### Array de dependencias en `useEffect`:
```jsx
useEffect(() => { /* se ejecuta en cada render */ });
useEffect(() => { /* se ejecuta solo al montar */ }, []);
useEffect(() => { /* se ejecuta cuando cambia 'valor' */ }, [valor]);
```

---

### Otros Hooks importantes:

| Hook | Para qué sirve |
|---|---|
| `useRef` | Referenciar elementos del DOM sin re-renderizar |
| `useContext` | Consumir un contexto global |
| `useMemo` | Memorizar cálculos costosos |
| `useCallback` | Memorizar funciones para evitar re-renders |
| `useReducer` | Manejo de estado complejo (alternativa a useState) |

---

## 9. Temas Adicionales Importantes

### JSX — JavaScript XML

JSX no es HTML. Es azúcar sintáctica que se convierte en llamadas a `React.createElement()`.

```jsx
// Esto que escribes (JSX):
const elemento = <h1 className="titulo">Hola</h1>;

// Es equivalente a esto (JS puro):
const elemento = React.createElement("h1", { className: "titulo" }, "Hola");
```

**Diferencias JSX vs HTML:**
| HTML | JSX |
|---|---|
| `class` | `className` |
| `for` | `htmlFor` |
| `onclick` | `onClick` |
| Atributos en minúsculas | camelCase para eventos |

---

### Renderizado Condicional

```jsx
function Mensaje({ estaLogueado }) {
  return (
    <div>
      {estaLogueado ? (
        <p>Bienvenido de vuelta 👋</p>
      ) : (
        <p>Por favor inicia sesión</p>
      )}
    </div>
  );
}

// Usando && (renderiza solo si la condición es true)
function Notificacion({ cantidad }) {
  return (
    <div>
      {cantidad > 0 && <span>Tienes {cantidad} notificaciones</span>}
    </div>
  );
}
```

---

### Renderizado de Listas

```jsx
function ListaCursos() {
  const cursos = [
    { id: 1, nombre: "React", duracion: "4 semanas" },
    { id: 2, nombre: "Next.js", duracion: "3 semanas" },
    { id: 3, nombre: "TypeScript", duracion: "2 semanas" },
  ];

  return (
    <ul>
      {cursos.map((curso) => (
        <li key={curso.id}>
          <strong>{curso.nombre}</strong> — {curso.duracion}
        </li>
      ))}
    </ul>
  );
}
```

---

### Manejo de Eventos

```jsx
function Formulario() {
  const manejarSubmit = (evento) => {
    evento.preventDefault(); // Previene recarga de página
    console.log("Formulario enviado");
  };

  const manejarCambio = (evento) => {
    console.log("Valor:", evento.target.value);
  };

  return (
    <form onSubmit={manejarSubmit}>
      <input type="text" onChange={manejarCambio} placeholder="Tu nombre" />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

---

## 10. Demostración — Componente Completo

Un componente real que combina todo lo visto: estado, props, eventos, listas y renderizado condicional.

```jsx
import { useState } from "react";

// ----- Componente hijo: Tarjeta de tarea -----
function TareaTarjeta({ tarea, onCompletar, onEliminar }) {
  return (
    <div
      style={{
        padding: "12px",
        marginBottom: "8px",
        borderRadius: "8px",
        border: "1px solid #ddd",
        backgroundColor: tarea.completada ? "#e8f5e9" : "#fff",
        display: "flex",
        justifyContent: "space-between",
        alignItems: "center",
      }}
    >
      <span
        style={{
          textDecoration: tarea.completada ? "line-through" : "none",
          color: tarea.completada ? "#888" : "#000",
        }}
      >
        {tarea.texto}
      </span>
      <div>
        <button onClick={() => onCompletar(tarea.id)} style={{ marginRight: "8px" }}>
          {tarea.completada ? "↩ Deshacer" : "✓ Completar"}
        </button>
        <button onClick={() => onEliminar(tarea.id)} style={{ color: "red" }}>
          🗑 Eliminar
        </button>
      </div>
    </div>
  );
}

// ----- Componente padre: Lista de tareas -----
function ListaDeTareas() {
  const [tareas, setTareas] = useState([
    { id: 1, texto: "Aprender qué es React", completada: true },
    { id: 2, texto: "Entender el Virtual DOM", completada: false },
    { id: 3, texto: "Practicar con useState", completada: false },
  ]);
  const [nuevaTarea, setNuevaTarea] = useState("");

  const agregarTarea = () => {
    if (!nuevaTarea.trim()) return;
    const tarea = {
      id: Date.now(),
      texto: nuevaTarea,
      completada: false,
    };
    setTareas([...tareas, tarea]);
    setNuevaTarea("");
  };

  const completarTarea = (id) => {
    setTareas(
      tareas.map((t) =>
        t.id === id ? { ...t, completada: !t.completada } : t
      )
    );
  };

  const eliminarTarea = (id) => {
    setTareas(tareas.filter((t) => t.id !== id));
  };

  const tareasPendientes = tareas.filter((t) => !t.completada).length;

  return (
    <div style={{ maxWidth: "500px", margin: "40px auto", fontFamily: "sans-serif" }}>
      <h1>Lista de Tareas React</h1>
      <p>
        {tareasPendientes === 0
          ? "¡Todas las tareas completadas! 🎉"
          : `Tienes ${tareasPendientes} tarea(s) pendiente(s)`}
      </p>

      {/* Input para agregar tareas */}
      <div style={{ display: "flex", gap: "8px", marginBottom: "16px" }}>
        <input
          type="text"
          value={nuevaTarea}
          onChange={(e) => setNuevaTarea(e.target.value)}
          onKeyDown={(e) => e.key === "Enter" && agregarTarea()}
          placeholder="Nueva tarea..."
          style={{ flex: 1, padding: "8px" }}
        />
        <button onClick={agregarTarea}>Agregar</button>
      </div>

      {/* Lista de tareas */}
      {tareas.length === 0 ? (
        <p style={{ color: "#999" }}>No hay tareas. ¡Agrega una!</p>
      ) : (
        tareas.map((tarea) => (
          <TareaTarjeta
            key={tarea.id}
            tarea={tarea}
            onCompletar={completarTarea}
            onEliminar={eliminarTarea}
          />
        ))
      )}
    </div>
  );
}

export default ListaDeTareas;
```

### ¿Qué conceptos aplica este componente?

| Concepto | Dónde se aplica |
|---|---|
| **Componentes** | `TareaTarjeta` y `ListaDeTareas` |
| **Props** | `tarea`, `onCompletar`, `onEliminar` |
| **useState** | Estado de tareas y del input |
| **Eventos** | `onClick`, `onChange`, `onKeyDown` |
| **Renderizado de listas** | `.map()` con `key` |
| **Renderizado condicional** | Mensaje cuando no hay tareas / tareas completadas |
| **Comunicación padre→hijo** | Props de datos |
| **Comunicación hijo→padre** | Props de funciones (callbacks) |

---

## Resumen de la clase

```
React
├── Biblioteca JS para UIs
├── Componentes (funciones que retornan JSX)
│   ├── Props → reciben datos del padre
│   └── State → datos internos del componente
├── Virtual DOM → copia en memoria del DOM real
├── Reconciliación → algoritmo de actualización eficiente
├── Hooks
│   ├── useState → estado local
│   ├── useEffect → efectos secundarios
│   └── ...más hooks
└── JSX → HTML + JS combinados
```

---

## Recursos para seguir aprendiendo

- [react.dev](https://react.dev) — Documentación oficial (muy buena)
- [javascript.info](https://javascript.info) — Reforzar bases de JS
- [roadmap.sh/react](https://roadmap.sh/react) — Ruta de aprendizaje

---

> **Tarea:** Crea un componente `TarjetaPerfil` que reciba por props `nombre`, `edad` y `tecnologia`, y renderiza esa información en pantalla con un estilo básico.
