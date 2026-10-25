# Hato App Android — fuente inicial

Este paquete contiene el backend original de `Innovacion-main.zip` en `backend/` y un proyecto Android nativo mínimo en `android/`. La aplicación Android carga una interfaz local dentro de WebView; los registros se guardan en IndexedDB del dispositivo y se sincronizan con la API existente cuando hay conexión.

## Estado verificado

- ZIP de origen inspeccionado: FastAPI + SQLAlchemy + Jinja2, con API existente para `GET /ganado/api/`, `POST /ganado/api/`, `PUT /ganado/{id}`, `GET /fincas/` y `GET /tipos-animales/`.
- Entorno actual: OpenJDK 21 disponible.
- No se encontraron `gradle`, `adb`, `sdkmanager`, Flutter ni variables `ANDROID_HOME` / `ANDROID_SDK_ROOT`.
- La conexión de red DNS no está disponible en este entorno; no fue posible descargar Gradle ni el Android SDK.
- Por lo anterior, **no se ha compilado ni verificado un APK**. Este entregable es código fuente, no un APK listo para instalar.

## Funciones de esta versión inicial

- Interfaz móvil local, que puede abrirse sin cargar la web del servidor.
- Configuración de la URL HTTPS del backend por dispositivo.
- Descarga y almacenamiento local de fincas, tipos y animales consultados.
- Consulta de animales previamente sincronizados sin Internet.
- Alta y edición local de animales, con cola persistente en IndexedDB.
- Sincronización al recuperar conexión y al pulsar el botón correspondiente.
- Si falla el envío, la operación pendiente se conserva en el dispositivo.

## Limitaciones importantes

1. **No incluye autenticación ni autorización.** El README del backend original marca usuarios/login como pendiente. El backend actual expone operaciones CRUD sin comprobar identidad. No lo publiques con datos reales ni lo uses con información sensible hasta implementar login, roles y protección de todas las rutas.
2. Los dispositivos comparten datos únicamente si apuntan al mismo backend y a la misma base de datos persistente (PostgreSQL/Supabase recomendado). No uses SQLite efímero en un despliegue como Render.
3. El modo offline actual cubre lectura, registro y edición de datos básicos de animales. Las fotografías, eventos/historial, cambios de fincas/tipos, borrados y modo QR offline no están en la cola offline.
4. Los conflictos simultáneos entre dos dispositivos todavía no se resuelven automáticamente. Antes de uso productivo, se debe añadir control de versión/conflicto en el servidor; no se debe asumir que una sincronización concurrente conserva siempre la última edición deseada.
5. Para que las listas de fincas y tipos estén disponibles sin conexión, el usuario debe conectarse al menos una vez y descargarlas.
6. El API de origen no implementa una clave idempotente para creación. La app elimina reintentos pendientes después de una respuesta satisfactoria, pero un corte de conexión en el instante exacto en que el servidor guarda el registro puede requerir resolver manualmente un duplicado por identificación.

## Cómo compilar en Android Studio

Requisitos: Android Studio reciente, JDK 17 o 21, Android SDK Platform 35 y Gradle compatible con Android Gradle Plugin 8.7.3 (Gradle 8.9).

1. Descomprime el paquete.
2. Abre la carpeta `android/` en Android Studio.
3. Instala Android SDK Platform 35 desde SDK Manager cuando lo solicite.
4. Permite la sincronización de Gradle y espera a que descargue Android Gradle Plugin 8.7.3.
5. Ejecuta en un dispositivo/emulador con `Run`.
6. Para crear un APK de prueba: **Build > Build Bundle(s) / APK(s) > Build APK(s)**. El APK de depuración se genera normalmente en `android/app/build/outputs/apk/debug/app-debug.apk`.
7. Para distribuirlo, genera un APK firmado desde **Build > Generate Signed Bundle / APK**.

## Configurar el backend

1. Despliega `backend/` en un servicio HTTPS.
2. Configura `DATABASE_URL` con PostgreSQL/Supabase persistente.
3. Configura `PUBLIC_BASE_URL` con el dominio público para que los QR web apunten al dominio correcto.
4. Configura almacenamiento persistente/externo para fotos.
5. Añade autenticación y autorización antes de exponer datos reales.
6. Instala la app, abre la configuración del servidor y pega la URL HTTPS. Pulsa **Conectar y descargar datos**.

## Pruebas mínimas antes de uso real

- Primer inicio online, descarga de fincas/tipos/animales.
- Activar modo avión, consultar animales y editar uno.
- Registrar un animal offline, cerrar y volver a abrir la app, comprobar que sigue pendiente.
- Recuperar conexión y verificar que el servidor recibió la operación.
- Repetir en dos dispositivos apuntando al mismo backend.
- Simular identificación duplicada, servidor no disponible y corte de conexión durante sincronización.
- Añadir autenticación, permisos y pruebas de conflictos antes de datos de producción.
