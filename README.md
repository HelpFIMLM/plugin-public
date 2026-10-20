# Plugin Help

Extensión de Chrome para revisar estudiantes de **Campus** frente a **Mi Nube**, y para generar informes de ingreso/conexión a Campus.

Versión actual: **1.9.1**

---

## Qué hace

La extensión tiene tres pestañas principales:

### 1. Validar Mi Nube

Genera un informe Excel para ver, curso por curso:

- si el estudiante está registrado en Mi Nube
- si tiene información y cursos cargados (según la página de perfil)

Flujo:

1. Toma los cursos que elegiste en Configuración.
2. Consulta los estudiantes de cada curso en Campus.
3. Usa el documento (`sis_user_id`) para consultar Mi Nube (1 petición cada 3 segundos).
4. Arma un Excel con: nombre, identificación, correo y estado Mi Nube (`EXISTE`, `SIN BENEFICIOS`, `NO EXISTE`, `ERROR`).

**Importante:** mientras corre, deja abierta una pestaña de Mi Nube (cualquier página del mismo sitio, por ejemplo el perfil).

### 2. Validar Ingreso

Genera un informe Excel de conexión a Campus, una hoja por curso:

- si el estudiante se ha conectado
- tiempo de actividad (minutos y horas)
- última conexión
- hace cuánto se conectó (respecto a la fecha actual)

### 3. Configuración

Tiene dos secciones:

| Sección | Uso |
|--------|-----|
| **Gestión de cursos** | Carga los cursos de Campus, marca los que quieras y guarda la selección. Esa lista se usa en Mi Nube y en Ingreso. |
| **Acceso Campus** | Guarda el ID / Access Token de Canvas (solo en este navegador). |

También puedes:

- **Cancelar** un proceso en curso
- **Descargar resultado** cuando termine (o un parcial si cancelaste)
- **↻** limpiar la pantalla e historial de esa pestaña
- Ver un **badge** en el ícono de la extensión cuando termina una tarea (`!` verde / rojo / amarillo)

---

## Cómo instalarla (modo desarrollador)

1. Descarga o clona este proyecto en tu computador.
2. Abre Chrome y ve a:  
   `chrome://extensions`
3. Activa **Modo de desarrollador** (arriba a la derecha).
4. Pulsa **Cargar extensión sin empaquetar**.
5. Selecciona la carpeta del proyecto  
   (`PluginValidateThirds`), la que contiene el archivo `manifest.json`.
6. Aparece como **Plugin Help** en la barra. Si no la ves, pulsa el ícono de piezas y fíjala.

No hace falta `npm install` para usarla: la librería Excel ya está en `lib/`.

---

## Cómo configurarla (primera vez)

1. Abre la extensión.
2. Ve a **Configuración → Acceso Campus**.
3. Pega tu **Access Token / ID de Canvas** y pulsa **Guardar token**.
4. Ve a **Configuración → Gestión de cursos**.
5. Pulsa **Cargar cursos**.
6. Marca los cursos (o **Todos**) y pulsa **Guardar selección**.

Sin token o sin cursos guardados, Validar Mi Nube / Validar Ingreso no pueden correr.

### Validar Mi Nube

1. Abre una pestaña de Mi Nube.
2. En la extensión, pestaña **Validar Mi Nube**.
3. Pulsa **Validar**.
4. Cuando termine (notificación / badge), pulsa **Descargar resultado**.

### Validar Ingreso

1. Pestaña **Validar Ingreso**.
2. Pulsa **Validar**.
3. Al terminar, pulsa **Descargar resultado**.

---

## Qué hacer cuando hay una nueva versión

Si ya tienes la extensión cargada en modo desarrollador:

1. Actualiza los archivos del proyecto (pull de git, o reemplaza la carpeta con la nueva versión).
2. Abre `chrome://extensions`.
3. En la tarjeta de **Plugin Help**, pulsa el botón **Recargar** (ícono circular).
4. Cierra y vuelve a abrir el popup de la extensión.

Notas:

- El token y los cursos guardados suelen conservarse (viven en el almacenamiento local de Chrome).
- Si algo se ve raro (pantalla vieja, botones que no responden), recarga la extensión y limpia con **↻** en la pestaña.
- No hace falta “desinstalar y volver a cargar” salvo que cambie la estructura de carpetas o el `manifest.json` de forma fuerte. En ese caso: quita la extensión y vuelve a **Cargar extensión sin empaquetar**.

### Si es la primera vez que instalas esa versión en otro PC

Repite los pasos de [Cómo instalarla](#cómo-instalarla-modo-desarrollador) y [Cómo configurarla](#cómo-configurarla-primera-vez).

---

## Requisitos

- Google Chrome (o navegador compatible con extensiones Manifest V3).
- Access Token de Canvas con permiso para leer cursos y usuarios.
- Acceso a:
  - `https://campus.fundacionmarialuisa.org`
  - el sitio de Mi Nube que uses para validar perfiles

---

## Estructura del proyecto (resumen)

| Archivo / carpeta | Rol |
|-------------------|-----|
| `manifest.json` | Definición de la extensión |
| `popup.html` / `popup.js` | Interfaz |
| `background.js` | Procesos en segundo plano |
| `canvasApi.js` | Llamadas a la API de Campus |
| `excelExport.js` / `ingresoExport.js` | Generación de Excel |
| `lib/exceljs.min.js` | Librería para Excel |

---

## Soporte rápido de problemas

| Problema | Qué revisar |
|----------|-------------|
| “Configura el token…” | Configuración → Acceso Campus |
| “Selecciona cursos…” | Configuración → Gestión de cursos → Guardar selección |
| Mi Nube no valida | Pestaña de Mi Nube abierta en el mismo navegador |
| No aparece la pestaña nueva | Recargar la extensión en `chrome://extensions` |
| Badge o aviso viejo | Abrir el popup (limpia el badge) o usar **↻** |
