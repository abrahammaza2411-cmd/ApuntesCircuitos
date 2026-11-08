# Semana 2: Álgebra de Boole y Formas Canónicas

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de Lógica Combinacional con Diseño Clásico

---

## Objetivos de la semana

Al terminar esta semana se debe ser capaz de:

- Expresar un circuito de compuertas como una expresión booleana y viceversa.
- Aplicar los postulados, teoremas y el principio de dualidad del álgebra de Boole.
- Usar las leyes de De Morgan para complementar expresiones.
- Traducir un enunciado verbal a una tabla de verdad, detectando ambigüedades.
- Escribir una función en sus formas canónicas **SOP** (suma de productos, $\Sigma m$) y **POS** (producto de sumas, $\Pi M$).
- Simplificar funciones por vía algebraica para reducir compuertas y literales.

> **Conocimientos previos (Semana 1):** compuertas NOT, AND, OR, NAND, NOR, XOR, XNOR con sus tablas de verdad, el hecho de que NAND y NOR son universales, y la numeración binaria. Aquí se usan sin volver a explicarlos.

---

## 1. Álgebra de Boole

Las especificaciones en lenguaje natural suelen ser ambiguas. El **álgebra de Boole** ofrece un modelo matemático exacto para describir el comportamiento de señales binarias y, a partir de él, diseñar y optimizar circuitos.

Se define sobre el conjunto $\{0, 1\}$ con tres operaciones:

| Operación | Símbolo | Compuerta |
|---|---|---|
| Suma lógica | $+$ | OR |
| Producto lógico | $\cdot$ (o yuxtaposición: $XY$) | AND |
| Complemento | $'$ (o barra superior) | NOT |

### 1.1 Correspondencia circuito ↔ expresión

| Elemento del circuito | Elemento algebraico |
|---|---|
| Cable de entrada | Variable ($A$, $B$, $C$) |
| Compuerta AND | Producto |
| Compuerta OR | Suma |
| Compuerta NOT | Complemento |
| Salida | Valor de la función ($F$ o $Y$) |

Cada expresión tiene un circuito directo, y cada circuito se puede leer como una expresión.

**Ejemplo:** $F = A\,B' + C$ corresponde a una NOT sobre $B$, una AND con $A$ y una OR con $C$.

```text
 B ──▷o──┐
         ├── AND ──┐
 A ──────┘         ├── OR ── F
 C ────────────────┘
```

### 1.2 Precedencia de operadores

Se evalúa primero lo que está entre **paréntesis** y luego, en este orden:

$$\text{NOT } (') \;\rightarrow\; \text{AND } (\cdot) \;\rightarrow\; \text{OR } (+)$$

Por ejemplo, $A + B\,C'$ significa $A + \big(B \cdot (C')\big)$, que es distinto de $(A + B)\,C'$. Alterar la precedencia cambia el circuito resultante.

### 1.3 Vocabulario

| Término | Definición | Ejemplo |
|---|---|---|
| **Literal** | Una variable, complementada o no | $A$, $B'$ |
| **Término producto** | Literales unidos por AND | $AB'C$ |
| **Término suma** | Literales unidos por OR | $A + B' + C$ |

Para medir la complejidad de una expresión se cuentan **términos** y **literales**; menos literales significa menos entradas de compuertas.

---

## 2. Postulados, teoremas y dualidad

### 2.1 Postulados de Huntington

Son los axiomas de partida, aceptados sin demostración. Cada uno tiene una forma OR y una forma AND (dual):

| Postulado | Forma OR | Forma AND |
|---|---|---|
| Identidad | $X + 0 = X$ | $X \cdot 1 = X$ |
| Complemento | $X + X' = 1$ | $X \cdot X' = 0$ |
| Conmutativa | $X + Y = Y + X$ | $XY = YX$ |
| Distributiva | $X(Y+Z) = XY + XZ$ | $X + YZ = (X+Y)(X+Z)$ |

> **Diferencia con el álgebra convencional:** la distributiva de la suma sobre el producto, $X + YZ = (X+Y)(X+Z)$, **también es válida** aquí. Además no existen la resta ni la división.

### 2.2 Teoremas derivados

Se demuestran a partir de los postulados:

| Teorema | Forma OR | Forma AND |
|---|---|---|
| Elemento nulo | $X + 1 = 1$ | $X \cdot 0 = 0$ |
| Idempotencia | $X + X = X$ | $X \cdot X = X$ |
| Involución | $(X')' = X$ | — |
| Absorción | $X + XY = X$ | $X(X + Y) = X$ |
| Absorción del complemento | $X + X'Y = X + Y$ | $X(X' + Y) = XY$ |
| Adyacencia (combinación) | $XY + XY' = X$ | $(X+Y)(X+Y') = X$ |
| Consenso | $XY + X'Z + YZ = XY + X'Z$ | $(X+Y)(X'+Z)(Y+Z) = (X+Y)(X'+Z)$ |
| Asociativa | $X + (Y+Z) = (X+Y) + Z$ | $X(YZ) = (XY)Z$ |

### 2.3 Principio de dualidad

Toda identidad booleana sigue siendo válida si se **intercambian simultáneamente** $+ \leftrightarrow \cdot$ y $0 \leftrightarrow 1$, **sin complementar las variables**. Por eso cada identidad de las tablas anteriores aparece en dos formas.

**Ejemplo:** el dual de $X + X'Y = X + Y$ es $X(X' + Y) = XY$.

### 2.4 Demostraciones

Hay dos métodos: **algebraico** (con postulados y teoremas) y por **inducción perfecta** (comprobar con una tabla de verdad todas las combinaciones).

**a) Distributiva de la suma sobre el producto: $A + BC = (A+B)(A+C)$**

Se parte del miembro derecho:

$$
\begin{aligned}
(A+B)(A+C) &= AA + AC + AB + BC && \text{(distributiva)} \\
&= A + AC + AB + BC && \text{(idempotencia: } AA = A\text{)} \\
&= A(1 + C + B) + BC && \text{(factorizando } A\text{)} \\
&= A \cdot 1 + BC && \text{(elemento nulo: } 1 + C + B = 1\text{)} \\
&= A + BC && \text{(identidad)} \quad \blacksquare
\end{aligned}
$$

**b) Absorción del complemento: $X + X'Y = X + Y$**

$$
\begin{aligned}
X + X'Y &= (X + X')(X + Y) && \text{(distributiva de la suma sobre el producto)} \\
&= 1 \cdot (X + Y) && \text{(complemento)} \\
&= X + Y && \text{(identidad)} \quad \blacksquare
\end{aligned}
$$

**c) Consenso: $XY + X'Z + YZ = XY + X'Z$**

$$
\begin{aligned}
XY + X'Z + YZ &= XY + X'Z + YZ(X + X') \\
&= XY + X'Z + XYZ + X'YZ \\
&= XY(1 + Z) + X'Z(1 + Y) \\
&= XY + X'Z \quad \blacksquare
\end{aligned}
$$

### 2.5 Leyes de De Morgan

Permiten complementar una expresión compuesta. Regla práctica: **se rompe la barra y se cambia el operador**.

$$(A \cdot B \cdot C)' = A' + B' + C' \qquad\qquad (A + B + C)' = A' \cdot B' \cdot C'$$

**Verificación por inducción perfecta (2 variables):**

| $A$ | $B$ | $(AB)'$ | $A' + B'$ | $(A+B)'$ | $A'B'$ |
|---|---|---|---|---|---|
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 1 | 1 | 0 | 0 |
| 1 | 0 | 1 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 |

**Ejemplo de uso:** complementar $F = A + BC'$:

$$F' = A' \cdot (BC')' = A'(B' + C)$$

Consecuencia a nivel de compuertas: una AND con salida negada equivale a una OR con las entradas negadas (y viceversa), lo que permite reescribir una expresión con distintos tipos de compuertas.

---

## 3. De enunciado verbal a tabla de verdad

Pasar de una especificación a un circuito sigue estos pasos:

1. **Definir las variables** y qué significa un `1` y un `0` en cada una.
2. **Construir la tabla de verdad** con las $2^n$ combinaciones de entrada, en orden binario.
3. **Asignar la salida** de cada fila según el enunciado.
4. **Escribir la forma canónica** (sección 4).
5. **Simplificar** (sección 5) y dibujar el circuito.

Un error de interpretación en el paso 3 produce un circuito que no cumple lo pedido.

### 3.1 Ambigüedad en el lenguaje natural: la sirena

> *"La sirena suena cuando se activan dos sensores."* (tres sensores: $A$, $B$, $C$)

El enunciado admite dos lecturas distintas:

| Lectura | Condición | Filas con $F=1$ | $F(1,1,1)$ |
|---|---|---|---|
| **Exactamente dos** | Solo suena con dos activos | $\Sigma m(3, 5, 6)$ | 0 (sirena apagada) |
| **Al menos dos** | Suena con dos o más activos | $\Sigma m(3, 5, 6, 7)$ | 1 (sirena encendida) |

Con la primera lectura la sirena se apagaría si los tres sensores se activan, algo inadmisible en un sistema de seguridad. Para alarmas de incendio o intrusión corresponde la lectura **"al menos dos"**, y es la que se usa en lo que sigue.

| $A$ | $B$ | $C$ | $F$ (al menos dos) | Fila |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 2 |
| 0 | 1 | 1 | **1** | 3 |
| 1 | 0 | 0 | 0 | 4 |
| 1 | 0 | 1 | **1** | 5 |
| 1 | 1 | 0 | **1** | 6 |
| 1 | 1 | 1 | **1** | 7 |

### 3.2 Convención de los sensores: bomba de la cisterna

Un sistema de bombeo usa tres entradas:

- $A$: tanque alto. $1$ = nivel bajo (necesita agua), $0$ = lleno.
- $B$: cisterna. $1$ = tiene agua, $0$ = vacía.
- $C$: mando manual. $1$ = encendido solicitado.

**Especificación:** la bomba se enciende cuando **hay agua en la cisterna Y (el tanque necesita agua O hay orden manual)**, es decir:

$$F = B\,(A + C)$$

| $A$ | $B$ | $C$ | $A + C$ | $F$ | Fila |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 0 | 2 |
| 0 | 1 | 1 | 1 | **1** | 3 |
| 1 | 0 | 0 | 1 | 0 | 4 |
| 1 | 0 | 1 | 1 | 0 | 5 |
| 1 | 1 | 0 | 1 | **1** | 6 |
| 1 | 1 | 1 | 1 | **1** | 7 |

Dos observaciones de diseño:

- **El significado lógico depende de la polaridad del sensor.** Si el sensor del tanque entregara `1` cuando está **lleno** (en lugar de cuando está vacío), la variable significaría lo contrario y la función pasaría a ser $F = B(A' + C)$. Hay que fijar la convención antes de armar la tabla.
- **Revisión funcional.** La fila 3 ($A=0$, $B=1$, $C=1$) enciende la bomba por mando manual aunque el tanque no necesite agua. La tabla lo especifica así; el diseñador debe decidir si es aceptable (por ejemplo, para mantenimiento) o si se necesita un bloqueo contra desbordes.

---

## 4. Formas canónicas: SOP y POS

Una **forma canónica** es la expresión estándar y única que se obtiene de una tabla de verdad, donde **cada término contiene todas las variables**. Hay dos formas duales: una describe cuándo la salida vale $1$ y la otra cuándo vale $0$.

### 4.1 Minitérminos y maxitérminos

- **Minitérmino $m_i$:** producto (AND) de las $n$ variables. Cada variable aparece **directa si vale 1** en esa fila y **complementada si vale 0**. Vale $1$ solo en su fila.
- **Maxitérmino $M_i$:** suma (OR) de las $n$ variables. Cada variable aparece **directa si vale 0** y **complementada si vale 1**. Vale $0$ solo en su fila.

Se cumple que $M_i = (m_i)'$.

| $i$ | $A\,B\,C$ | Minitérmino $m_i$ | Maxitérmino $M_i$ |
|---|---|---|---|
| 0 | 0 0 0 | $A'B'C'$ | $A + B + C$ |
| 1 | 0 0 1 | $A'B'C$ | $A + B + C'$ |
| 2 | 0 1 0 | $A'BC'$ | $A + B' + C$ |
| 3 | 0 1 1 | $A'BC$ | $A + B' + C'$ |
| 4 | 1 0 0 | $AB'C'$ | $A' + B + C$ |
| 5 | 1 0 1 | $AB'C$ | $A' + B + C'$ |
| 6 | 1 1 0 | $ABC'$ | $A' + B' + C$ |
| 7 | 1 1 1 | $ABC$ | $A' + B' + C'$ |

> **Regla mnemotécnica:** en el minitérmino un `1` se escribe como $A$ y un `0` como $A'$; en el maxitérmino es al revés.

### 4.2 Suma de productos (SOP, $\Sigma m$)

Se toman las filas con $F = 1$, se escribe su minitérmino y se unen con **OR**.

Para la sirena ("al menos dos"), $F=1$ en las filas 3, 5, 6 y 7:

$$F = \Sigma m(3, 5, 6, 7) = A'BC + AB'C + ABC' + ABC$$

### 4.3 Producto de sumas (POS, $\Pi M$)

Se toman las filas con $F = 0$, se escribe su maxitérmino y se unen con **AND**.

Para la sirena, $F=0$ en las filas 0, 1, 2 y 4:

$$F = \Pi M(0, 1, 2, 4) = (A+B+C)(A+B+C')(A+B'+C)(A'+B+C)$$

### 4.4 Relación entre ambas formas

Las dos describen la **misma función**. Los índices del POS son el **complemento** de los del SOP dentro del universo $U = \{0, 1, \dots, 2^n - 1\}$:

$$\Sigma m(3,5,6,7) \;\Longleftrightarrow\; \Pi M(0,1,2,4) \qquad\text{pues}\quad U \setminus \{3,5,6,7\} = \{0,1,2,4\}$$

Además, nº de minitérminos + nº de maxitérminos $= 2^n$.

**Para la bomba de la cisterna:** $F = \Sigma m(3, 6, 7) = \Pi M(0, 1, 2, 4, 5)$.

**¿Cuál usar?** Conviene el SOP cuando hay **pocos unos** y el POS cuando hay **pocos ceros** en la tabla, porque se escriben menos términos.

Las formas canónicas son completas pero poco eficientes: cada término lleva todas las variables. Implementarlas directamente exigiría más compuertas de las necesarias.

---

## 5. Simplificación algebraica

Aplicar los postulados y teoremas reduce términos y literales. Esto significa menos compuertas, menos entradas, menos área, menos consumo y menor retardo de propagación.

### 5.1 Ejemplo guiado

Simplificar $F = A + A'B + AB'C$.

| Paso | Expresión | Justificación |
|---|---|---|
| 0 | $A + A'B + AB'C$ | Punto de partida |
| 1 | $(A + B) + AB'C$ | Absorción del complemento: $A + A'B = A + B$ |
| 2 | $A + AB'C + B$ | Conmutativa (reordenar) |
| 3 | $A + B$ | Absorción: $A + A(B'C) = A$ |

$$F = A + B$$

| | Original | Simplificada |
|---|---|---|
| Términos | 3 | 2 |
| Literales | 6 | 2 |
| Circuito | NOT + 2 AND + OR (con una AND de 3 entradas) | 1 OR de 2 entradas |

### 5.2 De la forma canónica a la mínima

**Bomba de la cisterna: $F = \Sigma m(3, 6, 7)$**

Se duplica $m_7$ (idempotencia) para combinarlo con sus dos vecinos:

$$
\begin{aligned}
F &= A'BC + ABC' + ABC \\
  &= (A'BC + ABC) + (ABC' + ABC) && (X + X = X) \\
  &= BC(A' + A) + AB(C' + C) && \text{(factor común)} \\
  &= BC + AB && (X + X' = 1,\; X \cdot 1 = X) \\
  &= B(A + C) && \text{(factor común } B\text{)}
\end{aligned}
$$

Así se recupera la función de partida, ahora con una OR y una AND.

**Sirena (al menos dos): $F = \Sigma m(3, 5, 6, 7)$**

Se usa $m_7$ tres veces:

$$
\begin{aligned}
F &= A'BC + AB'C + ABC' + ABC + ABC + ABC \\
  &= BC(A' + A) + AC(B' + B) + AB(C' + C) \\
  &= BC + AC + AB
\end{aligned}
$$

Es la función "mayoría": la salida es `1` cuando la mayoría de las entradas valen `1`.

> **Idea clave:** al combinar dos minitérminos que difieren en una sola variable (teorema de adyacencia, $XY + XY' = X$) esa variable desaparece.

### 5.3 Casos de aplicación

#### Alarma de seguridad de un automóvil

Variables: $A$ = motor encendido (1), $B$ = cinturón abrochado (1) y $C$ = puerta del conductor abierta (1).

**Especificación:** la alarma suena si el **motor está encendido Y el cinturón NO está abrochado**, **O** si la **puerta está abierta**.

$$\text{Alarma} = A \cdot B' + C$$

Tabla de verdad y formas canónicas:

| $A$ | $B$ | $C$ | $F$ | Fila |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | **1** | 1 |
| 0 | 1 | 0 | 0 | 2 |
| 0 | 1 | 1 | **1** | 3 |
| 1 | 0 | 0 | **1** | 4 |
| 1 | 0 | 1 | **1** | 5 |
| 1 | 1 | 0 | 0 | 6 |
| 1 | 1 | 1 | **1** | 7 |

$$F = \Sigma m(1, 3, 4, 5, 7) = \Pi M(0, 2, 6)$$

Aquí el POS tiene menos términos que el SOP (3 frente a 5).

Simplificación desde el SOP: las filas 1, 3, 5 y 7 son todas las que tienen $C=1$, así que valen $C$. Queda el minitérmino $m_4 = AB'C'$:

$$F = C + AB'C' = C + AB' \qquad \text{(absorción del complemento: } X + X'Y = X + Y\text{)}$$

Coincide con la expresión original.

#### Luz de pasillo con interruptor maestro

Variables: $A$ y $B$ son los interruptores de los extremos (la lámpara cambia de estado al accionar cualquiera, comportamiento XOR) y $M$ es el interruptor maestro.

**Especificación:** si $M = 1$ la luz permanece **apagada**, sin importar $A$ y $B$. Si $M = 0$, la luz sigue a $A \oplus B$.

$$\text{Luz} = (A \oplus B) \cdot M'$$

| $A$ | $B$ | $M$ | Luz | Fila |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | **1** | 2 |
| 0 | 1 | 1 | 0 | 3 |
| 1 | 0 | 0 | **1** | 4 |
| 1 | 0 | 1 | 0 | 5 |
| 1 | 1 | 0 | 0 | 6 |
| 1 | 1 | 1 | 0 | 7 |

$$\text{Luz} = \Sigma m(2, 4) = A'BM' + AB'M' \qquad \Pi M(0,1,3,5,6,7)$$

Como $A \oplus B = A'B + AB'$, al multiplicar por $M'$ se obtiene exactamente el SOP canónico. Aquí el SOP es mucho más corto que el POS (2 términos frente a 6).

---

## Resumen de la semana

1. El **álgebra de Boole** modela los circuitos: variables = cables, producto = AND, suma = OR, complemento = NOT. La precedencia es paréntesis, NOT, AND y OR.
2. Los **postulados de Huntington** (identidad, complemento, conmutativa, distributiva) son la base; de ellos salen los teoremas (nulo, idempotencia, involución, absorción, adyacencia, consenso). El **principio de dualidad** duplica cada identidad.
3. Las **leyes de De Morgan** complementan expresiones: se rompe la barra y se cambia el operador.
4. Antes de construir la tabla de verdad hay que **eliminar ambigüedades** y fijar la **polaridad** de cada variable (caso de la sirena y de la cisterna).
5. **SOP** $= \Sigma m$ (filas con $F=1$, términos AND unidos con OR) y **POS** $= \Pi M$ (filas con $F=0$, términos OR unidos con AND). Sus índices son complementarios.
6. La **simplificación algebraica** (adyacencia, absorción, idempotencia) reduce términos y literales y, con ello, compuertas.

---

## Autoevaluación

1. Demuestre con una tabla de verdad que $X + X'Y = X + Y$.
2. Escriba como $\Pi M$ la función $F = \Sigma m(1, 2, 5, 6, 7)$ de 3 variables.
3. Simplifique $F = AB + AB' + A'B$.
4. Obtenga el complemento de $F = A + BC'$ y de $F = A'B + AB'$ usando De Morgan.
5. Escriba el dual de $X(Y + Z) = XY + XZ$.
6. Plantee la tabla de verdad y la expresión SOP de una función que vale $1$ cuando **exactamente una** de tres entradas vale $1$.

<details>
<summary>Ver respuestas</summary>

1. | $X$ | $Y$ | $X'Y$ | $X + X'Y$ | $X + Y$ |
   |---|---|---|---|---|
   | 0 | 0 | 0 | 0 | 0 |
   | 0 | 1 | 1 | 1 | 1 |
   | 1 | 0 | 0 | 1 | 1 |
   | 1 | 1 | 0 | 1 | 1 |

   Las dos últimas columnas coinciden en todas las filas.
2. El universo es $\{0,\dots,7\}$; los ceros están en las filas 0, 3 y 4: $F = \Pi M(0, 3, 4)$.
3. $F = A(B + B') + A'B = A + A'B = A + B$.
4. $(A + BC')' = A'(B' + C)$. Para el segundo: $(A'B + AB')' = (A'B)'\,(AB')' = (A + B')(A' + B)$.
5. $X + YZ = (X + Y)(X + Z)$.
6. $F = 1$ en las filas 1, 2 y 4: $F = \Sigma m(1,2,4) = A'B'C + A'BC' + AB'C'$.

</details>

---
