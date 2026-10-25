# Generar el APK de Hato App con GitHub Actions

Este repositorio incluye un workflow en `.github/workflows/android.yml` que compila el APK de depuración en los servidores de GitHub. No requiere instalar Android Studio ni Android SDK en tu computador, pero sí necesitas una cuenta de GitHub.

## Pasos

1. Entra a https://github.com/new y crea un repositorio nuevo, por ejemplo `hato-app-android`.
2. Descomprime el ZIP y sube **todo el contenido** de la carpeta `HatoApp-Android` al repositorio. La carpeta `.github/workflows/` debe quedar en la raíz del repositorio, junto a `android/` y `backend/`. No subas una carpeta adicional que haga que el workflow quede anidado.
3. Confirma los cambios en la rama `main` o `master`.
4. En GitHub, abre la pestaña **Actions**. Selecciona **Build Hato App APK** y pulsa **Run workflow** si no se inició automáticamente.
5. Espera a que la ejecución termine con el icono verde. Abre la ejecución y baja hasta **Artifacts**.
6. Descarga `HatoApp-debug-apk`. Dentro encontrarás `app-debug.apk`.
7. Pasa el APK a tu teléfono Android y ábrelo para instalarlo. Puede que Android solicite permitir instalaciones desde esa fuente.

## Si la compilación falla

Abre la ejecución en Actions y entra al paso rojo para ver el error. No descargues el APK desde una ejecución fallida. El workflow instala Gradle 8.9 y Android SDK Platform 35, y después ejecuta `assembleDebug`.

## Importante

- El APK de depuración sirve para pruebas; no es una versión de lanzamiento firmada para distribución pública.
- La app necesita que configures la URL HTTPS real del backend desde la propia aplicación.
- El backend de esta versión no implementa autenticación/autorización completa. No lo uses con datos reales o sensibles hasta implementar controles de acceso y asegurar la persistencia de la base de datos y las fotos.
- Los dispositivos solo compartirán datos cuando apunten al mismo backend y base de datos persistente.
