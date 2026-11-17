# 08 — Restricciones y reglas de seguridad (guía global)

## 1. Líneas rojas (nunca)

* **No usar git** (commit, push, branch, reset, stash) salvo petición explícita del usuario.
* **No borrar** archivos desconocidos o cambios ajenos; tratarlos como trabajo del usuario.
* **No editar fuera del alcance** indicado (módulos, archivos, funciones).
* **No ejecutar comandos destructivos** (`rm -rf`, `format`, `dd`, `mkfs`, `sudo` innecesario).
* **No exponer secrets**: no leer/mostrar API keys, tokens, contraseñas, `.git-credentials`, cookies.
* **No subir nada** a servicios externos ni abrir datos personales del usuario.

## 2. Restricciones operativas

* Un solo tema por turno; si hay varias tareas, preguntar el orden.
* Cambios quirúrgicos: mínimo texto posible; no reescribir archivos enteros.
* Respaldar antes de editar (`04`).
* Verificar después de editar (`06`).
* Si algo falla (I/O, compilación, permiso): diagnosticar con `00`/`03` y reportar; no insistir ciegamente.
* Si el usuario pide algo riesgoso, confirmar antes de ejecutar.

## 3. Restricciones de alcance típicas (ejemplos)

* "Solo el módulo X; dejar Y intacto."
* "No tocar el reenvío masivo ni el pacing."
* "No cambiar contadores de otros módulos."
* "No tocar estilos/UI global."

Aplicar siempre la última instrucción explícita del usuario sobre alcance.

## 4. Validaciones obligatorias antes de terminar

1. ¿Solo se tocó lo del alcance? (releer/grep)
2. ¿Hay respaldos `.bak` de los archivos editados?
3. ¿No se usó git?
4. ¿Se reportó qué quedó intacto?
5. ¿Se explicó cómo validar (compilar/probar)?

## 5. Si se rompe algo

1. Restaurar `.bak` inmediatamente.
2. Informar al usuario qué pasó y qué se restauró.
3. Reintentar con edit más preciso o pedir el fragmento del archivo.
4. Documentar la causa en `NOTAS_AGENTE.md` del proyecto.

## 6. Comunicación

* Respuestas cortas y directas; sin preámbulos innecesarios.
* Indicar siempre rutas concretas (`archivo:línea`).
* No inventar datos del proyecto; si no se puede leer, decirlo y pedir el fragmento.
* Ante duda de librería/API, consultar documentación actual; no adivinar sintaxis.
