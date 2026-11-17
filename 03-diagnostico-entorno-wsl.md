# 03 — Diagnóstico del entorno WSL (guía global)

## 1. Rutas válidas

| Ruta | Qué es |
|---|---|
| `/mnt/c`, `/mnt/d`, `/mnt/e` | Unidades de Windows (reales; aquí se trabaja). |
| `/home/victor` (`~`) | Home de Linux (no visible desde Windows). |
| `/tmp/opencode` | Temporal aprobado para el agente. |

## 2. Herramientas

**Disponibles:** `read`, `grep`, `glob`, `edit`, `write`, `shell` (bash en WSL), `subagent` con `agent: "explore"`.

**No disponibles:** `powershell.exe` (no está en PATH); compilador de Visual Studio; git remoto (no push salvo permiso).

## 3. Verificación rápida del entorno

```bash
whoami; pwd                          # usuario y directorio
mount | grep drvfs                   # unidades Windows montadas
df -h /mnt/c                         # salud del disco
ls /mnt/c/Users/VICTOR/code          # acceso a proyectos
cat /proc/version                    # versión WSL/kernel
```

## 4. Problemas típicos y soluciones

| Problema | Solución |
|---|---|
| I/O error en /mnt | Ver `00-acceso-rutas-wsl.md` §3 (reintentar → re-montar → `wsl --shutdown`) |
| Archivo aparece en `/home/victor/` con nombre literal `C:\...` | El montaje falló al crearlo; mover/recrear en `/mnt/...` |
| Permisos denegados | Verificar uid/gid (1000:victor); montar con `uid=1000,gid=1000`; usar sudo solo para montar |
| Mayúsculas/minúsculas | WSL distingue, Windows no; usar nombres exactos |
| Disco lleno | `df -h`; liberar espacio en Windows |
| Rendimiento lento en /mnt | Copiar trabajo pesado a `/tmp/opencode` o `~/`; compilar en Windows |
| WSL se queda colgado | `wsl --shutdown` desde PowerShell admin y reabrir |
| Docker/WSL2 reconfigura montajes | Verificar `mount | grep drvfs` tras iniciar Docker |

## 5. Configuración recomendada (ver `07` para detalles)

`/etc/wsl.conf` (montaje automático, uid por defecto):
```ini
[automount]
enabled = true
options = "uid=1000,gid=1000,umask=022"
```

`C:\Users\VICTOR\.wslconfig` (recursos):
```ini
[wsl2]
memory=4GB
processors=4
```

## 6. Buenas prácticas

- Cambios reales solo en `/mnt/...`; `/home/victor/` para borradores.
- Verificar con `ls -la` que cada archivo creado quedó en la ruta Windows esperada.
- Ante corte transitorio: esperar y reintentar antes de alterar montajes.
- No editar archivos abiertos/bloqueados en Windows.
