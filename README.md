# 📄 README.md - Hospital.Project


```markdown
# 🏥 Hospital Management System - ASP.NET Core

Sistema de gestión hospitalaria desarrollado con ASP.NET Core MVC y Entity Framework Core.

## 📋 Descripción

Aplicación web para la gestión de personal médico y administrativo de un hospital. Permite administrar:
- Personal del hospital (Staff)
- Categorías de personal (Doctores, Enfermeras, Administrativos)
- Especialidades médicas (Pediatría, Cardiología, etc.)
- Pacientes

## 🛠️ Tecnologías Utilizadas

- **Framework:** ASP.NET Core 8.0 (LTS)
- **ORM:** Entity Framework Core 9.0.13
- **Base de Datos:** SQL Server
- **Lenguaje:** C# 12
- **Patrón de Diseño:** MVC (Model-View-Controller)

## 📦 Requisitos Previos

- Visual Studio 2022 o superior
- .NET 8.0 SDK
- SQL Server 2019 o superior (o SQL Server Express)
- SQL Server Management Studio (opcional, recomendado)

## 🚀 Instalación

### 1. Clonar el repositorio
```bash
git clone [URL-del-repositorio]
cd Hospital.Project
```

### 2. Restaurar paquetes NuGet
```bash
dotnet restore
```

O desde Visual Studio:
- Clic derecho en la solución → **Restore NuGet Packages**

### 3. Configurar la base de datos

Editar el archivo `appsettings.json` y actualizar la cadena de conexión:

```json
{
  "ConnectionStrings": {
    "DefaultDbConnection": "Data Source=TU_SERVIDOR;Initial Catalog=Hospital2;Integrated Security=True;TrustServerCertificate=True"
  }
}
```

Reemplazar `TU_SERVIDOR` con:
- `localhost` o `(localdb)\MSSQLLocalDB` para LocalDB
- `.\SQLEXPRESS` para SQL Server Express
- Tu nombre de servidor personalizado

### 4. Crear la base de datos

Abrir **Package Manager Console** en Visual Studio:
- Tools → NuGet Package Manager → Package Manager Console

Ejecutar:
```bash
Add-Migration InitialMigration
Update-Database
```

### 5. Ejecutar la aplicación

Presionar **F5** en Visual Studio o ejecutar:
```bash
dotnet run
```

## 📁 Estructura del Proyecto

```
Hospital.Project/
│
├── Controllers/           # Controladores MVC
│   └── HomeController.cs
│
├── Data/                  # Contexto de base de datos
│   └── ApplicationDbContext.cs
│
├── Models/                # Modelos de datos
│   ├── StaffModel.cs
│   ├── StaffCategoryModel.cs
│   ├── SpecialityModel.cs
│   └── PatientModel.cs
│
├── Views/                 # Vistas Razor
│   ├── Home/
│   └── Shared/
│
├── Migrations/            # Migraciones de EF Core
│
├── wwwroot/              # Archivos estáticos (CSS, JS, imágenes)
│
├── appsettings.json      # Configuración de la aplicación
└── Program.cs            # Punto de entrada
```

## 🗂️ Modelos de Datos

### StaffModel (Personal del Hospital)
Representa a los empleados del hospital.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| Id | int | Identificador único |
| Name | string | Nombre (requerido, max 100) |
| LastName | string | Apellido (requerido, max 100) |
| Email | string | Correo electrónico |
| PhoneNumber | string | Número de teléfono |
| StaffCategoryId | int | ID de categoría (FK) |
| SpecialityId | int? | ID de especialidad (FK, nullable) |
| HireDate | DateTime | Fecha de contratación |
| IsActive | bool | Estado activo (soft delete) |

### StaffCategoryModel (Categorías de Personal)
Define el tipo de empleado (Doctor, Enfermera, etc.).

| Campo | Tipo | Descripción |
|-------|------|-------------|
| Id | int | Identificador único |
| Name | string | Nombre de categoría (requerido, max 100) |

### SpecialityModel (Especialidades Médicas)
Especialidades para médicos.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| Id | int | Identificador único |
| Name | string | Nombre de especialidad (requerido, max 100) |

### PatientModel (Pacientes)
Información de pacientes registrados.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| Id | int | Identificador único |
| Name | string | Nombre (requerido, max 100) |
| LastName | string | Apellido (requerido, max 100) |
| DateOfBirth | DateTime | Fecha de nacimiento |
| BloodType | string | Tipo de sangre (max 50) |
| PhoneNumber | string | Teléfono |
| Email | string | Correo electrónico |
| Address | string | Dirección (max 200) |
| RegistrationDate | DateTime | Fecha de registro |
| IsActive | bool | Estado activo |

## 🔗 Relaciones entre Modelos

```
StaffCategoryModel (1) ──→ (N) StaffModel
SpecialityModel (1) ──→ (N) StaffModel
```

- Una **categoría** puede tener muchos **empleados**
- Una **especialidad** puede tener muchos **empleados**
- Un **empleado** tiene una **categoría** (obligatoria)
- Un **empleado** puede tener una **especialidad** (opcional)

## 💾 Comandos de Entity Framework

### Crear una nueva migración
```bash
Add-Migration NombreDeLaMigracion
```

### Aplicar migraciones pendientes
```bash
Update-Database
```

### Revertir la última migración
```bash
Remove-Migration
```

### Ver migraciones aplicadas
```bash
Get-Migration
```

## 🎨 Características Implementadas

- ✅ Arquitectura MVC
- ✅ Entity Framework Core (Code First)
- ✅ Migraciones automáticas
- ✅ Relaciones entre tablas (Foreign Keys)
- ✅ Data Annotations (validaciones)
- ✅ Soft Delete (IsActive)
- ✅ Manejo de valores nullable

## 📝 Buenas Prácticas Aplicadas

1. **Convenciones de nombres:**
   - Modelos terminan en `Model`
   - Propiedades en PascalCase
   - Nombres descriptivos en inglés

2. **Validaciones:**
   - `[Required]` para campos obligatorios
   - `[MaxLength]` para limitar longitud
   - Data Annotations centralizadas

3. **Soft Delete:**
   - Campo `IsActive` en lugar de eliminar registros
   - Preservación de datos históricos

4. **Separación de responsabilidades:**
   - Carpeta `Data/` para contexto de BD
   - Carpeta `Models/` para entidades
   - Configuración en `appsettings.json`

## 🐛 Solución de Problemas Comunes

### Error: "Cannot connect to database"
**Solución:** Verificar que SQL Server esté corriendo y la cadena de conexión sea correcta.

### Error: "A network-related error occurred"
**Solución:** Habilitar TCP/IP en SQL Server Configuration Manager.

### Error: "The view was not found"
**Solución:** Verificar que el nombre de la carpeta de vistas coincida con el nombre del Controller.

### Error: "Unable to create migrations"
**Solución:** Verificar que los paquetes de EntityFrameworkCore.Tools estén instalados.

## 📚 Recursos Adicionales

- [Documentación ASP.NET Core](https://docs.microsoft.com/aspnet/core/)
- [Entity Framework Core](https://docs.microsoft.com/ef/core/)
- [C# Programming Guide](https://docs.microsoft.com/dotnet/csharp/)

## 👨‍💻 Autor

**Proyecto de práctica - Desarrollo de Páginas Web Activas**
- Universidad de Oriente
- Sección A
- Fecha: Febrero 2026

## 📄 Licencia

Este proyecto es con fines educativos.

---

## 🔄 Historial de Versiones

### v1.0.0 (27/02/2026)
- ✅ Implementación inicial
- ✅ Modelos: Staff, StaffCategory, Speciality
- ✅ Configuración de base de datos
- ✅ Migraciones iniciales

### v1.1.0 (27/02/2026)
- ✅ Agregado modelo Patient
- ✅ Migración AddPatientModel

---

**Estado del Proyecto:** 🟢 Activo | **Build:** ✅ Passing | **Tests:** ⏳ Pendiente

```

---

## 📌 **CÓMO AGREGAR EL README A TU PROYECTO**

1. Clic derecho en el proyecto **Hospital.Project** (raíz)
2. **Add** → **New Item**
3. Busca: **Markdown File** o **Text File**
4. Nombre: `README.md`
5. Copia y pega el contenido de arriba
6. **Guarda** (Ctrl + S)

---

## 🎨 **OPCIONAL: Personalizar el README**

Puedes cambiar:
- ✏️ Tu nombre en la sección **Autor**
- 📅 Las fechas en **Historial de Versiones**
- 🔗 Agregar URL del repositorio si lo subes a GitHub
- 📸 Agregar capturas de pantalla
