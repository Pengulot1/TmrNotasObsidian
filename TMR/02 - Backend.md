---
tags: [tmr, backend, dotnet]
---

# Backend (`tmr-backend`)

← [[00 - TMR (inicio)]] · [[01 - Arquitectura general]]

API REST en **C# / .NET 10 con ASP.NET Core**, usando *Minimal APIs* (las rutas se declaran con `MapGet`, `MapPost`, etc.). Unos 315 archivos y ~3.000 nodos en el grafo.

## Estructura de carpetas

```
tmr-backend/
├── Program.cs            ← arranque: registra servicios, seguridad, jobs y rutas
├── Features/             ← un módulo por carpeta (vertical slices)
│   ├── Auth/
│   ├── TimeReport/
│   ├── Notificaciones/
│   ├── Dashboard/  Clientes/  Colaboradores/  Lideres/
│   ├── Proyectos/  Proveedores/  Catalogos/  Configuracion/
│   ├── CargaActividades/  Reportes/  HealthCheck/
├── Infrastructure/
│   ├── Database/         ← ApplicationDbContext + entidades (Tbl...)
│   ├── Security/         ← JWT, hash de contraseñas, permisos
│   ├── Messaging/        ← EventBus, Outbox, Dispatcher
│   ├── BackgroundServices/ ← limpieza de sesiones, aviso semanal
│   └── Logging/ Persistence/ Extensions/ ...
├── Shared/               ← middleware, excepciones, wrappers de respuesta
└── Migrations/           ← historial de cambios de la base de datos
```

### Dentro de un feature

Cada carpeta de `Features/` repite el mismo patrón. Ejemplo con `TimeReport`:

| Subcarpeta | Qué contiene | Ejemplo |
| --- | --- | --- |
| `Endpoints/` | Las URLs que expone el módulo | `TimeReportEndpoints.cs` |
| `DTOs/` | Objetos que entran y salen por la API | `ActividadesDTOs.cs` |
| `Domain/` | Reglas y conceptos del negocio | `RegistroTiempo.cs` |
| `Services/` | Lógica reutilizable | `CalculoHorasPeriodo.cs` |

Módulos más grandes, como `Notificaciones`, se dividen en capas: `Domain/`, `Application/` (handlers y queries), `Infrastructure/` (jobs, Apprise) y `Presentation/` (endpoints).

## Tecnologías y para qué se usan

| Tecnología | Uso en TMR |
| --- | --- |
| ASP.NET Core Minimal APIs | Definir los endpoints REST |
| Entity Framework Core 10 + Npgsql | Leer y escribir en PostgreSQL sin SQL a mano |
| JWT Bearer + BCrypt | Autenticación. Ver [[05 - Autenticación y permisos]] |
| FluentValidation | Validar los DTOs de entrada (`LoginRequestValidator`, etc.) |
| Quartz | Job que revisa las fechas de corte cada minuto |
| Channels + BackgroundService | Bus de eventos interno con Outbox |
| Apprise | Envío de notificaciones (correo y otros canales) |
| MailKit | Correos de Auth (recuperar contraseña) |
| QuestPDF / ClosedXML / MiniExcel | PDFs y Excel |
| Serilog | Logs estructurados |
| Scalar / OpenAPI | Documentación interactiva de la API |
| Rate limiting (política `auth`) | Limitar intentos de login |

## `Program.cs`: el punto de arranque

Es el archivo donde todo se "enchufa" (63 conexiones en el grafo). En orden aproximado:

1. Carga configuración (`appsettings.json`, variables de entorno, `.env`).
2. Configura Serilog.
3. Registra `ApplicationDbContext` con `UseNpgsql(...)`.
4. Configura CORS (`AllowFrontend`) y rate limiting.
5. Configura autenticación JWT y el proveedor de políticas de permisos.
6. Registra Quartz para notificaciones (`AddNotificacionesQuartz`).
7. Mapea los endpoints de cada feature y expone Scalar.

## La pieza central: `ApplicationDbContext`

Es la clase con más conexiones de todo el sistema (189). Casi todos los endpoints la reciben por inyección de dependencias y la usan para consultar o guardar. Ver [[04 - Base de datos]].

```csharp
// Patrón típico de un endpoint
group.MapGet("/", async (ApplicationDbContext db) => { ... });
```

## Patrones que conviene reconocer

- **Inyección de dependencias:** los endpoints piden lo que necesitan como parámetros (`ApplicationDbContext db`, `ClaimsPrincipal user`) y .NET se los entrega.
- **Grupos de rutas:** `app.MapGroup("/api/time-report").RequireAuthorization()` protege todos los endpoints del grupo de una vez.
- **Eventos de dominio:** `CorteDeHorasAlcanzado`, `HorasFaltantesDetectadas`, `NotificacionEnviada`... Ver [[07 - Notificaciones y cortes]].

> [!note] Detalle a revisar
> El `.csproj` incluye el proveedor de SQL Server, pero `Program.cs` usa `UseNpgsql`. Si solo se usa PostgreSQL, ese paquete podría sobrar.
