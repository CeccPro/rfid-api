# RFID API (Bun)

Server minimal en Node.js compatible con Bun para exponer una API que guarda alumnos leídos por el sistema RFID.

Características:
- Endpoints:
  - GET /                -> health check
  - POST /register       -> Registrar alumno (requiere API Key)
  - GET /student/:uid    -> Obtener alumno por UID (requiere API Key)
  - GET /students        -> Listar todos o buscar por `control` query param (requiere API Key)

- Autenticación por API Key: la variable de entorno `VALID_API_KEYS` debe contener una JSON array como `["key1","key2"]` o una ruta a un archivo `.json` que contenga ese array, o una lista separada por comas.

- Almacenamiento: `alumnos.json` en la misma carpeta.

Ejemplo de export de `VALID_API_KEYS` en PowerShell:

```powershell
$env:VALID_API_KEYS='["devkey123","anotherkey"]'
bun run main.js
```

Ejemplo de petición para registrar (usar curl):

```powershell
curl -X POST http://localhost:3000/register -H "Content-Type: application/json" -H "x-api-key: devkey123" -d '{"nombre":"Juan Perez","grupo":"3A","control":"12345","uid":"04AABBCC"}'
```

Consultar por UID:

```powershell
curl http://localhost:3000/student/04AABBCC -H "x-api-key: devkey123"
```

Listar todos:

```powershell
curl http://localhost:3000/students -H "x-api-key: devkey123"
```

Notas:
- El server usa Bun. Si no dispones de Bun, el archivo se puede ejecutar con Node.js en modo compatibilidad.
- Los campos esperados para registrar están alineados con `main.py`: nombre, grupo, control (numérico), uid.
