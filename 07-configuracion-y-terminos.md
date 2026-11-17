# 07 — Configuración del entorno y glosario (guía global)

## 1. Configuración WSL

`/etc/wsl.conf` (montaje automático, uid por defecto):
```ini
[automount]
enabled = true
root = /mnt/
options = "uid=1000,gid=1000,umask=022"

[network]
generateHosts = true
generateResolvConf = true
```
Aplicar cambios: `wsl --shutdown` y reabrir.

`C:\Users\VICTOR\.wslconfig` (recursos):
```ini
[wsl2]
memory=4GB
processors=4
swap=2GB
```

## 2. Variables y PATH

* Ver PATH: `echo $PATH`.
* `powershell.exe` NO está en PATH en este entorno; usar comandos WSL nativos.
* Para ejecutar algo de Windows, buscar el .exe real o usar `cmd.exe /c` si existe interop (verificar con `which cmd.exe`).

## 3. Montajes y permisos

* Unidades Windows: `/mnt/<letra>` (9p/drvfs).
* Propietario habitual: uid/gid 1000 (victor). Si falla escritura, montar con `uid=1000,gid=1000`.
* No usar `sudo` para editar archivos de proyecto; solo para montar/desmontar.

## 4. Rendimiento

* `/mnt` es más lento que ext4: copiar trabajo pesado a `/tmp/opencode` o `~/`.
* Compilar desde Windows (Visual Studio), no desde WSL.
* Evitar `grep -r` masivo sobre `/mnt/c` completo; acotar a la carpeta del proyecto.

## 5. Glosario

| Término | Significado |
|---|---|
| WSL / WSL2 | Subsistema de Windows para Linux; entorno Linux sobre Windows. |
| drvfs | Sistema que expone unidades Windows dentro de WSL (`/mnt/c`). |
| 9p | Protocolo de transferencia de archivos usado por WSL2 para /mnt. |
| mount / montaje | Vincular una unidad a una ruta del sistema de archivos. |
| I/O error | Error de entrada/salida; en /mnt suele ser corte transitorio del montaje. |
| uid/gid | Identificadores de usuario/grupo Linux (1000 = victor aquí). |
| umask | Máscara de permisos por defecto al crear archivos. |
| PATH | Lista de directorios donde se buscan ejecutables. |
| symlinkroot | Opción de drvfs que define dónde se resuelven symlinks. |
| `.sln` / `.vcxproj` | Solución y proyecto de Visual Studio (C++). |
| MSVC | Compilador de Microsoft Visual C++. |
| C2672 / C2065 / LNK2019 | Códigos de error típicos de MSVC (ver `02`). |
| rpl / combine | Librería de streams reactiva usada en tdesktop; `rpl::combine` une productores. |
| pacing / rate limit | Control de velocidad/ritmo de operaciones (ej.: reenvíos masivos). |
| token bucket / AIMD | Estrategias de control de tasa (se usaron y revertieron en apiwrap.cpp). |
| badge (DateBadge) | Indicador visual pequeño (ej.: fecha sobre miniaturas). |
| composite / unión | Módulo construido en cliente uniendo secuencias por tipo (ej.: All = Photo+Video+File). |
| materialización | Creación proactiva de ítems/ventanas de ids para carga dinámica al scroll. |
| scope / alcance | Conjunto de archivos/módulos que se permite modificar. |
| rollback | Volver al estado previo (con `.bak`). |
| interop | Capa WSL que permite ejecutar programas Windows desde Linux. |

## 6. Archivos de configuración clave (rutas)

* `/etc/wsl.conf` — configuración WSL del distro.
* `C:\Users\VICTOR\.wslconfig` — recursos de WSL2.
* `/etc/fstab` — montajes persistentes (no tocar sin necesidad).
* `NOTAS_AGENTE.md` (por proyecto) — alcance, compilación, pruebas.
