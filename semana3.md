# Semana 3: Simplificación con Mapas de Karnaugh y Compuertas Universales

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de Lógica Combinacional con Diseño Clásico

---

## Objetivos de la semana

Al terminar esta semana se debe ser capaz de:

- Construir Mapas de Karnaugh (K-Maps) de 2, 3 y 4 variables a partir de una tabla de verdad.
- Agrupar unos para obtener la forma mínima **SOP** y agrupar ceros para obtener la forma mínima **POS**.
- Aprovechar las condiciones **"no importa"** (*don't care*) para simplificar más.
- Implementar una función simplificada usando **solo compuertas NAND o solo NOR**.

> **Conocimientos previos:** código Gray y su adyacencia de un solo bit (Semana 1); minitérminos, maxitérminos, formas canónicas $\Sigma m$ y $\Pi M$, teoremas de adyacencia y absorción, y De Morgan (Semana 2). También el hecho de que NAND y NOR son universales y los chips 74LS00 y 74LS02 (Semana 1).

---

## 1. Por qué un método visual

La simplificación algebraica de la Semana 2 funciona, pero depende de **ver** qué teorema aplicar y no hay una receta mecánica que garantice haber llegado al mínimo. El **Mapa de Karnaugh** convierte la tabla de verdad en una cuadrícula donde las simplificaciones por adyacencia ($XY + XY' = X$) se ven como **grupos de celdas vecinas**. El resultado es menos compuertas, menos literales, menos área de silicio y menor retardo.

---

## 2. Estructura del Mapa de Karnaugh

Cada celda representa **una fila** de la tabla de verdad (un minitérmino). Las filas y columnas se rotulan en **código Gray** (`00, 01, 11, 10`), de modo que **celdas vecinas difieren en una sola variable**. Por eso dos celdas adyacentes se pueden combinar con el teorema de adyacencia y esa variable desaparece.

### 2.1 Mapa de 2 variables

```text
        B
 A      0     1
 0   |  m0 |  m1 |
 1   |  m2 |  m3 |
```

### 2.2 Mapa de 3 variables

```text
        BC
 A     00    01    11    10
 0   |  m0 |  m1 |  m3 |  m2 |
 1   |  m4 |  m5 |  m7 |  m6 |
```

### 2.3 Mapa de 4 variables

```text
          CD
 AB      00    01    11    10
 00   |  m0 |  m1 |  m3 |  m2 |
 01   |  m4 |  m5 |  m7 |  m6 |
 11   | m12 | m13 | m15 | m14 |
 10   |  m8 |  m9 | m11 | m10 |
```

> **Atención:** el orden de las columnas es `00, 01, 11, 10`, **no** el binario natural. Los números de los minitérminos no siguen una secuencia corrida.

### 2.4 Adyacencia en los bordes (*wrap-around*)

El mapa es **continuo**: la columna izquierda es vecina de la derecha, y la fila superior es vecina de la inferior. En un mapa de 4 variables, las **cuatro esquinas** también son adyacentes entre sí. Por eso un grupo puede "dar la vuelta" por los bordes.

### 2.5 Cómo llenar el mapa

- Con una lista de minitérminos, se escribe `1` en cada celda $m_i$ listada y `0` en las demás.
- Con una tabla de verdad, se copia cada salida a la celda de su fila.
- Con una expresión, se marcan con `1` las celdas donde el producto es verdadero.

---

## 3. Simplificación a suma de productos (SOP) agrupando unos

### 3.1 Reglas de agrupación

1. Los grupos son **rectángulos** de celdas con valor `1` y su tamaño debe ser **potencia de dos**: 1, 2, 4, 8 o 16.
2. Los grupos pueden **solaparse** y pueden envolver los bordes.
3. Se buscan los grupos **más grandes posibles** y el **menor número** de grupos que cubra todos los unos.
4. Todo `1` debe quedar cubierto por al menos un grupo.

Un grupo de $2^k$ celdas elimina $k$ variables:

| Tamaño del grupo | Variables eliminadas | Literales en el término (4 variables) |
|---|---|---|
| 1 | 0 | 4 |
| 2 | 1 | 3 |
| 4 | 2 | 2 |
| 8 | 3 | 1 |
| 16 | 4 | función constante 1 |

### 3.2 Cómo leer el término de un grupo

Dentro de un grupo, las variables que **no cambian** se conservan y las que **cambian** (pasan por 0 y 1) se eliminan. Las que se conservan se escriben:

- **directas** si valen `1` en todo el grupo;
- **complementadas** si valen `0` en todo el grupo.

Los términos de todos los grupos se unen con **OR**.

### 3.3 Procedimiento

1. Llenar el mapa.
2. Marcar primero los unos que tienen **una sola forma de agruparse** (suelen ser aislados o en un borde).
3. Agrupar el resto con los rectángulos más grandes.
4. Verificar que no sobre ningún grupo (uno cuyos unos ya estén cubiertos por otros).
5. Escribir un término por grupo y sumarlos.

### 3.4 Ejemplos

**Ejemplo 1 (2 variables): $F = \Sigma m(1, 3)$**

```text
        B
 A      0     1
 0   |  0  |  1  |
 1   |  0  |  1  |
```

El grupo vertical (columna $B=1$) elimina $A$: $F = B$.

**Ejemplo 2 (3 variables): $F = \Sigma m(3, 6, 7)$**

```text
        BC
 A     00    01    11    10
 0   |  0  |  0  |  1  |  0  |
 1   |  0  |  0  |  1  |  1  |
```

- Grupo vertical $\{m_3, m_7\}$ (columna $BC = 11$): $A$ cambia, queda $BC$.
- Grupo horizontal $\{m_6, m_7\}$ ($A=1$, $B=1$): $C$ cambia, queda $AB$.

$$F = BC + AB = B(A + C)$$

Es el mismo resultado que se obtuvo algebraicamente para la bomba de la cisterna, pero ahora se lee directamente del mapa.

**Ejemplo 3 (3 variables, bordes): $F = \Sigma m(0, 2, 4, 6)$**

```text
        BC
 A     00    01    11    10
 0   |  1  |  0  |  0  |  1  |
 1   |  1  |  0  |  0  |  1  |
```

Las columnas `00` y `10` son vecinas por el borde, así que forman un **grupo de 4**. $A$ y $B$ cambian, solo se conserva $C = 0$:

$$F = C'$$

**Ejemplo 4 (3 variables, grupo sobrante): $F = \Sigma m(0, 1, 3, 7)$**

```text
        BC
 A     00    01    11    10
 0   |  1  |  1  |  1  |  0  |
 1   |  0  |  0  |  1  |  0  |
```

Los grupos posibles de 2 son $\{m_0,m_1\} = A'B'$, $\{m_1,m_3\} = A'C$ y $\{m_3,m_7\} = BC$.

- $m_0$ solo está en $A'B'$, y $m_7$ solo está en $BC$: esos grupos son **obligatorios** (esenciales).
- Con ellos ya quedan cubiertos $m_1$ y $m_3$, así que $A'C$ **sobra**.

$$F = A'B' + BC$$

**Ejemplo 5 (4 variables, esquinas y centro): $F = \Sigma m(0, 2, 5, 7, 8, 10, 13, 15)$**

```text
          CD
 AB      00    01    11    10
 00   |  1  |  0  |  0  |  1  |
 01   |  0  |  1  |  1  |  0  |
 11   |  0  |  1  |  1  |  0  |
 10   |  1  |  0  |  0  |  1  |
```

- Las **cuatro esquinas** ($m_0, m_2, m_8, m_{10}$) forman un grupo de 4 donde solo se conserva $B=0$, $D=0$: $B'D'$.
- El **bloque central** ($m_5, m_7, m_{13}, m_{15}$) conserva $B=1$, $D=1$: $BD$.

$$F = B'D' + BD$$

---

## 4. Simplificación a producto de sumas (POS) agrupando ceros

Para obtener el POS mínimo se agrupan los **ceros** con las mismas reglas de tamaño y forma. La diferencia está en cómo se lee cada grupo: cada grupo da un **término suma** y las variables se escriben al revés:

- **directa** si valen `0` en el grupo;
- **complementada** si valen `1` en el grupo.

Los términos suma se unen con **AND**.

**Ejemplo: la función $F = \Sigma m(3, 6, 7)$ otra vez**, ahora con sus ceros ($m_0, m_1, m_2, m_4, m_5$):

```text
        BC
 A     00    01    11    10
 0   |  0  |  0  |  1  |  0  |
 1   |  0  |  0  |  1  |  1  |
```

- Grupo de 4 $\{m_0, m_1, m_4, m_5\}$: solo se conserva $B = 0$, con $B$ directa: término $B$.
- Grupo de 2 $\{m_0, m_2\}$ (columnas `00` y `10` de la fila 0, unidas por el borde): se conservan $A=0$ y $C=0$: término $(A + C)$.

$$F = B\,(A + C)$$

Coincide con el resultado SOP, como debe ser. Se elige SOP o POS según cuál dé una expresión más sencilla o más conveniente para los chips disponibles; en general conviene agrupar lo que haya **menos**.

---

## 5. Condiciones "no importa" (*don't care*)

Hay combinaciones de entrada que **nunca ocurren** o cuya salida da igual. Se marcan con **X** (o $d$) y se escribe la función como $F = \Sigma m(\dots) + d(\dots)$.

**Regla:** cada X puede tomarse como `1` o como `0`, **a conveniencia**.

- Se incluye en un grupo solo si **agranda** ese grupo.
- **No es obligatorio cubrirlas**: una X que no ayuda se ignora.

| | Sin usar X | Usando X |
|---|---|---|
| Tamaño de grupos | Limitado por los unos | Más grandes |
| Términos resultantes | Más literales | Menos literales |
| Compuertas | Más | Menos |

### Ejemplo con BCD: "el dígito es mayor o igual a 5"

Las entradas $ABCD$ son un dígito BCD (Semana 1). Las combinaciones $1010_2$ a $1111_2$ (10 a 15) **no son válidas**, así que son X.

$$F = \Sigma m(5, 6, 7, 8, 9) + d(10, 11, 12, 13, 14, 15)$$

```text
          CD
 AB      00    01    11    10
 00   |  0  |  0  |  0  |  0  |
 01   |  0  |  1  |  1  |  1  |
 11   |  X  |  X  |  X  |  X  |
 10   |  1  |  1  |  X  |  X  |
```

- Las filas `11` y `10` completas (celdas 8 a 15) forman un **grupo de 8**, donde solo se conserva $A=1$: término $A$.
- $\{m_5, m_7, m_{13}, m_{15}\}$: conserva $B=1$, $D=1$: término $BD$.
- $\{m_6, m_7, m_{14}, m_{15}\}$: conserva $B=1$, $C=1$: término $BC$.

$$F = A + BD + BC = A + B(C + D)$$

**Sin** aprovechar las X, la expresión quedaría $AB'C' + A'BD + A'BC$ (9 literales) frente a los 5 literales de $A + BD + BC$.

---

## 6. Implementación con compuertas universales (NAND y NOR)

Un circuito AND-OR necesita tres tipos de compuerta (AND, OR y NOT). Con compuertas universales se puede construir **con un solo tipo de chip**, lo que reduce el número de circuitos integrados y simplifica la placa. La clave está en las leyes de De Morgan.

### 6.1 Inversor con NAND o NOR

Si se unen las dos entradas de una NAND (o de una NOR), la compuerta actúa como un **NOT**:

$$(A \cdot A)' = A' \qquad\qquad (A + A)' = A'$$

### 6.2 SOP con solo NAND (estructura NAND-NAND)

Una función SOP es una OR de productos. Aplicando doble complemento y De Morgan:

$$F = AB + CD = \big[(AB + CD)'\big]' = \big[(AB)'\cdot(CD)'\big]'$$

Es decir: cada **producto** se convierte en una NAND, y la **suma** final se convierte en otra NAND que recibe todas esas salidas.

```text
 A ──┐
     NAND ──┐
 B ──┘      │
            NAND ── F
 C ──┐      │
     NAND ──┘
 D ──┘
```

**Procedimiento:** (1) simplificar a SOP mínimo; (2) un NAND por cada término producto; (3) un NAND final que junta las salidas. Si algún término es un solo literal $X$, entra a la NAND final **complementado** ($X'$), porque la compuerta final espera el complemento de cada término.

### 6.3 POS con solo NOR (estructura NOR-NOR)

Una función POS es una AND de sumas:

$$F = (A + B)(C + D) = \big[(A+B)' + (C+D)'\big]'$$

Cada **suma** se convierte en una NOR y el **producto** final se convierte en otra NOR.

**Procedimiento:** (1) simplificar a POS mínimo; (2) un NOR por cada término suma; (3) un NOR final que junta las salidas.

### 6.4 Caso de estudio: control de la bomba

La función minimizada de la bomba es $F = B(A + C) = BC + AB$.

**Con NAND (desde el SOP $BC + AB$):**

$$F = \big[(BC)'\cdot(AB)'\big]'$$

Se necesitan 3 NAND: una para $(BC)'$, otra para $(AB)'$ y la final. Cabe en **un solo 74LS00**, que trae 4.

**Con NOR (desde el POS $B(A+C)$):**

$$F = \big[B' + (A+C)'\big]'$$

- NOR de $A$ y $C$ da $(A+C)'$.
- NOR con las entradas unidas de $B$ da $B'$.
- NOR final de ambas da $F$.

Son 3 NOR y también caben en **un 74LS02**, que trae 4.

Con ambas realizaciones se evita mezclar tipos de compuerta. Frente al circuito canónico original (3 minitérminos de 3 literales cada uno, con NOT, AND de 3 entradas y OR de 3 entradas) la reducción es considerable.

---

## Resumen de la semana

1. El **K-Map** coloca la tabla de verdad en una cuadrícula con orden Gray, de modo que las celdas vecinas difieren en una sola variable y los grupos equivalen a aplicar el teorema de adyacencia.
2. Se agrupan **unos** en rectángulos de tamaño 1, 2, 4, 8 o 16 (con vuelta por los bordes y solapes permitidos). Un grupo de $2^k$ celdas elimina $k$ variables y da un **término producto**: SOP mínimo.
3. Se agrupan **ceros** con las mismas reglas, pero cada grupo da un **término suma** con las variables escritas al revés: POS mínimo.
4. Las condiciones **X** se usan como `1` o `0` solo cuando agrandan un grupo; no hace falta cubrirlas todas.
5. Con **NAND-NAND** se implementa un SOP y con **NOR-NOR** un POS, apoyándose en De Morgan. Un NAND o NOR con entradas unidas es un inversor.

---

## Autoevaluación

1. Minimice con un mapa de 3 variables $F = \Sigma m(1, 3, 5, 7)$.
2. Minimice $F = \Sigma m(0, 1, 2, 3, 4, 5)$ y compare con su POS.
3. Minimice $F = \Sigma m(1, 5, 6, 7, 11, 12, 13, 15)$ con 4 variables.
4. Minimice $F = \Sigma m(1, 3, 7) + d(0, 2, 5)$ de 3 variables.
5. Implemente $F = A'B + AB'$ usando solo compuertas NAND.

<details>
<summary>Ver respuestas</summary>

1. La columna $C=1$ completa es un grupo de 4: $F = C$.
2. Unos en $A=0$ (todas) y en $m_4, m_5$ ($A=1$, $B=0$): grupos $A'$ y $B'$, de modo que $F = A' + B'$. Ceros: $m_6, m_7$ → grupo $\{6,7\}$ con $A=1$, $B=1$: término suma $(A' + B')$. El POS es $F = A' + B'$, igual.
3. Cuatro unos solo tienen una forma de agruparse (son esenciales): $\{m_1,m_5\} = A'C'D$, $\{m_6,m_7\} = A'BC$, $\{m_{11},m_{15}\} = ACD$ y $\{m_{12},m_{13}\} = ABC'$. Entre los cuatro ya cubren los 8 unos, así que el bloque central $BD$ **sobra**: $F = A'C'D + A'BC + ACD + ABC'$.
4. $F = \Sigma m(1,3,7)$ y X en 0, 2, 5: tomando $m_5$ y $m_1$, $m_3$, $m_7$ como grupo de 4 ($C=1$) se obtiene $F = C$.
5. Es una XOR. Con 4 NAND: $N_1 = (AB)'$, $N_2 = (A \cdot N_1)'$, $N_3 = (B \cdot N_1)'$, $F = (N_2 \cdot N_3)'$. Alternativamente, desde el SOP: NAND con entradas unidas para $A'$ y $B'$, dos NAND para $(A'B)'$ y $(AB')'$ y una NAND final (5 compuertas).

</details>

