---
tags: [tmr, glosario]
---

# Glosario

← [[00 - TMR (inicio)]]

| Término | Qué significa |
| --- | --- |
| **API** | La "ventanilla" del backend: URLs a las que el frontend envía pedidos |
| **Endpoint** | Una URL concreta de la API, como `/api/time-report/actividades` |
| **GET / POST / PUT / DELETE** | Tipos de pedido HTTP: leer, crear, modificar, eliminar |
| **Código de estado** | Número de respuesta: 200 bien, 201 creado, 400 datos inválidos, 401 sin sesión, 403 sin permiso, 404 no existe, 500 error del servidor |
| **JSON** | Formato de texto para enviar datos: `{ "horas": 8 }` |
| **JWT** | Texto firmado que prueba quién eres; viaja en cada pedido. Ver [[05 - Autenticación y permisos]] |
| **Refresh token** | Token de larga duración para pedir un JWT nuevo |
| **Claim** | Un dato dentro del JWT (id de usuario, permisos) |
| **Interceptor** | Código Angular que pasa por todos los pedidos HTTP |
| **Guard** | Código Angular que decide si puedes entrar a una ruta |
| **Componente** | Pieza de pantalla: HTML + estilos + lógica |
| **Servicio** | Clase que agrupa lógica reutilizable, por ejemplo llamar al backend |
| **NgRx / Store** | Lugar central donde vive el estado compartido de la app |
| **Observable (RxJS)** | Un valor que llegará en el futuro, o varios a lo largo del tiempo |
| **DTO** | Objeto que solo transporta datos entre capas |
| **Entidad** | Clase C# que representa una tabla de la base de datos |
| **ORM / EF Core** | Traduce objetos C# a filas de la base de datos y viceversa |
| **DbContext** | La "puerta" de EF Core a la base de datos. En TMR: `ApplicationDbContext` |
| **Migración** | Archivo que describe un cambio en la estructura de la base de datos |
| **Esquema (BD)** | Agrupación de tablas dentro de la base de datos |
| **Inyección de dependencias** | El framework te entrega los objetos que pides como parámetros |
| **Vertical slice** | Organizar el código por funcionalidad en vez de por tipo de archivo |
| **Evento de dominio** | Hecho del negocio que ya ocurrió (`CorteDeHorasAlcanzado`) |
| **Handler** | Código que reacciona a un evento |
| **Outbox** | Patrón: guardar el evento en la BD antes de procesarlo, para no perderlo. Ver [[07 - Notificaciones y cortes]] |
| **Job / Quartz** | Tarea que se ejecuta sola según un horario |
| **SSE** | Server-Sent Events: el servidor empuja datos al navegador por una conexión abierta |
| **Contenedor / Docker** | Caja con la app y todo lo que necesita para correr igual en cualquier servidor |
| **Imagen** | La "plantilla" a partir de la cual se crea un contenedor |
| **GHCR** | GitHub Container Registry: donde se guardan las imágenes |
| **CI/CD** | Automatización que compila, prueba y despliega en cada cambio. Ver [[08 - Despliegue CI-CD]] |
| **Health check** | Endpoint que responde si la app está viva y sus dependencias funcionan |
