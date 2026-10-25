# Notas para el backend Android/offline

Este directorio conserva el backend original del ZIP sin modificar las reglas de negocio.

La app Android de `../android/` utiliza las rutas actuales:

- `GET /fincas/`
- `GET /tipos-animales/`
- `GET /ganado/api/`
- `POST /ganado/api/` (multipart)
- `PUT /ganado/{id}` (JSON)

Antes de producción, implementar en el backend:

1. Autenticación (por ejemplo OAuth2/JWT) y autorización por usuario/finca para todas las vistas y endpoints.
2. Una ruta de sincronización idempotente que acepte un UUID de operación generado por el cliente y registre el resultado transaccionalmente.
3. Campos de versión/`updated_at` y control de concurrencia para detectar edición simultánea; responder con conflicto en vez de sobrescribir silenciosamente.
4. Política para los datos temporales creados offline y su relación con los IDs de PostgreSQL.
5. Almacenamiento de fotos en objeto/bucket persistente y carga reintentable.
6. Pruebas de migración, sincronización duplicada, dos dispositivos concurrentes y recuperación tras desconexión.
