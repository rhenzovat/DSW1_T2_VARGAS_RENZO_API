# Mi Library API - Backend

## ¿Qué es este proyecto?

Este es un proyecto de API backend desarrollado con ASP.NET Core para gestionar una biblioteca. Aquí encontrarás todas las instrucciones para configurar tu entorno, crear la base de datos y ejecutar la aplicación en tu computadora.

## Requisitos (qué necesitas instalar)

Antes de empezar, asegúrate de tener instalado lo siguiente:

- **.NET 8 SDK** — verifica con `dotnet --version`
- **MySQL o MariaDB** — una base de datos para guardar la información
- **Opcional**: `dotnet-ef` — herramienta para gestionar la base de datos
- **Proveedor MySQL**: `Pomelo.EntityFrameworkCore.MySql` v8.0.2

## Cómo ejecutar el proyecto (Guía rápida)

Sigue estos 5 pasos para que tu API esté funcionando:

Paso 1: Configurar las credenciales de la base de datos

Copia el archivo `.env.example` y renómbralo a `.env`. Luego, actualiza los datos de conexión con tus credenciales de MySQL:

BD_HOST = tu_host
DB_PORT = tu_puerto
DB_NAME = tu_base_de_datos
DB_USER = tu_usuario
DB_PASSWORD = tu_contraseña

Paso 2: Crear la base de datos en MySQL

Abre tu cliente MySQL y ejecuta este comando:

```sql
CREATE DATABASE library_db;

Paso 3: Aplicar las migraciones

Las migraciones actualizan tu base de datos con las tablas necesarias. Ejecuta este comando en PowerShell:

```dotnet ef database update --project src\Library.Infrastructure\Library.Infrastructure.csproj --startup-project src\Library.API\Library.API.csproj
```

Paso 4: Iniciar la API
Ejecuta este comando para que la API comience a funcionar:

```powershell
dotnet run --project "src\Library.API\Library.API.csproj"
```

Paso 5: Acceder a la interfaz gráfica (Swagger)
Abre tu navegador y ve a la URL que aparece en la consola (normalmente http://localhost:5185/swagger ). Aquí podrás probar todos los endpoints de forma visual.

¿En qué puerto se ejecuta?
Por defecto, la API escucha en http://localhost:5185. Cuando ejecutes dotnet run, busca en la consola la línea que dice Now listening on: para ver la URL exacta.

Ejemplos de uso (Requests)
Aquí hay algunos ejemplos de cómo usar los endpoints principales:

Crear un nuevo libro
Método: POST /api/books

{
  "title": "Libro 01",
  "author": "Autor 01",
  "isbn": "ISBN-PRUEBA-001",
  "stock": 2
}
```

- Crear préstamo (POST `/api/loans`):

```json
{
  "bookId": 1,
  "studentName": "Alumno 01"
}
```

- Dar de baja un libro (POST `/api/books/dar-baja/{id}`):

```json
{
  "reason": "motivo opcional"
}
```

Reglas importantes del sistema
Antes de usar la API, entiende estos comportamientos:

Endpoint de libros: GET /api/books solo devuelve los libros activos (IsActive = true).

Crear un préstamo:

El stock del libro disminuye en 1.
No se puede prestar un libro si no hay stock disponible.
Devolver un préstamo:

El stock del libro aumenta en 1.
Dar de baja un libro:

Se registra siempre en la tabla de bajas (tb_articulos_baja).
Si hay stock, también se crea un registro en la tabla de liquidación (tb_articulos_liquidacion) y el stock se pone en 0.
Solución de problemas
Ver los comandos SQL que se ejecutan
Si estás en modo Development, la API mostrará todos los comandos SQL en la consola. Esto es útil para verificar que los datos se están guardando correctamente en las tablas de bajas y liquidación.

Si hay errores con Entity Framework Core
Puedes generar un script SQL manualmente y ejecutarlo en MySQL:

dotnet ef migrations script --project src\Library.Infrastructure\Library.Infrastructure.csproj --startup-project src\Library.API\Library.API.csproj -o migration.sql
# Luego ejecuta el archivo migration.sql en tu cliente MySQL


4. Guarda el archivo (Ctrl+S)

**Una vez actualizado, haz commit y push:**

```powershell
cd "c:\Users\Asus\Desarrollo de ser web1\DSW1_T2_VARGAS_RENZO_API"
git add README.md
git commit -m "Mejorar README con explicaciones más claras para estudiantes"
git push origin main
---