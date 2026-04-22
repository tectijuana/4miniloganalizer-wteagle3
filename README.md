# Práctica 1 - Mini Cloud Log Analyzer en ARM64

##  Autor

Fidel Muñoz 22211619

---

##  Descripción

En esta práctica se implementó un analizador de logs en lenguaje ensamblador ARM64, capaz de procesar códigos de estado HTTP leídos desde la entrada estándar (stdin).

El programa analiza los códigos y permite detectar condiciones específicas dependiendo de la variante asignada.

---

##  Variante implementada

### Variante C: Detección del primer evento crítico (503)

El objetivo de esta variante es detectar la primera aparición del código HTTP **503 (Service Unavailable)** dentro del flujo de entrada.

---

##  Lógica implementada

El programa funciona de la siguiente manera:

1. Lee la entrada estándar (`stdin`) en bloques de datos.
2. Procesa cada byte leído.
3. Si el byte es un dígito, construye un número entero acumulado.
4. Cuando encuentra un salto de línea (`\n`), interpreta el número completo como un código HTTP.
5. Se compara el número con el valor **503**.
6. Si el valor es 503:

   * Se imprime un mensaje indicando el evento crítico.
   * El programa termina inmediatamente.
7. Si no es 503:

   * Continúa procesando la entrada normalmente.

---
##  Implementación en ARM64

La detección del evento crítico se implementó utilizando:

* `cmp` → para comparar el valor leído con 503
* `b.ne` → para continuar si no es igual
* `b` → para terminar la ejecución cuando se detecta el evento
* `write syscall` → para imprimir el mensaje en pantalla

Fragmento clave:

```asm
cmp x22, #503
b.ne continuar

adrp x0, msg_critico
add x0, x0, :lo12:msg_critico
bl write_cstr

b salida_ok
```

---

##  Compilación

```bash
make
```

---

##  Ejecución

```bash
cat data/logs_*.txt | ./run.sh
```

---

##  Ejemplo de salida

```
Primer evento crítico detectado: 503
```

---

##  Evidencia de ejecución

Grabación realizada con asciinema:

 (https://asciinema.org/a/M3MLNRZdYEfchq2T)


---

##  Pruebas

Se ejecutaron pruebas para validar el correcto funcionamiento del programa:

```bash
make test
```

---

##  Estructura del proyecto

```
cloud-log-analyzer/
│
├── src/
│   └── analyzer.s
├── data/
│   └── logs_*.txt
├── tests/
├── run.sh
├── Makefile
└── README.md
```

---

## Restricciones cumplidas

✔ Implementación en ARM64 Assembly
✔ Uso exclusivo de syscalls Linux
✔ No se utilizó C ni Python
✔ Se respetó la variante asignada

---

##  Conclusión

Esta práctica permitió comprender cómo procesar datos a bajo nivel utilizando instrucciones ARM64, implementando lógica de análisis de eventos directamente sobre registros y memoria.

Se logró detectar correctamente el primer evento crítico dentro del flujo de datos, replicando un comportamiento típico de sistemas de monitoreo en la nube.

---
