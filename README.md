# Sistema de Gestión de Solicitudes - Práctica Guiada

Este repositorio contiene la solución completa para la práctica guiada de **Consumo Avanzado de APIs REST**, implementando la gestión asíncrona de solicitudes, manejo de estados con `useReducer` y cancelación de peticiones mediante `AbortController`.

---

## 🛠️ Tecnologías Utilizadas

- **Backend:** Java 17, Spring Boot, Spring Data JPA, H2 Database.
- **Frontend:** React, Vite, Axios, React Context API.
- **Pruebas de API:** Postman.

---

## ❓ Preguntas de Reflexión y Respuestas Institucionales

### 1. ¿Por qué es importante cancelar peticiones HTTP pendientes cuando un componente de React se desmonta?
Es fundamental para evitar fugas de memoria (*memory leaks*) y errores de estado en componentes desmontados (*state updates on unmounted components*). Si el usuario navega a otra pantalla antes de que responda el servidor, la promesa trataría de actualizar un estado que ya no existe en el DOM, lo que degrada el rendimiento y puede causar comportamientos inesperados en la aplicación.

### 2. ¿Cuál es la diferencia entre usar `useState` y `useReducer` para gestionar el estado de una aplicación?
`useState` es idóneo para estados simples o independientes. `useReducer` es preferible cuando la lógica de estado es compleja, involucra múltiples subvalores o cuando el siguiente estado depende del anterior. `useReducer` centraliza la lógica de actualización en una sola función pura (`reducer`), facilitando el mantenimiento, la escalabilidad y las pruebas unitarias.

### 3. ¿Cómo ayuda el patrón Context API a evitar el "prop drilling" en React?
Permite compartir datos globales (como la lista de solicitudes, estados de carga y errores) a través de la interfaz sin necesidad de pasar *props* manualmente de nivel en nivel por toda la jerarquía de componentes. Cualquier componente dentro del `SolicitudesProvider` puede consumir directamente el contexto usando el hook `useSolicitudes()`.

### 4. ¿Qué ventaja tiene aplicar un filtro de búsqueda en el cliente (frontend) versus solicitarlo al backend?
Filtrar en el cliente ofrece una respuesta instantánea al usuario (latencia cero) sin generar peticiones HTTP adicionales al servidor, ideal para conjuntos de datos pequeños o medianos ya cargados en memoria. Sin embargo, cuando los datos son masivos, el filtrado debe realizarse en el backend mediante paginación y consultas a la base de datos para optimizar el ancho de banda.

### 5. Explique el flujo de datos desde que el usuario realiza una acción en la interfaz hasta que se actualiza la base de datos y se refleja en la vista.
1. **Acción del usuario:** El usuario interactúa con la UI (ej. hace clic en "Resolver").
2. **Custom Hook / Context:** Se llama a la función `resolver(id)` del contexto `SolicitudesContext`.
3. **Servicio HTTP:** `solicitudesApi` realiza una petición `PATCH` o `PUT` vía Axios al endpoint correspondiente del backend.
4. **Backend (Spring Boot):** El controlador recibe la solicitud, el servicio procesa la regla de negocio y el repositorio actualiza el registro en la base de datos H2.
5. **Respuesta y Reducer:** El backend responde con la entidad actualizada (`200 OK`). El cliente recibe la respuesta y despacha la acción `ACTUALIZADA` al `reducer`.
6. **Re-render:** React actualiza el estado central y la interfaz refleja el nuevo estado de la solicitud de forma reactiva.

---

## 📸 Evidencias de Pruebas

Las capturas de pantalla de las pruebas de Postman y la integración de la interfaz web se encuentran organizadas dentro de la carpeta `/Evidencia postman y Api web`.
