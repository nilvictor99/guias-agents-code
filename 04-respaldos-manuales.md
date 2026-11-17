# 04 — Respaldos manuales (sin git) (guía global)

Como no se usa git, respaldar antes de editar.

## 1. Respaldo por archivo (recomendado)

```bash
f=/mnt/c/Users/VICTOR/code/<proyecto>/ruta/archivo.cpp
cp "$f" "$f.bak"
# o con sello de fecha:
cp "$f" "$f.bak.$(date +%Y%m%d-%H%M%S)"
```

## 2. Respaldo por carpeta (cambios grandes)

```bash
cp -r /mnt/c/Users/VICTOR/code/<proyecto>/modulo \
      /mnt/c/Users/VICTOR/code/<proyecto>/modulo.bak
```

## 3. Restaurar

```bash
cp archivo.cpp.bak archivo.cpp
# o
cp -r modulo.bak modulo
```

## 4. Verificar el respaldo antes de editar

```bash
ls -la archivo.cpp archivo.cpp.bak    # mismo tamaño
diff archivo.cpp archivo.cpp.bak      # sin diferencias al inicio
```

## 5. Cuándo respaldar es obligatorio

* Edit que toca más de 10 líneas.
* Cambios en lógica central (unión, materialización, contadores, rutas).
* Cambios de constantes globales (límites, tiempos, flags).
* Cualquier cambio que el usuario no pidió explícitamente (no hacerlo).

## 6. Política de limpieza

* Un `.bak` por sesión de edición; no acumular.
* Eliminar `.bak` tras validación exitosa (o mover a `~/backups/<fecha>/<proyecto>/`).
* Los `.bak` no deben quedar dentro de rutas que el compilador/empaquetado incluya como código.

## 7. Reglas

- Respaldar SIEMPRE antes del primer edit del día en cada archivo.
- Nunca editar sin respaldo disponible.
- Nunca borrar un `.bak` sin confirmar que el cambio quedó validado (`06`).
