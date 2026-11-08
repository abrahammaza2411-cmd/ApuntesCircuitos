# Semana 4: Diseño Combinacional y Bloques Aritméticos

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de Lógica Combinacional con Diseño Clásico

---

## Objetivos de la semana

Al terminar esta semana se debe ser capaz de:

- Aplicar el procedimiento estándar de diseño combinacional, también para circuitos con **varias salidas**.
- Diseñar e interpretar el **medio sumador** y el **sumador completo**, y encadenarlos en un sumador paralelo.
- Implementar el **desbordamiento** (*overflow*) con compuertas.
- Diseñar **restadores** y, sobre todo, un **sumador/restador** en complemento a dos.
- Diseñar **comparadores de magnitud**.
- Probar los circuitos en simulador y con chips TTL (práctica APE 04).

> **Conocimientos previos:** complemento a dos, regla de detección de overflow y compuertas XOR/XNOR (Semana 1); minitérminos y simplificación (Semana 2); Mapas de Karnaugh y uso de compuertas universales (Semana 3). Aquí solo se aplican.

---

## 1. Procedimiento de diseño combinacional

Un circuito **combinacional** es aquel cuyas salidas dependen **solo del valor actual de las entradas**. El diseño sigue cinco pasos:

| Paso | Qué se hace |
|---|---|
| 1. Especificación | Describir con claridad y sin ambigüedad lo que debe hacer el circuito |
| 2. Asignación de variables | Definir las $n$ entradas y las $m$ salidas, con nombre y significado de `0`/`1` |
| 3. Tabla de verdad | Dar el valor de **cada salida** para las $2^n$ combinaciones de entrada |
| 4. Funciones y simplificación | Escribir cada salida en SOP o POS y minimizarla (álgebra o K-Map) |
| 5. Esquema | Dibujar el circuito con símbolos ANSI/IEEE |

**Si hay varias salidas** ($m > 1$), la tabla de verdad tiene una columna por salida y cada una se simplifica **por separado** (un K-Map por salida). Después se buscan términos comunes que se puedan **compartir** entre salidas para ahorrar compuertas.

Menos literales y menos compuertas implican menos transistores en serie, menos consumo y menor **retardo de propagación** ($t_{pd}$), que es el tiempo que tarda un cambio en las entradas en reflejarse en la salida.

---

## 2. Sumadores

La suma es la operación base de la aritmética digital: la resta, la comparación y otras operaciones se construyen a partir de ella.

### 2.1 Medio sumador (*Half Adder*, HA)

Suma **dos bits** $A$ y $B$ y produce la **suma** $S$ y el **acarreo** $C_{out}$. No admite acarreo de una etapa anterior.

*Paso 1 y 2:* entradas $A$, $B$; salidas $S$ (bit de suma) y $C_{out}$ (acarreo).

*Paso 3, tabla de verdad* (es la suma binaria de la Semana 1):

| $A$ | $B$ | $S$ | $C_{out}$ |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

*Paso 4, ecuaciones:*

$$S = A'B + AB' = A \oplus B \qquad\qquad C_{out} = A \cdot B$$

*Paso 5, esquema:* una XOR y una AND que comparten las mismas entradas.

```text
        ┌───────┐
 A ──●──┤  XOR  ├── S = A ⊕ B
     │  │       │
 B ──┼●─┤       │
     ││ └───────┘
     ││ ┌───────┐
     │└─┤  AND  ├── Cout = A · B
     └──┤       │
        └───────┘
```

### 2.2 Sumador completo (*Full Adder*, FA)

Añade una tercera entrada, el **acarreo de entrada** $C_{in}$, lo que permite **encadenar** varias etapas.

| $A$ | $B$ | $C_{in}$ | $S$ | $C_{out}$ |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

**Simplificación con K-Map.** Filas con $A$ y las columnas $B\,C_{in}$:

```text
 S (m1, m2, m4, m7)               Cout (m3, m5, m6, m7)
       B Cin                            B Cin
 A    00  01  11  10             A    00  01  11  10
 0  |  0 | 1 | 0 | 1 |           0  |  0 | 0 | 1 | 0 |
 1  |  1 | 0 | 1 | 0 |           1  |  0 | 1 | 1 | 1 |
```

- **$S$:** los unos forman un "tablero de ajedrez", sin celdas vecinas, por lo que **no se puede agrupar**. Es la señal de una función XOR de tres variables.
- **$C_{out}$:** tres grupos de 2: $\{m_3,m_7\} = B\,C_{in}$, $\{m_5,m_7\} = A\,C_{in}$, $\{m_6,m_7\} = AB$.

$$S = A \oplus B \oplus C_{in}$$

$$C_{out} = AB + A\,C_{in} + B\,C_{in} = AB + C_{in}(A \oplus B)$$

La segunda forma de $C_{out}$ reutiliza $A \oplus B$, que ya se necesita para $S$.

### 2.3 Sumador completo a partir de dos medios sumadores

Como $S = (A \oplus B) \oplus C_{in}$ y $C_{out} = AB + C_{in}(A \oplus B)$, el FA se arma con **2 HA y una OR**:

```text
 HA #1:  entradas A, B       →  S1 = A ⊕ B,        C1 = A · B
 HA #2:  entradas S1, Cin    →  S  = S1 ⊕ Cin,     C2 = S1 · Cin
 OR:     entradas C1, C2     →  Cout = C1 + C2

 A ──┐        ┌──────┐ S1  ┌──────┐
     ├────────┤ HA#1 ├─────┤ HA#2 ├──── S
 B ──┘        │      │     │      │
              └──┬───┘ Cin─┤      ├─ C2 ─┐
                 C1        └──────┘      OR ── Cout
                 └─────────────────────────┘
```

Equivale a **2 XOR + 2 AND + 1 OR**.

### 2.4 Sumador paralelo con propagación de acarreo (*Ripple Carry*)

Para sumar palabras de $n$ bits se conectan $n$ FA en cascada: el $C_{out}$ de cada etapa entra como $C_{in}$ de la siguiente.

```text
          A3 B3        A2 B2        A1 B1        A0 B0
           │ │          │ │          │ │          │ │
 Cout ◄── [FA3] ◄─C3─ [FA2] ◄─C2─ [FA1] ◄─C1─ [FA0] ◄── Cin = 0
           │            │            │            │
           S3           S2           S1           S0
```

- El bit 0 (LSB) puede usar un HA, o un FA con $C_{in} = 0$.
- **Limitación:** cada etapa debe esperar el acarreo de la anterior, así que el retardo total **crece con el número de bits**.

**Ejemplo: $5 + 3$ en 4 bits**

| Bit | $A$ | $B$ | $C_{in}$ | $S$ | $C_{out}$ |
|---|---|---|---|---|---|
| 0 | 1 | 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 | 0 | 1 |
| 2 | 1 | 0 | 1 | 0 | 1 |
| 3 | 0 | 0 | 1 | 1 | 0 |

Resultado: $S_3S_2S_1S_0 = 1000_2$.

### 2.5 Detección de desbordamiento por hardware

La regla de la Semana 1 (acarreo que entra al MSB distinto del que sale) se implementa con **una sola XOR** sobre los acarreos del último FA:

$$V = C_{in,\,msb} \oplus C_{out,\,msb}$$

En el ejemplo $5 + 3$: al bit 3 entra un acarreo (`1`) y no sale ninguno (`0`), luego $V = 1$. En efecto, $+8$ no cabe en 4 bits con signo y $1000_2$ se lee como $-8$.

Forma equivalente, usando los bits de signo de los operandos ($A_{s}$, $B_{s}$) y del resultado ($S_{s}$):

$$V = A_s B_s S_s' + A_s' B_s' S_s$$

(dos positivos que dan negativo, o dos negativos que dan positivo).

| Bloque | Entradas | Salidas | ¿Tiene $C_{in}$? | Uso |
|---|---|---|---|---|
| HA | 2 ($A$, $B$) | 2 ($S$, $C_{out}$) | No | Bit menos significativo |
| FA | 3 ($A$, $B$, $C_{in}$) | 2 ($S$, $C_{out}$) | Sí | Bits intermedios y más significativo |

---

## 3. Restadores

### 3.1 Restadores dedicados

Se calcula $A - B$ ($A$ = minuendo, $B$ = sustraendo) con una **diferencia** $D$ y un **préstamo** (*borrow*) de salida $B_{out}$, que vale 1 cuando hace falta "pedir prestado" ($A < B$ en esa posición).

**Medio restador (Half Subtractor):**

| $A$ | $B$ | $D$ | $B_{out}$ |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |

$$D = A \oplus B \qquad\qquad B_{out} = A'B$$

**Restador completo (Full Subtractor)**, con préstamo de entrada $B_{in}$:

| $A$ | $B$ | $B_{in}$ | $D$ | $B_{out}$ |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 1 |
| 0 | 1 | 0 | 1 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 | 1 |

$$D = A \oplus B \oplus B_{in} \qquad\qquad B_{out} = A'B + A'B_{in} + B\,B_{in}$$

La diferencia $D$ es igual a la suma $S$ del sumador; solo cambia el acarreo, donde aparece $A'$.

### 3.2 Sumador/restador con complemento a dos

Construir sumadores **y** restadores separados duplicaría el hardware. En su lugar se aprovecha que (Semana 1):

$$A - B = A + (-B) = A + B' + 1$$

Entonces basta con el sumador paralelo más una **XOR por bit** en la entrada $B$, que actúa como **inversor controlable** gracias a una línea `Modo`:

| `Modo` | $B_i \oplus \text{Modo}$ | $C_{in}$ del bit 0 | Operación |
|---|---|---|---|
| 0 | $B_i$ (sin cambio) | 0 | $A + B$ |
| 1 | $B_i'$ (invertido) | 1 | $A + B' + 1 = A - B$ |

```text
 Modo ────────────────────────────────────────┬─ (a las 4 XOR)
                                              │
     B3 ─XOR─┐    B2 ─XOR─┐    B1 ─XOR─┐    B0 ─XOR─┐
             │            │            │            │
     A3 ───[FA3]◄─C3──[FA2]◄─C2──[FA1]◄─C1──[FA0]◄─── Cin0 = Modo
             │            │            │            │
             R3           R2           R1           R0
```

El mismo circuito suma o resta según `Modo`, y con la XOR de la sección 2.5 se obtiene también el indicador de desbordamiento $V$.

**Ejemplo: $5 - 3$ con `Modo` = 1**

```text
 A        = 0101
 B        = 0011  →  B XOR 1 = 1100
 Cin0     = 1
 0101 + 1100 + 1 = 1 0010   →   R = 0010 = +2
```

Acarreos: entra `1` al MSB y sale `1`, luego $V = 0$.

> En una resta, un $C_{out}$ final igual a `1` indica que **no hubo préstamo** (es decir, $A \ge B$ si se interpretan como números sin signo); `0` indica que sí lo hubo.

---

## 4. Comparadores de magnitud

Un **comparador** recibe dos palabras de $n$ bits, $A$ y $B$, y activa **exactamente una** de tres salidas: $A > B$, $A = B$ o $A < B$.

### 4.1 Comparador de 1 bit

| $A$ | $B$ | $A > B$ | $A = B$ | $A < B$ |
|---|---|---|---|---|
| 0 | 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 1 | 0 |

$$(A>B) = A\,B' \qquad (A<B) = A'B \qquad (A=B) = (A \oplus B)' = A \odot B$$

### 4.2 Comparador de $n$ bits

Se define la **igualdad bit a bit** con una XNOR:

$$x_i = (A_i \oplus B_i)' = A_i \odot B_i$$

- **Igualdad:** las dos palabras son iguales si **todos** los bits coinciden:

$$(A = B) = x_{n-1}\,x_{n-2}\cdots x_1\,x_0$$

- **Mayor y menor:** se examina desde el **bit más significativo** hacia abajo. $A > B$ en el primer bit donde difieren, siempre que los bits más altos hayan sido iguales:

$$(A>B) = A_{n-1}B_{n-1}' + x_{n-1}A_{n-2}B_{n-2}' + x_{n-1}x_{n-2}A_{n-3}B_{n-3}' + \cdots$$

$$(A<B) = A_{n-1}'B_{n-1} + x_{n-1}A_{n-2}'B_{n-2} + x_{n-1}x_{n-2}A_{n-3}'B_{n-3} + \cdots$$

Como solo una de las tres salidas es `1`, también se cumple $(A = B) = \big[(A>B) + (A<B)\big]'$.

### 4.3 Ejemplo de 2 bits

$$x_1 = A_1 \odot B_1 \qquad x_0 = A_0 \odot B_0$$

$$(A=B) = x_1x_0 \qquad (A>B) = A_1B_1' + x_1A_0B_0' \qquad (A<B) = A_1'B_1 + x_1A_0'B_0$$

**Prueba con $A = 10_2$ (2) y $B = 01_2$ (1):** $A_1B_1' = 1\cdot1 = 1$, de modo que $A > B$ y las otras dos salidas valen 0.

---

## 5. Práctica de laboratorio (APE 04)

| Dato | Detalle |
|---|---|
| Sesión | APE 04 |
| Horario | Martes 27 de octubre, 07:30 a 09:30 |
| Entorno | Simulador **Logisim-evolution** y banco físico con chips TTL |

**Chips útiles** (Semana 1): 74LS08 (AND), 74LS32 (OR), 74LS86 (XOR) y 74LS04 (NOT). Con ellos se puede armar:

- **HA:** 1 XOR (74LS86) + 1 AND (74LS08).
- **FA:** 2 XOR + 2 AND + 1 OR, o sea un 74LS86, un 74LS08 y un 74LS32.

Existen además sumadores y comparadores integrados de 4 bits (por ejemplo el 74LS83 y el 74LS85).

**Qué verificar:**

1. Que la tabla de verdad medida coincida con la teórica, probando las $2^n$ combinaciones.
2. El $t_{pd}$ de cada salida (simulación o instrumento).
3. En el sumador en cascada, que el retardo aumenta con el número de etapas.

---

## Resumen de la semana

1. El **diseño combinacional** sigue cinco pasos y, con varias salidas, cada una se simplifica por separado compartiendo términos cuando se puede.
2. **Medio sumador:** $S = A \oplus B$, $C_{out} = AB$. **Sumador completo:** $S = A \oplus B \oplus C_{in}$, $C_{out} = AB + C_{in}(A \oplus B)$, equivalente a 2 HA + OR.
3. Encadenando FA se obtiene el **sumador paralelo con propagación de acarreo**, cuyo retardo crece con el número de bits.
4. El **overflow** se detecta con $V = C_{in,msb} \oplus C_{out,msb}$.
5. **Restadores:** $D = A \oplus B \,(\oplus\, B_{in})$ y $B_{out} = A'B$ (o $A'B + A'B_{in} + BB_{in}$).
6. El **sumador/restador** reutiliza el sumador con una XOR por bit y $C_{in,0} = \text{Modo}$: $A - B = A + B' + 1$.
7. El **comparador** usa XNOR bit a bit para la igualdad y una inspección de MSB a LSB para mayor y menor.

---

## Autoevaluación

1. Sume $0110 + 0111$ en 4 bits con signo. Indique el resultado y el valor de $V$.
2. Calcule $6 - 7$ con el sumador/restador de 4 bits. ¿Qué indica el $C_{out}$?
3. Obtenga $D$ y $B_{out}$ del restador completo para $A = 0$, $B = 1$, $B_{in} = 1$.
4. Con el comparador de 2 bits, evalúe las tres salidas para $A = 11_2$ y $B = 10_2$.
5. ¿Qué chips 74LS y cuántas compuertas se necesitan para un FA con compuertas básicas?
6. Escriba la expresión de $(A<B)$ para 3 bits.

<details>
<summary>Ver respuestas</summary>

1. $0110 + 0111 = 1101$, que se lee $-3$. Acarreos: entra `1` al MSB y no sale ninguno, así que $V = 1$ (overflow, $+13$ no cabe en 4 bits).
2. $B = 0111 \to B' = 1000$, $C_{in0} = 1$: $0110 + 1000 + 1 = 1111 = -1$. No hay acarreos hacia ni desde el MSB, luego $V = 0$. $C_{out} = 0$ indica que **sí hubo préstamo** ($6 < 7$).
3. $D = 0 \oplus 1 \oplus 1 = 0$ y $B_{out} = A'B + A'B_{in} + BB_{in} = 1$ (se cumple $0 - 1 - 1 = -2$).
4. $x_1 = 1 \odot 1 = 1$ y $x_0 = 1 \odot 0 = 0$. Entonces $A>B = 0 + 1\cdot1\cdot1 = 1$, $A=B = 0$ y $A<B = 0$.
5. 2 XOR (74LS86), 2 AND (74LS08) y 1 OR (74LS32): 5 compuertas en 3 chips.
6. $(A<B) = A_2'B_2 + x_2A_1'B_1 + x_2x_1A_0'B_0$.

</details>

---
