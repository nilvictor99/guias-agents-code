# 00 — Acceso a rutas de Windows desde WSL (guía global)

## 1. Mapeo universal

```
X:\carpeta\sub\archivo.ext   →   /mnt/x/carpeta/sub/archivo.ext
```

* Letra de unidad en minúscula tras `/mnt/`.
* Barras `\` → `/`.
* Espacios y caracteres especiales: entre comillas si hace falta.

| Windows | WSL |
|---|---|
| `C:\Users\VICTOR\code\<proyecto>` | `/mnt/c/Users/VICTOR/code/<proyecto>` |
| `C:\Users\VICTOR\Documents` | `/mnt/c/Users/VICTOR/Documents` |
| `D:\Proyectos\app` | `/mnt/d/Proyectos/app` |
| `E:\Datos\informe.pdf` | `/mnt/e/Datos/informe.pdf` |

Atajos:

```bash
cd /mnt/c/Users/VICTOR/code/<proyecto>   # entrar al proyecto
~                                        # home Linux (/home/victor), NO Windows
/mnt/c/Users/VICTOR                      # perfil de Windows
```

## 2. Verificar montajes

```bash
mount | grep -E 'drvfs|9p'        # unidades Windows montadas
grep -E 'drvfs|9p' /proc/mounts
df -h /mnt/c /mnt/d               # espacio y salud
ls /mnt/c                         # acceso por niveles
```

Salida sana (ejemplo):

```
C:\ on /mnt/c type 9p (rw,noatime,aname=drvfs;path=C:\;uid=1000;gid=1000;...)
C:\  894G  328G  566G  37% /mnt/c
```

Si falta una unidad:

```bash
sudo mkdir -p /mnt/d
sudo mount -t drvfs D: /mnt/d
```

## 3. Error `Input/output error`

**Síntomas:** `ls: cannot access '/mnt/...': Input/output error`; fallos en grep/read/edit.

**Causas típicas:** corte momentáneo 9p/drvfs; suspensión/hibernación de Windows; disco en standby; Docker Desktop/WSL2 reconfigurando montajes; antivirus; archivo bloqueado por otro proceso; recurso ocupado.

**Corrección (en orden):**

1. Reintentar tras segundos: `sleep 5; ls /mnt/c`.
2. Re-montar: `sudo umount /mnt/c && sudo mount -t drvfs C: /mnt/c`.
3. Si "target is busy": cerrar procesos que usen la ruta o reiniciar sesión WSL.
4. Desde Windows (PowerShell admin): `wsl --shutdown`, reabrir terminal.
5. Último recurso: reiniciar Windows.

**Diagnóstico:** `stat /mnt/c` · `ls -la /mnt` · `dmesg | tail -30` · `df -h /mnt/c`.

## 4. Problemas frecuentes y soluciones

| Problema | Causa | Solución |
|---|---|---|
| Archivo aparece en `/home/victor/` con nombre literal `C:\...` | El montaje falló al crearlo | Mover/recrear en `/mnt/...` una vez liberado el I/O |
| "Permission denied" en /mnt | uid/gid o metadata | Montar con `uid=1000,gid=1000,umask=022`; o `sudo chown -R victor:victort` (solo si es necesario) |
| No encuentra archivo que "existe" | Mayúsculas/minúsculas (WSL distingue) | Usar nombre exacto; verificar con `ls` |
| Ruta muy larga falla | Límite de Windows | Acortar nombres; activar rutas largas en Windows |
| Espacios en ruta | Shell los parte | Entre comillas: `"/mnt/c/Users/VICTOR/My App/file.cpp"` |
| Symlinks rotos | drvfs usa `symlinkroot=/mnt/` | Usar rutas reales, no enlaces cruzados |
| Lento al compilar/grep masivo | 9p es más lento que ext4 | Copiar a `/tmp/opencode` o `~/` para procesos pesados |
| Archivo cambia en Windows pero WSL ve viejo | Caché 9p | Cerrar/reabrir; `wsl --shutdown` |

## 5. Reglas de oro

- Cambios reales solo en `/mnt/...` (son los archivos de Windows). `/home/victor/` es solo borradores.
- Tras crear un archivo, verificar con `ls -la` que quedó en la ruta Windows esperada.
- Ante corte transitorio: esperar segundos y reintentar antes de alterar montajes.
- No editar archivos que estén abiertos/bloqueados en Windows.
