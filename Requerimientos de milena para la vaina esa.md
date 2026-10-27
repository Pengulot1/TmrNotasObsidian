

pantallas principales : 
Inicio
Colaboradores 
contratos
anticipos
liquidaciones
documentos
exportaciones
configuracion

Inicio tiene:
una parte donde muestra la fecha por ejemplo solo muestra "Noviembre 2026"

Resumen neto

el uno y quincena

fin de mes

16 al 30 de noviembre 

Por revisar 
aqui estan los colaboradores pendientes


![[Pasted image 20261027095403.png]]

Frontend:

```mermaid
flowchart TB

    subgraph FRONT["rps-frontend · Aplicación Angular"]

        APP["Aplicación y rutas"]

  

        subgraph CORE["Core"]

            AUTH["Sesión y permisos"]

            HTTP["Cliente HTTP e interceptores"]

            LAYOUT["Layout y navegación"]

        end

  

        subgraph FEATURES["Funcionalidades"]

            DASH["Dashboard"]

            COL["Colaboradores y proyectos"]

            CON["Contratos y honorarios"]

            ANT["Anticipos"]

            LIQ["Liquidaciones"]

            DOC["Documentos"]

            EXP["Exportaciones"]

            CFG["Configuración"]

        end

  

        SHARED["Shared · Componentes visuales reutilizables"]

  

        APP --> CORE

        APP --> FEATURES

        FEATURES --> SHARED

        FEATURES --> HTTP

    end
```
