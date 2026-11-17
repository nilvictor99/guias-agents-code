# 06 — Validaciones y verificación (guía global)

## 1. Verificar sin git (no hay diff)

1. **Releer el bloque editado**:
   ```bash
   sed -n 'X,Yp' /mnt/c/Users/VICTOR/code/<proyecto>/ruta/archivo.cpp
   ```
2. **Confirmar que el cambio esperado está**:
   ```bash
   grep -n "símbolo_o_constante" archivo.cpp
   ```
3. **Confirmar que no hay duplicados/rotura de sintaxis**: buscar llaves, comas, paréntesis alrededor del edit.
4. **Confirmar alcance**: verificar que otros archivos mencionados en el plan NO cambiaron (releerlos o `grep` de sus constantes clave).

## 2. Verificación de archivos tocados

Listar archivos modificados recientemente en el proyecto:

```bash
find /mnt/c/Users/VICTOR/code/<proyecto> -name "*.cpp" -newer /tmp/opencode/marca \
  -not -path "*/out/*" 2>/dev/null
```
(Crear marca antes de editar: `touch /tmp/opencode/marca`.)

## 3. Criterios de aceptación

* El edit aplicado coincide con el plan (texto exacto, ubicación exacta).
* Solo cambió lo previsto; el resto del archivo y los demás módulos intactos.
* No se usó git.
* El usuario compila/prueba y reporta resultado; si hay error, se corrige con `02`.

## 4. Plan de rollback

* Si el edit sale mal: restaurar `.bak` (`04`), reintentar con `oldString` más preciso.
* Si no hay `.bak`: reconstruir el bloque original a partir de la última lectura confirmada.
* Nunca dejar un archivo a medio editar: si falla, volver al estado previo antes de reportar.

## 5. Validaciones extra según tipo de cambio

| Cambio | Validación extra |
|---|---|
| UI/badge/visual | Confirmar que el widget se crea y pinta en el flujo correcto (constructor, paint, update). |
| Constantes (límites/tiempos) | Buscar otros usos de la constante; confirmar que no afecta módulos no deseados. |
| Lógica de unión/materialización | Verificar orden, dedupe y contadores asociados. |
| Configuración/entorno | Verificar montaje y permisos tras el cambio (`03`). |

## 6. Reporte de validación

Al final, indicar: qué se verificó (relectura, grep, compilación del usuario) y resultado. Si no se pudo verificar algo, decirlo explícitamente.
