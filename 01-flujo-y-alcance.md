# 01 — Flujo de trabajo y alcance (guía global)

## 1. Flujo estándar (siempre)

1. **Investigar**: `grep` (símbolos/texto), `glob` (patrones), `read` (rangos con offset/limit). No asumir sin leer.
2. **Planificar**: plan numerado breve antes de editar (3+ pasos o cambio significativo).
3. **Respaldar**: ver `04-respaldos-manuales.md`.
4. **Editar**: cambios quirúrgicos con `edit` (`oldString` exacto → `newString` mínimo). Nunca reescribir archivos enteros.
5. **Verificar**: releer el resultado (`06-validaciones-y-verificacion.md`).
6. **Resumir**: qué archivos se tocaron, qué quedó intacto, si se usó git (no).

## 2. Definir alcance por proyecto

Antes de editar, confirmar en `NOTAS_AGENTE.md` del proyecto (o preguntar):

* **Módulos/archivos permitidos** (ej.: solo módulo X).
* **Intocables** (ej.: módulo Y, reenvíos, contadores, configuración global).
* **Comportamiento esperado** del cambio.
* **Cómo se valida** (compilar, abrir app, comando de prueba).

Si el archivo a editar no está en el alcance permitido → **preguntar antes**, no editar.

## 3. Reglas firmes

- **No tocar git** (commit/push/branch) salvo petición explícita del usuario.
- No añadir comentarios al código salvo que se pida.
- No volcar de nuevo contenido ya mostrado en outputs; referenciar `archivo:línea`.
- Si un edit falla por texto no encontrado, ajustar `oldString` con más contexto (no adivinar).
- Ante duda sobre una librería/API, consultar documentación actual en vez de inventar sintaxis.
- Archivos desconocidos o cambios ajenos: tratar como trabajo del usuario; investigar antes de borrar/sobrescribir.

## 4. Reporte final esperado

Indicar siempre:

1. Archivos tocados (ruta + función).
2. Qué se dejó intacto y por qué.
3. Verificación realizada (relectura, grep, compilación si aplica).
4. Si se usó git (no, salvo permiso).
5. Pasos siguientes o pendientes.

## 5. Ejemplo de alcance bien definido

> "Solo modificar `<módulo X>` en `<proyecto>`. Mantener `<módulo Y>` exactamente igual. No tocar git. Verificar releyendo el bloque editado."
