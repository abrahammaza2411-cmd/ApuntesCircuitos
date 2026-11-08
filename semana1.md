# Semana 1: Sistemas Numéricos, Códigos y Compuertas Lógicas

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de Lógica Combinacional con Diseño Clásico

---

## Objetivos de la semana

Al terminar esta semana se debe ser capaz de:

- Explicar por qué los computadores usan lógica binaria y cómo se relacionan los voltajes con los estados `0` y `1`.
- Calcular márgenes de ruido a partir de las especificaciones de la familia 74LS.
- Convertir números entre decimal, binario, octal y hexadecimal.
- Representar enteros con signo en complemento a dos, restar sumando y detectar desbordamiento.
- Distinguir los códigos BCD, Gray y ASCII y saber para qué sirve cada uno.
- Conocer las 7 compuertas lógicas (tabla de verdad, expresión y símbolo).
- Leer la hoja de datos de un circuito integrado 74LS (pines, encapsulado y nomenclatura).

---

## 1. Señales digitales y margen de ruido

### 1.1 ¿Por qué binario?

En el nivel más bajo del hardware, la información son **voltajes**. Las señales analógicas continuas se degradan por atenuación y ruido electromagnético. En cambio, un transistor usado como **interruptor** solo tiene dos estados:

| Estado del transistor | Región | Nivel lógico típico |
|---|---|---|
| Interruptor abierto | Corte | BAJO (`0`) |
| Interruptor cerrado | Saturación | ALTO (`1`) |

Basta con distinguir dos niveles bien separados, lo que vuelve al sistema **robusto frente a perturbaciones**.

### 1.2 Rangos de voltaje de la familia 74LS (TTL, alimentación de +5 V)

Un nivel lógico no es un voltaje exacto, sino un **rango garantizado por el fabricante**. Hay que distinguir lo que **garantiza una salida** de lo que **exige una entrada**:

| Parámetro | Símbolo | Condición | Quién lo define |
|---|---|---|---|
| Salida en ALTO | $V_{OH}$ | $\ge 2.7\ \text{V}$ | La compuerta emisora garantiza al menos este voltaje |
| Entrada en ALTO | $V_{IH}$ | $\ge 2.0\ \text{V}$ | La compuerta receptora exige al menos este voltaje |
| Entrada en BAJO | $V_{IL}$ | $\le 0.8\ \text{V}$ | La compuerta receptora exige como máximo este voltaje |
| Salida en BAJO | $V_{OL}$ | $\le 0.5\ \text{V}$ | La compuerta emisora garantiza como máximo este voltaje |

```text
 5.0 V ─┬─────────────────────────────────────
        │  Zona ALTO     (salida ≥ 2.7 V, entrada ≥ 2.0 V)
 2.7 V ─┤  ← V_OH        │ NM_H = 0.7 V
 2.0 V ─┤  ← V_IH        ┘
        │
        │  ZONA INDEFINIDA (0.8 V a 2.0 V): lectura NO garantizada
        │
 0.8 V ─┤  ← V_IL        ┐
 0.5 V ─┤  ← V_OL        │ NM_L = 0.3 V
        │  Zona BAJO     (entrada ≤ 0.8 V, salida ≤ 0.5 V)
 0.0 V ─┴─────────────────────────────────────
```

**Zona indefinida (0.8 V a 2.0 V):** el chip no garantiza si leerá `0` o `1`. Puede oscilar, dar estados erráticos o aumentar el consumo de corriente. Se llega a ella por ruido excesivo, acoplamiento deficiente entre etapas o por dejar una **entrada flotante** (sin conectar). En TTL una entrada flotante suele comportarse como un `1`, pero eso **no está garantizado** y nunca debe aprovecharse.

### 1.3 Margen de ruido

El **margen de ruido** es la perturbación máxima que una señal válida puede sufrir en el trayecto y aun así ser interpretada correctamente por el receptor. Es la diferencia entre lo que el emisor garantiza y lo que el receptor exige:

$$\text{NM}_H = V_{OH} - V_{IH} = 2.7\ \text{V} - 2.0\ \text{V} = 0.7\ \text{V}$$

$$\text{NM}_L = V_{IL} - V_{OL} = 0.8\ \text{V} - 0.5\ \text{V} = 0.3\ \text{V}$$

> **Ejemplo de falla por ruido.** Una compuerta emite un BAJO de $0.5\ \text{V}$ y la línea pasa cerca de un motor que induce un pico de $0.4\ \text{V}$. En la entrada de la siguiente compuerta llegan $0.5 + 0.4 = 0.9\ \text{V}$. Como $0.9\ \text{V} > V_{IL} = 0.8\ \text{V}$, la señal entra en la zona indefinida y el receptor **ya no garantiza** leer un `0`, de modo que el sistema puede fallar.

Por eso el margen de nivel BAJO (0.3 V) es el más delicado de la familia 74LS.

---

## 2. Sistemas numéricos posicionales

En un sistema **posicional** el valor de cada dígito depende de su posición. En uno no posicional (por ejemplo, el romano) cada símbolo vale lo mismo en cualquier lugar. Un número de base $b$ se evalúa como suma de dígito × $b^{\text{posición}}$.

### 2.1 Sistema binario

- **Base 2**, símbolos $\{0, 1\}$.
- **Bit** (*binary digit*): unidad mínima de información. **Nibble:** 4 bits. **Byte:** 8 bits.
- **LSB:** bit menos significativo (derecha). **MSB:** bit más significativo (izquierda).

| Potencia | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ |
|---|---|---|---|---|---|---|---|---|
| Peso | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

**Binario a decimal:**

$$1011_2 = 1\cdot2^3 + 0\cdot2^2 + 1\cdot2^1 + 1\cdot2^0 = 8 + 0 + 2 + 1 = 11_{10}$$

### 2.2 Decimal a binario (divisiones sucesivas)

Se divide entre 2 hasta llegar a cociente 0. Los **residuos** se leen **de abajo hacia arriba**.

**Ejemplo: $25_{10}$**

| División | Cociente | Residuo |
|---|---|---|
| $25 \div 2$ | 12 | **1** (LSB) |
| $12 \div 2$ | 6 | 0 |
| $6 \div 2$ | 3 | 0 |
| $3 \div 2$ | 1 | 1 |
| $1 \div 2$ | 0 | **1** (MSB) |

$$25_{10} = 11001_2 \qquad \text{Comprobación: } 16 + 8 + 1 = 25$$

### 2.3 Hexadecimal y octal como abreviaturas del binario

Como $16 = 2^4$ y $8 = 2^3$, un dígito hexadecimal resume exactamente **4 bits** y uno octal resume **3 bits**.

| Decimal | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Binario | 0000 | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 | 1101 | 1110 | 1111 |
| Hexadecimal | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |

**Procedimiento:**

1. Agrupar los bits **de derecha a izquierda** (de 4 en 4 para hex, de 3 en 3 para octal).
2. Completar con ceros a la izquierda el último grupo si hace falta.
3. Sustituir cada grupo por su símbolo.

**Ejemplo: $157_{10} = 10011101_2$**

- Hexadecimal: `1001 | 1101` → `9 | D` → $9\text{D}_{16}$
- Octal: `010 | 011 | 101` → `2 | 3 | 5` → $235_8$ (se añadió un cero a la izquierda)

Comprobación: $9\cdot16 + 13 = 157$ y $2\cdot64 + 3\cdot8 + 5 = 157$.

---

## 3. Aritmética binaria y complemento a dos

### 3.1 Suma binaria

| Operación | Suma | Acarreo |
|---|---|---|
| $0+0$ | 0 | 0 |
| $0+1$ | 1 | 0 |
| $1+0$ | 1 | 0 |
| $1+1$ | 0 | 1 |
| $1+1+1$ | 1 | 1 |

**Ejemplo: $11 + 6 = 17$**

```text
 acarreos:  1 1 1
              1 0 1 1   (11)
          +   0 1 1 0   (6)
          ───────────
            1 0 0 0 1   (17)
```

### 3.2 Complemento a dos

El hardware no tiene un símbolo "−"; todo se codifica con `0` y `1`. El **complemento a dos** representa enteros con signo en un ancho de palabra fijo y permite **restar usando el mismo sumador**.

**Para obtener el negativo de un número:**

1. **Invertir** todos los bits (complemento a uno).
2. **Sumar 1**.

**Ejemplo (4 bits): obtener $-3$ desde $+3$**

```text
+3        = 0011
invertir  = 1100
sumar 1   = 1100 + 0001 = 1101   →  -3
```

> **Atajo:** recorriendo desde la derecha, se copian los bits hasta el primer `1` inclusive y se invierten todos los demás. Para `0011`: se copia `1` y se invierten `001` → `1101`. ✔

**Otro ejemplo (8 bits): $-5$.** $5 = 00000101$ → invertido `11111010` → $+1$ = `11111011`.

### 3.3 Rango de representación

El MSB funciona como **bit de signo**: `0` es positivo y `1` es negativo. Con $n$ bits el rango es:

$$-2^{\,n-1} \ \le\ N\ \le\ +2^{\,n-1} - 1$$

| $n$ | Rango |
|---|---|
| 4 bits | $-8$ a $+7$ |
| 8 bits | $-128$ a $+127$ |

| Código | Valor | Código | Valor |
|---|---|---|---|
| 0000 | 0 | 1000 | −8 |
| 0001 | +1 | 1001 | −7 |
| 0010 | +2 | 1010 | −6 |
| 0011 | +3 | 1011 | −5 |
| 0100 | +4 | 1100 | −4 |
| 0101 | +5 | 1101 | −3 |
| 0110 | +6 | 1110 | −2 |
| 0111 | +7 | 1111 | −1 |

Observación: el rango es **asimétrico** (hay un negativo más que positivos) y existe un único cero.

### 3.4 Resta como suma

La resta $A - B$ se calcula como $A + (-B)$. El acarreo que sale por el MSB se **descarta**. Así no hace falta un circuito restador independiente: basta el sumador más la lógica que forma $-B$.

**Ejemplo: $5 - 3 = 2$ (4 bits)**

```text
   0101   (+5)
 + 1101   (-3)
 ──────
 1 0010   → se descarta el acarreo del MSB → 0010 = +2  ✔
```

### 3.5 Desbordamiento (*overflow*)

Ocurre cuando el resultado **no cabe** en el rango del ancho de palabra: el bit de signo se corrompe y el resultado es erróneo.

**Ejemplo (4 bits): $7 + 1$**

```text
   0111   (+7)
 + 0001   (+1)
 ──────
   1000   → se interpreta como -8  ✘  (+8 no cabe en 4 bits)
```

**Criterio de detección:** hay overflow cuando se suman dos números del **mismo signo** y el resultado tiene **signo distinto**. (Sumar números de signo opuesto nunca produce overflow.) Equivale a que el acarreo que **entra** al MSB sea distinto del que **sale** de él. En $7+1$ entra un acarreo al MSB y no sale ninguno (overflow); en $5-3$ entra uno y sale uno (sin overflow).

> **Conexión con software.** Los enteros con signo de 32 bits (`int` en C o Java, rango $-2\,147\,483\,648$ a $+2\,147\,483\,647$) se comportan igual:
>
> ```java
> int x = 2147483647;
> x = x + 1;   // x pasa a valer -2147483648 por desbordamiento
> ```

---

## 4. Códigos de representación: BCD, Gray y ASCII

Un **código** es una convención para asignar patrones de bits a información. No todos son numéricos ni ponderados.

### 4.1 BCD (Decimal Codificado en Binario, 8421)

Cada **dígito decimal** (0 a 9) se codifica por separado con 4 bits.

- **Uso:** interfaz con displays de 7 segmentos, calculadoras y contadores decimales.
- **Combinaciones inválidas:** $1010_2$ a $1111_2$ (10 a 15) **no representan ningún dígito**. Son 6 de las 16 combinaciones posibles; más adelante se tratarán como condiciones de "no importa".

**Comparación con $157_{10}$:**

| Representación | Resultado |
|---|---|
| Binario puro | `10011101` (8 bits) |
| BCD | `0001 0101 0111` (12 bits: dígitos 1, 5, 7) |

BCD usa más bits, pero convertir a visualización decimal es inmediato. **No debe confundirse con el binario puro**: $0001\,0101\,0111_{\text{BCD}} \neq 10011101_2$.

### 4.2 Código Gray

Código **no ponderado** en el que dos valores consecutivos difieren en **un solo bit**.

**Problema que resuelve.** En binario natural, pasar de 3 (`011`) a 4 (`100`) cambia **los tres bits a la vez**. En un encoder rotativo real los sensores no conmutan en el mismo instante, así que durante la transición pueden leerse valores falsos (*glitches*), por ejemplo `011 → 001 → 100`. Con Gray, de 3 (`010`) a 4 (`110`) cambia **solo un bit** y el error de lectura espurio no puede ocurrir.

| Decimal | Binario natural | Código Gray | Bit que cambia respecto al anterior |
|---|---|---|---|
| 0 | 000 | 000 | — |
| 1 | 001 | 001 | bit 0 |
| 2 | 010 | 011 | bit 1 |
| 3 | 011 | 010 | bit 0 |
| 4 | 100 | 110 | bit 2 |
| 5 | 101 | 111 | bit 0 |
| 6 | 110 | 101 | bit 1 |
| 7 | 111 | 100 | bit 0 |

**Construcción por reflejo (espejo):**

```text
1 bit:      0 1
reflejar:   0 1 | 1 0
prefijar:   00 01 | 11 10          ← secuencia de 2 bits: 00, 01, 11, 10
reflejar:   00 01 11 10 | 10 11 01 00
prefijar:   000 001 011 010 | 110 111 101 100   ← secuencia de 3 bits
```

Se antepone `0` a la primera mitad y `1` a la mitad reflejada.

**Conversión por fórmula** (usa la operación XOR, $\oplus$, que se define en la sección 5):

- Binario → Gray: $g_{n-1} = b_{n-1}$ y $g_i = b_{i+1} \oplus b_i$. Ejemplo: $100_2 \to 100 \oplus 010 = 110_{\text{Gray}}$.
- Gray → Binario: $b_{n-1} = g_{n-1}$ y $b_i = b_{i+1} \oplus g_i$.

> La secuencia `00, 01, 11, 10` es justamente el orden en que se rotulan filas y columnas de los **Mapas de Karnaugh**, que se estudiarán más adelante.

### 4.3 ASCII (*American Standard Code for Information Interchange*)

Codifica caracteres alfanuméricos y comandos de control con **7 bits** ($2^7 = 128$ combinaciones), almacenados normalmente en un byte.

| Carácter | Decimal | Hex | Binario (8 bits) |
|---|---|---|---|
| `0` | 48 | 30 | 00110000 |
| `A` | 65 | 41 | 01000001 |
| `a` | 97 | 61 | 01100001 |

- Una letra minúscula se diferencia de su mayúscula en un solo bit (el de peso 32).
- Al tener solo 128 posiciones, el ASCII estándar **no incluye** la "ñ" ni las tildes. Para eso existen extensiones como **UTF-8**.

### 4.4 Resumen comparativo

| Código | Ponderado | Bits | Aplicación principal |
|---|---|---|---|
| Binario natural | Sí | $n$ | Aritmética |
| BCD | Sí (8-4-2-1) | 4 por dígito | Displays y contadores decimales |
| Gray | No | $n$ | Encoders y sensores de posición |
| ASCII | No | 7 | Texto y caracteres de control |

---

## 5. Compuertas lógicas fundamentales

Las compuertas son los bloques básicos de la electrónica digital. Implementan las funciones del álgebra de Boole: reciben una o más entradas lógicas y producen **una** salida según una regla fija.

### 5.1 Compuerta NOT (inversor)

Entrega el complemento de la entrada: $Y = A'$ (también $\overline{A}$).

| $A$ | $Y$ |
|---|---|
| 0 | 1 |
| 1 | 0 |

### 5.2 Compuertas de dos entradas

| Compuerta | Expresión | Regla de la salida |
|---|---|---|
| **AND** | $Y = A \cdot B$ | Vale 1 **solo si todas** las entradas son 1 |
| **OR** | $Y = A + B$ | Vale 1 si **al menos una** entrada es 1 |
| **NAND** | $Y = (A \cdot B)'$ | AND invertida: vale 0 **solo si todas** las entradas son 1 |
| **NOR** | $Y = (A + B)'$ | OR invertida: vale 1 **solo si ninguna** entrada es 1 |
| **XOR** | $Y = A \oplus B$ | Vale 1 si las entradas son **distintas** |
| **XNOR** | $Y = (A \oplus B)'$ | Vale 1 si las entradas son **iguales** |

**Tabla de verdad conjunta:**

| $A$ | $B$ | AND | OR | NAND | NOR | XOR | XNOR |
|---|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 |

**Notas útiles:**

- **XOR** funciona como un **detector de diferencia** (por eso se usa en comparadores) y equivale a una suma módulo 2. Ejemplo físico: la luz de una escalera con dos interruptores, donde cambiar cualquiera invierte el estado de la lámpara.
- **XNOR** es un **detector de igualdad**.
- Con más de dos entradas, AND y OR siguen las mismas reglas (todas en 1 / al menos una en 1).
- **NAND** y **NOR** se llaman **universales**: cualquier circuito combinacional puede construirse usando solo uno de los dos tipos.

### 5.3 Normas de simbología

| Norma | Representación | Característica |
|---|---|---|
| **ANSI/IEEE Std 91-1984** | Formas distintivas | AND: entrada plana y salida curva. OR: entrada curva y salida en punta. Es la que se usa en esta asignatura |
| **IEC 60617-12** | Rectángulos con un símbolo interno | NOT: `1`. AND/NAND: `&`. OR/NOR: `≥1`. XOR/XNOR: `=1` |

En ambas normas, la inversión (NOT, NAND, NOR, XNOR) se marca con una **burbuja** (círculo de negación) en la terminal de salida.

---

## 6. Circuitos integrados y hojas de datos (serie 74LS08)

Las compuertas se materializan en **circuitos integrados** (chips) de la familia 74. Para usarlos hay que saber leer su *datasheet* y conocer la geometría del encapsulado.

### 6.1 Encapsulado DIP-14

*Dual In-line Package* de 14 pines: dos filas paralelas de 7 pines separados 0.1 pulgadas (compatible con protoboard).

```text
                 ┌───╮ ╭───┐   (muesca)
        1A  ──  1 │       │ 14 ──  VCC (+5 V)
        1B  ──  2 │       │ 13 ──  4B
        1Y  ──  3 │       │ 12 ──  4A
        2A  ──  4 │ 74LS08│ 11 ──  4Y
        2B  ──  5 │       │ 10 ──  3B
        2Y  ──  6 │       │  9 ──  3A
  GND (0 V) ──  7 │       │  8 ──  3Y
                  └───────┘
              Cuádruple AND de 2 entradas
```

**Cómo contar los pines:**

1. Sostener el chip con la **muesca** semicircular hacia arriba.
2. El **pin 1** está en la esquina superior izquierda, junto a la muesca.
3. Se cuenta en **sentido antihorario**: del 1 al 7 bajando por la izquierda; del 8 al 14 subiendo por la derecha.

### 6.2 Asignación de pines del 74LS08

| Función | Pines |
|---|---|
| Alimentación $V_{CC}$ (+5 V) | 14 |
| Tierra GND (0 V) | 7 |
| Compuerta 1 | Entradas 1 y 2, salida 3 |
| Compuerta 2 | Entradas 4 y 5, salida 6 |
| Compuerta 3 | Entradas 9 y 10, salida 8 |
| Compuerta 4 | Entradas 12 y 13, salida 11 |

**Buenas prácticas de conexión:**

- Alimentar siempre $V_{CC}$ y GND. La familia 74LS opera con $+5\ \text{V}$ (aprox. 4.75 V a 5.25 V).
- **No dejar entradas sin conectar:** llevar las entradas sobrantes a `0` o `1` definido (GND, o $V_{CC}$ mediante una resistencia).
- Si una compuerta del chip no se usa, sus **entradas** también deben quedar en un nivel definido (su salida puede dejarse libre).

### 6.3 Otros chips de compuertas de uso común

| Circuito | Contenido | Observación de pines |
|---|---|---|
| 74LS00 | 4 NAND de 2 entradas | Mismo patillaje que el 74LS08 |
| 74LS02 | 4 NOR de 2 entradas | **Distinto**: las salidas están en los pines 1, 4, 10 y 13 |
| 74LS04 | 6 NOT (inversores) | Distinto: una entrada y una salida por compuerta |
| 74LS08 | 4 AND de 2 entradas | Referencia de esta semana |
| 74LS32 | 4 OR de 2 entradas | Mismo patillaje que el 74LS08 |
| 74LS86 | 4 XOR de 2 entradas | Mismo patillaje que el 74LS08 |

> Siempre se debe verificar el patillaje en el *datasheet* del chip concreto: no todos los chips comparten la distribución del 74LS08.

### 6.4 Nomenclatura del fabricante

Ejemplo: **SN 74 LS 08 N**

| Código | Significado |
|---|---|
| **SN** | Prefijo del fabricante (Texas Instruments) |
| **74** | Rango comercial de temperatura: 0 °C a 70 °C. El prefijo **54** es de grado militar (−55 °C a 125 °C) |
| **LS** | Tecnología: *Low-power Schottky* (bipolar TTL de bajo consumo y buena velocidad). Con **HC** (*High-speed CMOS*) la función y los pines se mantienen, pero la tecnología es CMOS, de menor consumo estático |
| **08** | Función lógica: cuatro AND de 2 entradas |
| **N** | Encapsulado: DIP plástico de inserción (*through-hole*) |

---

## Resumen de la semana

1. Los voltajes se abstraen en dos estados, delimitados por $V_{OH}$, $V_{IH}$, $V_{IL}$ y $V_{OL}$. En 74LS los márgenes de ruido son **0.7 V** (ALTO) y **0.3 V** (BAJO), y la franja de 0.8 V a 2.0 V es la zona indefinida.
2. Las conversiones entre binario, decimal, octal y hexadecimal se hacen con pesos, divisiones sucesivas y agrupación de bits (4 para hex, 3 para octal).
3. El **complemento a dos** (invertir y sumar 1) permite restar con un sumador. El rango es $-2^{n-1}$ a $2^{n-1}-1$ y el **overflow** aparece cuando dos operandos del mismo signo dan un resultado de signo contrario.
4. **BCD** (dígito a dígito, 6 combinaciones inválidas), **Gray** (cambio de un solo bit entre valores consecutivos) y **ASCII** (texto en 7 bits) resuelven necesidades de interfaz.
5. Las **7 compuertas** (NOT, AND, OR, NAND, NOR, XOR, XNOR) tienen tablas de verdad y símbolos ANSI/IEEE e IEC. NAND y NOR son universales.
6. El **74LS08** es un DIP-14 con cuatro AND; $V_{CC}$ en el pin 14 y GND en el pin 7.

---

## Autoevaluación

1. Convierta $202_{10}$ a binario y a hexadecimal.
2. Represente $-6$ en complemento a dos con 4 bits.
3. Calcule $6 + 5$ en 4 bits con signo. ¿Hay overflow?
4. Convierta $101_2$ a código Gray.
5. Una salida BAJA de $0.5\ \text{V}$ recibe $0.2\ \text{V}$ de ruido. ¿La entrada receptora lo lee correctamente? ¿Y con $0.4\ \text{V}$?

<details>
<summary>Ver respuestas</summary>

1. $202 = 128 + 64 + 8 + 2 = 11001010_2$, que agrupado `1100 | 1010` da $\text{CA}_{16}$.
2. $+6 = 0110$ → invertido `1001` → $+1$ = **1010**.
3. $0110 + 0101 = 1011$, que se lee como $-5$. **Sí hay overflow**: dos positivos dieron un negativo ($+11$ no cabe en 4 bits).
4. $101 \oplus 010 = $ **111** en Gray.
5. Con $0.2\ \text{V}$: $0.7\ \text{V} \le 0.8\ \text{V}$, lectura correcta. Con $0.4\ \text{V}$: $0.9\ \text{V} > 0.8\ \text{V}$, zona indefinida, lectura no garantizada.

</details>

---
