# 02 — Compilación y errores comunes (guía global)

## 1. Compilar según el tipo de proyecto

| Tipo | Comando típico (Windows) |
|---|---|
| Visual Studio (.sln) | Abrir `.sln` y compilar, o `msbuild proyecto.sln /t:Build /p:Configuration=Release` |
| CMake | `cmake -S . -B build && cmake --build build` |
| Make | `make` / `make -j` |
| Node/JS | `npm run build` / `npm run dev` |
| Python | `python -m py_compile archivo.py` o ejecutar directamente |
| Otros | Ver `NOTAS_AGENTE.md` del proyecto |

El agente **no compila** desde WSL salvo que el proyecto lo permita; el usuario compila y reporta el error.

## 2. Cómo reportar un error al agente

Incluir siempre:

1. Archivo y línea (ej.: `archivo.cpp(406,8)`).
2. Código de error (ej.: `C2672`, `C2065`, `error:`, `TS2304`).
3. Mensaje completo tal como aparece.
4. Qué cambio se hizo justo antes.

## 3. Errores frecuentes y solución

### MSVC C2672: 'función': no se encontró una función sobrecargada que coincida
**Causa típica:** mezclar variables nombradas (`const auto x = ...`) con temporales en plantillas/combines; la deducción de plantillas falla.
**Solución:** usar lambdas que devuelvan temporales:
```cpp
// Mal
const auto src = make_source(...);
combine(a, src, b)

// Bien
const auto makeSrc = [&]() { return make_source(...); };
combine(a, makeSrc(), b)
```

### C2065: identificador no declarado
Verificar que el símbolo esté declarado en el header incluido y que el namespace sea correcto.

### C2664 / C2440: no se puede convertir
Revisar tipos de parámetros y valores de retorno; usar el tipo exacto esperado.

### C2039 / C2065 en header
Confirmar que el símbolo existe en la versión del header incluido; verificar include y mayúsculas.

### LNK2019: símbolo externo sin resolver
La función está declarada pero no definida, o falta el .cpp en el proyecto.

### error: / TSxxxx (JS/TS)
Leer la línea indicada; verificar import/export, tipos y nombres exactos.

### error genérico de compilador
1. Ir a la línea indicada.
2. Releer el bloque editado (`sed -n 'X,Yp' archivo`).
3. Buscar el error exacto en la documentación del compilador/librería.
4. Si no se resuelve, reportar al usuario con el mensaje completo.

## 4. Verificación previa a compilar

- Releer el bloque editado.
- `grep` para confirmar que solo cambió lo previsto.
- Confirmar que no quedan referencias a símbolos eliminados.
- Respaldos `.bak` listos para restaurar (`04`).
