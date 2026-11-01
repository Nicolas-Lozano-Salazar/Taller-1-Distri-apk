# Datos Abiertos de Colombia (Flutter)

Aplicación Flutter que consume la API pública [API Colombia](https://api-colombia.com/) para mostrar información de Colombia.

## Endpoints seleccionados

- `Department` (Departamentos)
- `President` (Presidentes)
- `TouristicAttraction` (Atractivos turísticos)
- `TypicalDish` (Platos típicos)
- `Airport` (Aeropuertos)

Base URL: `https://api-colombia.com/api/v1` (configurada en `.env`).

## Arquitectura

```
lib/
  config/      // configuración general (.env)
  models/      // modelos con fromJson y toJson
  routes/      // configuración de go_router
  services/    // llamadas HTTP (http)
  themes/      // estilos o temas globales
  views/       // pantallas principales (Dashboard, Listado, Detalle)
  widgets/     // componentes reutilizables
  main.dart
```

## Paquetes utilizados

- `http` — consumo de API.
- `go_router` — navegación entre pantallas.
- `flutter_dotenv` — manejo de variables de entorno.

## Rutas (go_router)

- `/` — Dashboard
- `/departments` — Listado de departamentos
- `/departments/:id` — Detalle de departamento
- `/presidents` — Listado de presidentes
- `/presidents/:id` — Detalle de presidente
- `/touristic-attractions` — Listado de atractivos turísticos
- `/touristic-attractions/:id` — Detalle de atractivo turístico
- `/typical-dishes` — Listado de platos típicos
- `/typical-dishes/:id` — Detalle de plato típico
- `/airports` — Listado de aeropuertos
- `/airports/:id` — Detalle de aeropuerto

## Ejemplo de respuesta JSON (Department)

```json
{
  "id": 1,
  "name": "Amazonas",
  "description": "Amazonas es uno de los treinta y dos departamentos...",
  "cityCapitalId": 1,
  "municipalities": 2,
  "surface": 109665,
  "population": 82068,
  "phonePrefix": "8",
  "regionId": 4
}
```

## Manejo de estados

Cada pantalla de listado y detalle maneja tres estados:
- Cargando (`CircularProgressIndicator`)
- Éxito (render de datos con `ListView.builder`)
- Error (mensaje + botón de reintento cuando aplica)

## Capturas de la aplicación

### Dashboard (Home)
![Dashboard](screenshots/dashboard.png)
Pantalla principal con cards para cada endpoint. Al tocar una card se navega al listado correspondiente.

### Listado (Departamentos)
![Listado](screenshots/listado.png)
Petición GET a la API, render con ListView.builder. Estados de carga, éxito y error (mensaje + reintentar).

### Detalle (Departamento)
![Detalle](screenshots/detalle.png)
Pantalla de detalle con información completa del registro seleccionado, navegación con go_router y paso de parámetros (id).

## Ejecución

```bash
flutter pub get
flutter run
```

Configurar `.env` con:

```
API_BASE_URL=https://api-colombia.com/api/v1
```

## Publicación (Firebase App Distribution)

Flujo resumido: **Generar APK → App Distribution → Testers → Instalación → Actualización**.

### Pasos para replicar el proceso en el equipo

1. Generar build de release:
   ```bash
   flutter build apk --release
   ```
   El APK se genera en `build/app/outputs/flutter-apk/app-release.apk`.
2. Verificar permisos mínimos en `android/app/src/main/AndroidManifest.xml` (incluye `INTERNET`).
3. Crear/abrir el proyecto en Firebase Console y registrar la app Android con el `applicationId` (`com.taller.taller_datos_abiertos`).
4. En App Distribution → Testers & Groups, crear el grupo `QA_Clase` y agregar el tester `dduran@uceva.edu.co`.
5. En Releases, subir `app-release.apk`, asignar al grupo `QA_Clase`, incluir Release Notes y distribuir; copiar el enlace de instalación.
6. Instalar en un dispositivo Android físico y ejecutar la app.
7. Para la actualización incremental, cambiar la versión en `pubspec.yaml` (ej.: `1.0.0+1` → `1.0.1+2`), regenerar el APK y volver a distribuir.

### Versionado

- `version: 1.0.1+2` (versionName 1.0.1, versionCode 2).
- Formato de Release Notes sugerido: fecha, cambios, responsable, incidencias y estado de pruebas.
