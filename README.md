# 📚 Sociología UGB — Manual de Usuario

> Plataforma web de contenido y exámenes en línea para el curso **Sociología para Ingeniería en Sistemas**.
> Este documento explica, en lenguaje sencillo, qué es cada parte del sitio y cómo usar el **panel docente**.

---

## 🧭 Índice

1. [¿Qué es esta plataforma?](#1--qué-es-esta-plataforma)
2. [Mapa general del sitio](#2--mapa-general-del-sitio)
3. [Área pública (lo que ve el estudiante)](#3--área-pública-lo-que-ve-el-estudiante)
4. [Cómo entrar al área docente](#4--cómo-entrar-al-área-docente)
5. [Cuenta de administrador por defecto](#5--cuenta-de-administrador-por-defecto)
6. [Recorrido por el panel docente](#6--recorrido-por-el-panel-docente)
7. [Aprobar cuentas (estudiantes y docentes)](#7--aprobar-cuentas-estudiantes-y-docentes)
8. [Base de datos: inicializar y limpiar duplicados](#8--base-de-datos-inicializar-y-limpiar-duplicados)
9. [Tipos de pregunta soportados](#9--tipos-de-pregunta-soportados)
10. [Cómo armar un examen](#10--cómo-armar-un-examen)
11. [🔒 Seguridad — leer antes de usar en producción](#11--seguridad--leer-antes-de-usar-en-producción)
12. [Preguntas frecuentes](#12--preguntas-frecuentes)

---

## 1. 📖 ¿Qué es esta plataforma?

Es un **sitio de una sola página** (`index.html`) que funciona como:

- Un **libro de texto interactivo** de Sociología, organizado en 5 módulos + material semanal.
- Un **sistema de exámenes en línea** con banco de preguntas, corrección automática y resultados.
- Un **panel de administración** donde el docente controla todo el contenido, sin tocar código.

Toda la información (semanas, autores, preguntas, exámenes, estudiantes, resultados, cuentas docentes) se guarda en **Firebase Firestore**, una base de datos en la nube. No hay que instalar nada: basta con abrir `index.html` en un navegador con conexión a internet.

---

## 2. 🗺️ Mapa general del sitio

El sitio tiene **dos áreas completamente separadas**:

| Área | ¿Quién la usa? | ¿Cómo se entra? |
|---|---|---|
| 🎓 **Área pública** | Estudiantes (y cualquier visitante) | Directamente, al abrir el sitio |
| 🔐 **Panel docente** | El/los docente(s) del curso | Botón **"Área docente"** → código de acceso → iniciar sesión |

```
index.html
 ├── 🎓 Vista pública ─── Módulos 1-5, Material semanal, Exámenes disponibles
 └── 🔐 Panel docente  ─── Dashboard, Estudiantes, Semanas, Material, Autores,
                            Banco de preguntas, Exámenes, Resultados,
                            Estadísticas, Docentes, Configuración
```

---

## 3. 🎓 Área pública (lo que ve el estudiante)

| Módulo / sección | Contenido | Interacción |
|---|---|---|
| **Módulo 1** — Ciencia y Ciencias Sociales | Delimitación del conocimiento científico y ubicación de las Ciencias Sociales | Lectura |
| **Módulo 2** — Conceptualización de la Sociología | Definición, objeto de estudio, campo de acción | Lectura |
| **Módulo 3** — Desarrollo Histórico | Línea de tiempo del surgimiento de la Sociología | 🖱️ Clic en cada hito → **ventana emergente** con el detalle |
| **Módulo 4** — Precursores y Exponentes | Tarjetas de autores (Durkheim, Weber, Marx, etc.) | 🖱️ Clic en cada tarjeta → **ventana emergente** con biografía, aporte y teoría |
| **Módulo 5** — Corrientes y Teorías | Acordeón con las principales corrientes sociológicas | 🖱️ Clic para desplegar cada corriente |
| **Material semanal** | Tarjetas por semana (1 a 5, o las que el docente cree) | 🖱️ Clic en una semana → descripción completa + lista de material + acceso directo al examen de esa semana |
| **Exámenes disponibles** | Lista de exámenes activos | Botón **"Tomar examen"** → el estudiante se identifica con nombre/correo y responde |

> 💡 Todo el contenido de "Autores", "Semanas", "Material" y "Exámenes" que ve el estudiante **lo administra el docente** desde el panel — nada de eso se edita en el código.

---

## 4. 🔑 Cómo entrar al área docente

El acceso está protegido en **dos pasos**:

| Paso | Qué hacer | Dato necesario |
|---|---|---|
| 1️⃣ | Clic en el botón **"🔑 Área docente"** (arriba del sitio) | **Código rápido: `747`** |
| 2️⃣ | Elegir **"Iniciar sesión"** (si ya tienes cuenta) o **"Crear cuenta"** (si eres nuevo) | Correo y contraseña |

> ⚠️ El código `747` solo abre la puerta al formulario de inicio de sesión / registro — **no es la contraseña de ninguna cuenta**. Compártelo únicamente con quien deba tener acceso al panel.

Si un docente nuevo se registra con **"Crear cuenta"**, además debe ingresar el **"Código de acceso docente"** (uno distinto, generado automáticamente y visible/regenerable en `Configuración → Base de datos`). Su cuenta queda en estado **Pendiente** hasta que alguien con acceso al panel la apruebe (ver [sección 7](#7--aprobar-cuentas-estudiantes-y-docentes)).

---

## 5. 👤 Cuenta de administrador por defecto

La primera vez que se usa el botón **"Inicializar base de datos"** (en `Configuración → 🗄️ Base de datos`), el sistema crea automáticamente una cuenta docente lista para usar:

| Campo | Valor |
|---|---|
| **Correo / usuario** | `ADMIN` |
| **Contraseña** | `ADMIN` |
| **Estado** | Activo desde el primer momento |

> 🚨 **Muy importante:** esta cuenta se crea **una sola vez**, solo si la colección de docentes está completamente vacía (por ejemplo, la primerísima vez que se configura el proyecto). Si ya existe al menos un docente registrado, no se vuelve a crear ni se sobrescribe nada.
>
> 🔒 **Cambia esta contraseña cuanto antes** desde tu propia cuenta, o crea tu cuenta personal y elimina/desactiva la cuenta `ADMIN` desde la sección **Docentes** del panel. Dejar activa una cuenta con usuario y contraseña `ADMIN`/`ADMIN` es un riesgo de seguridad si el sitio es público.

---

## 6. 🧩 Recorrido por el panel docente

Una vez dentro, el menú lateral tiene estas secciones:

| Sección | ¿Para qué sirve? |
|---|---|
| 📊 **Dashboard** | Resumen general: total de estudiantes, activos, pendientes, docentes pendientes, preguntas, exámenes, promedio general y actividad reciente. Las tarjetas de "pendientes" son clicables y te llevan directo a esa sección. |
| 🧑‍🎓 **Estudiantes** | Lista de estudiantes registrados: buscar, filtrar por estado, agregar manualmente, **aprobar** cuentas pendientes, editar o eliminar. |
| 📅 **Semanas** | Crear/editar/eliminar las semanas del curso (número, título, descripción) — son las que ve el estudiante en "Material semanal". |
| 📖 **Material** | Contenido educativo (lecturas, videos, recursos) asociado a cada semana. |
| 🧑‍🏫 **Autores** | Perfiles de los pensadores/exponentes (biografía, aporte, teoría, foto) que se muestran en el Módulo 4. |
| ❓ **Banco de preguntas** | Todas las preguntas de evaluación, de los 10 tipos disponibles (ver [sección 9](#9--tipos-de-pregunta-soportados)). |
| 📝 **Exámenes** | Crear exámenes (Semanal, Final, Mixto, Pre-examen) y armarlos seleccionando preguntas del banco. |
| 📈 **Resultados** | Historial de intentos de cada estudiante: puntuación, correctas, incorrectas, fecha y detalle pregunta por pregunta. Permite corregir una calificación manualmente. |
| 📊 **Estadísticas** | Promedio por semana, preguntas más falladas y distribución de resultados (excelente/bueno/regular/bajo). |
| 👥 **Docentes** | Aprobar, rechazar, desactivar o eliminar cuentas de acceso al panel docente. |
| ⚙️ **Configuración** | Nombre del curso, año, tiempo y número de intentos por defecto, nota mínima, código de acceso docente, validación de estudiantes, **base de datos** (inicializar / limpiar duplicados / aprobar solicitudes docentes) y el botón de este **manual**. |

---

## 7. ✅ Aprobar cuentas (estudiantes y docentes)

Tanto los **estudiantes** como los **docentes nuevos** quedan en estado **"Pendiente"** hasta que alguien los aprueba. Hay dos formas de hacerlo:

### Uno por uno
| Tipo de cuenta | Dónde | Botón |
|---|---|---|
| Estudiante | Sección **Estudiantes** | ✅ Aprobar (junto a cada fila pendiente) |
| Docente | Sección **Docentes** | ✅ Aprobar / ❌ Rechazar (junto a cada fila) |

### Todos a la vez (solo docentes)
En **Configuración → 🗄️ Base de datos**, el botón **"Aceptar todas las solicitudes docentes"** aprueba de un solo clic **todas** las cuentas docentes que estén pendientes en ese momento. Útil si se acumularon varias solicitudes.

> ⚠️ Este botón aprueba **todas** las pendientes sin distinción — úsalo solo cuando confíes en que todas las solicitudes acumuladas son legítimas. Para revisar caso por caso, usa la sección **Docentes**.

---

## 8. 🗄️ Base de datos: inicializar y limpiar duplicados

En **Configuración → 🗄️ Base de datos** hay tres botones:

| Botón | Qué hace | Cuándo usarlo |
|---|---|---|
| 🗄️ **Inicializar base de datos** | Crea la configuración inicial, la cuenta `ADMIN`/`ADMIN` (solo la primera vez) y verifica que todas las colecciones existan. No borra ni duplica nada. | Al configurar el proyecto por primera vez, o si algo parece faltar. |
| 🧹 **Limpiar duplicados** | Busca semanas, autores, material, preguntas, exámenes y estudiantes **repetidos** (por número, nombre, título o correo), conserva la versión más completa de cada uno y elimina las copias. Actualiza automáticamente las referencias (exámenes, material, resultados) para que nada quede roto. | Si importaste el mismo archivo dos veces o notas contenido repetido. |
| 👥 **Aceptar todas las solicitudes docentes** | Aprueba de golpe todas las cuentas docentes pendientes. | Ver [sección 7](#7--aprobar-cuentas-estudiantes-y-docentes). |

Junto a estos botones también está el acceso a este mismo manual: **📖 Manual de usuario**.

---

## 9. 🧠 Tipos de pregunta soportados

El banco de preguntas admite 10 tipos distintos:

| Tipo | Descripción |
|---|---|
| `VERDADERO_FALSO` | Pregunta de verdadero o falso. |
| `SELECCION_MULTIPLE` | Una sola respuesta correcta entre varias opciones. |
| `SELECCION_MULTIPLE_VARIAS` | Varias respuestas correctas a la vez. |
| `COMPLETAR` | El estudiante completa una palabra o frase faltante. |
| `RESPUESTA_CORTA` | Respuesta breve en texto libre (se compara contra la respuesta esperada). |
| `RELACIONAR` | Relacionar elementos de una columna con su pareja correspondiente. |
| `ORDENAR` | Ordenar una lista de elementos en la secuencia correcta. |
| `ARRASTRAR_SOLTAR` | Igual que ordenar, pero presentado como arrastrar y soltar. |
| `IDENTIFICAR_AUTOR` | Identificar qué autor propuso un concepto o idea. |
| `CASO_PRACTICO` | Analizar un caso y elegir el concepto sociológico que mejor lo explica. |

---

## 10. 📝 Cómo armar un examen

1. Ve a **Exámenes → Nuevo examen**.
2. Elige el **tipo**: `Pre-examen`, `Semanal`, `Mixto` o `Final`.
3. (Opcional) Asócialo a una **semana** específica.
4. Define **tiempo límite**, **intentos permitidos** y **nota mínima** de aprobación.
5. Guarda y abre el **Constructor de examen** para seleccionar, del banco de preguntas, cuáles formarán parte de ese examen.
6. Actívalo — solo los exámenes en estado **Activo** aparecen para los estudiantes en la vista pública.

---

## 11. 🔒 Seguridad — leer antes de usar en producción

- El panel docente **no usa Firebase Authentication todavía**: la protección es solo el código `747` + el login por contraseña dentro de la interfaz. Quien tenga acceso directo a la configuración de Firebase del sitio (visible en el propio código fuente) podría, en teoría, escribir directamente en la base de datos sin pasar por esa contraseña.
- Por eso es fundamental:
  - Cambiar la contraseña de la cuenta `ADMIN` apenas se configure el sitio.
  - No compartir el código `747` ni el "Código de acceso docente" fuera del equipo docente.
  - Aprobar cuentas docentes solo si reconoces la solicitud.
- Para un entorno de producción real (no solo un proyecto académico), lo recomendable es activar **Firebase Authentication** para el panel docente. Esta versión no lo implementa, pero queda como siguiente paso sugerido.

---

## 12. ❓ Preguntas frecuentes

**¿Qué hago si olvidé la contraseña de mi cuenta docente?**
Pide a otro docente con acceso que te elimine desde **Docentes** y regístrate de nuevo, o que edite tu contraseña directamente desde la consola de Firebase (Firestore → colección `docentes`).

**¿Por qué un estudiante/docente no puede iniciar sesión?**
Revisa que su cuenta esté en estado **Activo** (no "Pendiente" ni "Denegado") en las secciones **Estudiantes** o **Docentes**.

**Importé un archivo de datos y ahora veo contenido repetido, ¿qué hago?**
Ve a **Configuración → 🗄️ Base de datos** y usa **"Limpiar duplicados"**.

**¿Dónde cambio el nombre del curso, el año o la nota mínima?**
En **Configuración → ⚙️ Configuración general / Configuración de exámenes**.

**¿Puedo tener más de un docente?**
Sí. Cada nuevo docente se registra con **"Crear cuenta"** (necesita el código `747` y el "Código de acceso docente"), y alguien con acceso al panel debe aprobar su cuenta desde **Docentes**.

---

<p align="center">Sociología UGB · Proyecto académico</p>
