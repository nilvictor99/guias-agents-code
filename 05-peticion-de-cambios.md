# 05 — Cómo pedir cambios al agente (guía global)

## 1. Formato recomendado

1. **Objetivo**: qué se quiere lograr (una frase).
2. **Proyecto y ruta**: `/mnt/c/Users/VICTOR/code/<proyecto>/...` o `C:\...`.
3. **Archivo(s)**: ruta exacta si se conoce.
4. **Función/símbolo**: nombre concreto.
5. **Comportamiento esperado**: qué debe pasar tras el cambio.
6. **Restricciones**: qué NO tocar; si se usa git (no).
7. **Verificación**: cómo se prueba (compilar, abrir app, comando).

## 2. Ejemplo bueno

> En `<proyecto>/src/modulo.cpp`, en la función `crearLayout()`, agrega `|| type == Type::All` al chequeo de `goodType`. Solo eso. No tocar `<modulo Y>`. No usar git. Verificar releyendo el bloque.

## 3. Ejemplo malo

> Arregla el badge. (falta archivo, función, alcance y verificación)

## 4. Consejos

- Pegar el error de compilación completo (archivo, línea, código, mensaje).
- Indicar si el cambio ya existía (a veces HEAD ya lo tiene; verificar antes de editar).
- Pedir plan primero si el cambio es grande: "primero revisa y dame el plan".
- Confirmar alcance: "solo el módulo X, nada más".
- Al final, pedir confirmación de qué archivos quedaron intactos.
- Si el agente no llega a una ruta, pedir diagnóstico (`00`, `03`) antes de seguir.

## 5. Lo que el agente debe entregar

* Edit exacto aplicado (o texto buscar/reemplazar si no puede aplicarlo).
* Verificación realizada.
* Lista de archivos tocados e intactos.
* Confirmación de que no usó git.
