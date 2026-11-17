# Guías globales para agentes — Índice maestro

Conjunto de guías (tipo skills) para trabajar de forma estable en **cualquier proyecto** de Victor desde WSL, sin importar la ruta, el lenguaje o el editor. Son globales: sirven para tdesktop y para cualquier otro proyecto en `/mnt/c/Users/VICTOR/code/`.

## Contenido

| Archivo | Para qué sirve |
|---|---|
| `00-acceso-rutas-wsl.md` | Llegar a cualquier ruta de Windows (C:, D:, E:) desde WSL, mapeo universal, montajes, permisos, rutas largas, y corrección del error `Input/output error`. |
| `01-flujo-y-alcance.md` | Cómo trabajar: investigar → planificar → editar → verificar → resumir. Cómo definir el alcance (qué se puede y no tocar) en cada proyecto. |
| `02-compilacion-errores-comunes.md` | Cómo compilar según el tipo de proyecto, cómo leer y reportar errores (MSVC, gcc, cmake, npm…), y soluciones a errores frecuentes. |
| `03-diagnostico-entorno-wsl.md` | Diagnóstico del entorno: rutas válidas, herramientas disponibles/no disponibles, problemas típicos y sus soluciones. |
| `04-respaldos-manuales.md` | Estrategia de copias `.bak` y restauración, ya que no se usa git. |
| `05-peticion-de-cambios.md` | Formato recomendado para pedir cambios al agente y evitar malentendidos. |
| `06-validaciones-y-verificacion.md` | Cómo verificar un cambio sin git: relectura, grep, diff, aceptación y plan de rollback. |
| `07-configuracion-y-terminos.md` | Configuración del entorno (wsl.conf, .wslconfig, montajes, PATH) y glosario de términos. |
| `08-restricciones-y-reglas.md` | Restricciones de seguridad: git, borrados, secrets, scope, destructivos, reportes. |

## Uso rápido (cualquier proyecto)

1. Leer `00` antes de tocar archivos.
2. Leer `01` antes de editar; definir alcance del proyecto.
3. Ante error de compilación → `02`.
4. Ante error de entorno (I/O, montaje, permisos) → `03`.
5. Antes de editar → `04` (respaldo).
6. Al pedir un cambio → `05`.
7. Después de editar → `06` (validar).
8. Dudas de configuración o términos → `07`.
9. Líneas rojas → `08`.

## Adaptar a un proyecto nuevo (3 pasos)

1. Abrir el proyecto: `cd /mnt/c/Users/VICTOR/code/<proyecto>`.
2. Crear `<proyecto>/NOTAS_AGENTE.md` con: alcance permitido, comando de compilación, comandos de prueba, archivos clave.
3. Trabajar con estas guías como referencia global.

## Reglas de oro

- No tocar git (commit/push) salvo petición explícita.
- Solo cambios en el alcance indicado; dejar intacto el resto.
- Respaldar antes de editar (`04`).
- Cambios quirúrgicos: `oldString` exacto → `newString` mínimo.
- Verificar después de editar (`06`) y resumir al final.
