# Práctica 04: Automatización de Pruebas de API con Postman/Newman y Bruno

## 🔗 Recursos de la API

- **Base URL**: `https://jsonplaceholder.typicode.com`
- **Endpoint Usuarios**: `https://jsonplaceholder.typicode.com/users`
- **Endpoint Publicaciones**: `https://jsonplaceholder.typicode.com/posts`

---

## 📖 Contexto: Validación de Servicios Externos (CRUD Completo)

Como especialistas en Quality Assurance, debemos asegurar que las APIs que consumimos o desplegamos cumplen con el contrato técnico. En esta práctica validaremos el ciclo de vida completo de los recursos **Usuarios** y **Publicaciones** en **JSONPlaceholder**.

Esta práctica está diseñada para que domines **ambas herramientas**. Te recomiendo:
- Si es tu primera vez: empieza por **Bruno** (más simple, código abierto)
- Si ya conoces Postman: haz ambas opciones para ser experto

---

## 🎯 Objetivos de la Práctica

### Objetivos Comunes (ambas herramientas)
1. Implementar el flujo **CRUD completo** (Create, Read, Update, Patch, Delete)
2. Validar la diferencia entre **PUT** (reemplazo total) y **PATCH** (modificación parcial)
3. Gestionar persistencia simulada mediante variables de entorno
4. **Obtener reportes en consola** durante la ejecución
5. **Generar reportes HTML** al finalizar los tests

### Objetivos Específicos Postman/Newman
- Usar **Newman** desde línea de comandos
- Configurar reporters HTML y JSON
- Ejecutar en contenedores Docker

### Objetivos Específicos Bruno
- Usar **CLI nativa `bru`**
- Configurar reporters HTML y JSON
- Ejecutar en contenedores Docker

---

## 🛠️ Tareas Comunes (CRUD)

### Carpeta A: Gestión de Usuarios (`/users`)

Implementa las siguientes peticiones:

| Método | Endpoint | Descripción | Variable Guardada |
|--------|----------|-------------|-------------------|
| POST | `/users` | Crear usuario | `user_id` |
| GET | `/users` | Listar todos | - |
| GET | `/users/{{user_id}}` | Obtener por ID | - |
| PUT | `/users/{{user_id}}` | Actualizar completo | - |
| PATCH | `/users/{{user_id}}` | Actualizar parcial (phone) | - |
| DELETE | `/users/{{user_id}}` | Eliminar usuario | - |

### Carpeta B: Gestión de Publicaciones (`/posts`)

Implementa las siguientes peticiones:

| Método | Endpoint | Descripción | Variable Guardada |
|--------|----------|-------------|-------------------|
| POST | `/posts` | Crear post | `post_id` |
| GET | `/posts/{{post_id}}` | Obtener por ID | - |
| PUT | `/posts/{{post_id}}` | Actualizar completo | - |
| DELETE | `/posts/{{post_id}}` | Eliminar post | - |

---

## 📋 Requisitos de Implementación

### 1. Validaciones Obligatorias (Tests)

Cada petición debe incluir scripts de test que validen:

1. **Código de Estado**: 200, 201 o 204 según corresponda el método HTTP
2. **Tiempo de respuesta**: Menor a 1000ms
3. **Integridad de Datos**: El ID devuelto debe coincidir con el solicitado
4. **Campos requeridos**: Validar que la respuesta contiene los campos esperados
5. **Variables de entorno**: Guardar `user_id` tras el POST de usuario y `post_id` tras el POST de post

### 2. Diferencia PUT vs PATCH

Demuestra en tus tests la diferencia entre ambos métodos:

- **PUT**: Envía el recurso completo y verifica que todos los campos cambian
- **PATCH**: Envía solo campos parciales y verifica que solo esos campos cambian

### 3. Entorno

Crea un archivo de entorno con las siguientes variables:

```json
{
  "base_url": "https://jsonplaceholder.typicode.com",
  "user_id": "",
  "post_id": ""
}
```

### 4. Reportes

Configura la ejecución para obtener:

- **Reporte en consola**: Salida verbose mostrando cada test passed/failed
- **Reporte HTML**: Archivo generado con formato visual
- **Reporte JSON**: Archivo con datos estructurados para integración CI/CD

---

## 📋 Opción A: Postman + Newman

### Entregables Postman

```
📁 practica-04-postman/
├── 📄 API-Expert-Validation.postman_collection.json
├── 📄 Remote.postman_environment.json
├── 📄 run-tests.sh (o .ps1)
├── 📄 Dockerfile
├── 📄 docker-compose.yml
└── 📄 README.md
```

### Pasos a seguir:

1. **Crear Colección**: `API-Expert-Validation`
2. **Crear Entorno**: `Remote` con las variables definidas
3. **Crear Requests**: Implementa CRUD para Users y Posts
4. **Escribir Tests**: Añade scripts de validación en cada request
5. **Probar en Postman**: Ejecuta manualmente para verificar
6. **Exportar**: Colección (v2.1) y Entorno
7. **Instalar Newman**: `npm install -g newman newman-reporter-html newman-reporter-json`
8. **Ejecutar con Newman**:
   - Ver salida en consola con `--verbose`
   - Generar HTML con `-r html --reporter-html-export`
   - Generar JSON con `-r json --reporter-json-export`
9. **Crear Script**: Automatiza la ejecución con colores y métricas
10. **Dockerizar**: Crea Dockerfile y docker-compose para ejecutar en contenedor

---

## 📋 Opción B: Bruno

### Entregables Bruno

```
📁 practica-04-bruno/
├── 📁 collections/
│   └── 📁 jsonplaceholder/
│       ├── 📁 users/
│       │   ├── 01-create-user.bru
│       │   ├── 02-get-all-users.bru
│       │   ├── 03-get-user-by-id.bru
│       │   ├── 04-put-update-user.bru
│       │   ├── 05-patch-update-user.bru
│       │   └── 06-delete-user.bru
│       └── 📁 posts/
│           ├── 01-create-post.bru
│           ├── 02-get-post-by-id.bru
│           ├── 03-put-update-post.bru
│           └── 04-delete-post.bru
├── 📁 environments/
│   └── remote.bru
├── 📁 reports/
├── 📄 run-tests.sh (o .ps1)
├── 📄 Dockerfile
├── 📄 docker-compose.yml
└── 📄 README.md
```

### Pasos a seguir:

1. **Instalar Bruno CLI**: `npm install -g @usebruno/cli @usebruno/reporter-html @usebruno/reporter-json`
2. **Crear Estructura**: Organiza carpetas de users y posts
3. **Crear Archivos .bru**: Implementa cada request con sintaxis Bruno
4. **Escribir Tests**: Usa `bru.expectStatus()`, `bru.expect()`, `vars.set()`
5. **Probar en Bruno GUI**: Ejecuta desde la interfaz para verificar
6. **Ejecutar con CLI**:
   - Ver salida en consola con `--verbose`
   - Generar HTML con `--reporter html --output`
   - Generar JSON con `--reporter json --output`
7. **Crear Script**: Automatiza la ejecución
8. **Dockerizar**: Crea Dockerfile y docker-compose

---

## 🐳 Opción C: Docker (Ambas Herramientas)

Crea la infraestructura Docker para ejecutar los tests en contenedores aislados, generando los reportes en el host.

### Para Postman/Newman:

```dockerfile
FROM node:20-alpine
RUN npm install -g newman newman-reporter-html newman-reporter-json
WORKDIR /etc/newman
COPY collections/ ./collections/
COPY environments/ ./environments/
RUN mkdir -p reports
```

### Para Bruno:

```dockerfile
FROM node:20-alpine
RUN npm install -g @usebruno/cli @usebruno/reporter-html @usebruno/reporter-json
WORKDIR /etc/bruno
COPY collections/ ./collections/
COPY environments/ ./environments/
RUN mkdir -p reports
```

---

## 📦 Entregables

### Para Postman + Newman:

```
📁 practica-04-postman/
├── 📄 API-Expert-Validation.postman_collection.json
├── 📄 Remote.postman_environment.json
├── 📄 Dockerfile
├── 📄 docker-compose.yml
├── 📄 run-tests.sh
└── 📄 README.md
```

### Para Bruno:

```
📁 practica-04-bruno/
├── 📁 collections/
│   └── [archivos .bru organizados]
├── 📁 environments/
│   └── remote.bru
├── 📁 reports/
├── 📄 Dockerfile
├── 📄 docker-compose.yml
├── 📄 run-tests.sh
└── 📄 README.md
```

### Para Ambos (Experto):

```
📁 practica-04-apis/
├── 📁 postman/
│   └── [archivos de arriba]
├── 📁 bruno/
│   └── [archivos de arriba]
└── 📄 COMPARACION.md
```

---

## 🎯 Criterios de Evaluación

| Criterio | Puntos | Descripción |
|----------|--------|-------------|
| **CRUD Completo** | 4p | Todos los verbos HTTP implementados (POST, GET, PUT, PATCH, DELETE) |
| **Lógica de Variables** | 2p | IDs se pasan correctamente entre peticiones |
| **Scripts de Validación** | 2p | Tests robustos con aserciones de contenido y timing |
| **Reportes en Consola** | 1p | Salida verbose visible durante ejecución |
| **Reportes HTML** | 1p | Archivo HTML generado correctamente |
| **Docker (opcional)** | 2p | Contenedor funcional con reportes visibles |

### Criterios Adicionales para Doble Versión (Experto)

Realiza la práctica en **ambas herramientas** y entrega un archivo `COMPARACION.md`:

- **+2 puntos** por implementar ambas herramientas correctamente
- **+2 puntos** por incluir **COMPARACION.md** detallando:
  - Diferencias de sintaxis entre ambas herramientas
  - Ventajas y desventajas de cada una
  - Cuándo usarías una u otra
  - Similitudes en estructura de tests
- **+2 puntos** por **documentación profesional** en README.md con instrucciones claras

---

## 💡 Diferencias Clave: PUT vs PATCH

| Aspecto | PUT | PATCH |
|---------|-----|-------|
| **Alcance** | Reemplazo total | Modificación parcial |
| **Body** | Recurso completo | Solo campos a modificar |
| **Ejemplo JSONPlaceholder** | `{ "id": 1, "name": "New", "username": "new", "email": "new@test.com" }` | `{ "phone": "123" }` |
| **Uso típico** | Actualización completa | Quick fixes, parches |

**Demuéstralo en tus tests**: El PUT debe validar que todos los campos cambian, el PATCH solo los que envías.

---

## 📚 Recursos Adicionales

### Documentación Oficial

- **Postman**: https://learning.postman.com/docs/introduction/overview/
- **Newman**: https://github.com/postmanlabs/newman
- **Bruno**: https://docs.usebruno.com/
- **Bruno CLI**: https://github.com/usebruno/cli

### APIs de Ejemplo

- **JSONPlaceholder**: https://jsonplaceholder.typicode.com/
- **Countries GraphQL**: https://countries.trevorblades.com/
- **SpaceX GraphQL**: https://api.spacex.land/graphql/

### Herramientas

- **Newman HTML Reporter**: `npm install -g newman-reporter-html`
- **Bruno Reporters**: `npm install -g @usebruno/reporter-html @usebruno/reporter-json`

---

## 📋 Instrucciones de Entrega

1. **Crea la estructura** de carpetas según la opción elegida
2. **Implementa los requests** con sus correspondientes tests
3. **Ejecuta y verifica** que todos los tests pasan
4. **Genera los reportes** HTML y JSON
5. **Documenta** en README.md cómo ejecutar los tests
6. **Opcional**: Crea script automatizado y Dockerfile
7. **Comprime** toda la carpeta en un ZIP
8. **Entrega** en el LMS antes de la fecha límite

---

> **Nota del Profesor**: Esta práctica está diseñada para que domines ambas herramientas. Si solo haces una opción, aprobarás. Si haces ambas con comparativa detallada, demostrarás nivel de experto y recibirás bonus de puntos.
