# Semana 5: Encaminamiento y Visualización de Datos con Bloques Combinacionales

**Asignatura:** Diseño de Circuitos · **Unidad 1:** Análisis de Lógica Combinacional con Diseño Clásico

---

## Objetivos de la semana

Al terminar esta semana se debe ser capaz de:

- Diseñar y expandir **multiplexores (MUX)** y usarlos para implementar funciones.
- Diseñar **demultiplexores (DEMUX)** y **decodificadores**, incluida la selección de dispositivos con *Chip Select*.
- Diseñar **codificadores**, con y sin prioridad.
- Diseñar el **decodificador BCD a 7 segmentos** aprovechando las condiciones "no importa".
- Combinar estos bloques con el sumador/restador para armar un pequeño sistema.

> **Conocimientos previos:** BCD y sus combinaciones inválidas (Semana 1); minitérminos y formas canónicas (Semana 2); K-Maps, condiciones "no importa" y NAND (Semana 3); sumador/restador (Semana 4). Aquí solo se aplican.

---

## 1. Multiplexores (MUX)

Interconectar muchos módulos con una línea dedicada para cada uno exigiría una cantidad inviable de cables y pines. Un **multiplexor** permite que varias fuentes compartan **una sola línea**.

Un MUX es un conmutador de **muchos a uno**: tiene $n = 2^s$ **entradas de datos** ($I_0 \dots I_{n-1}$), $s$ **líneas de selección** y **una salida** $Y$. El código binario en las líneas de selección decide cuál entrada pasa a la salida.

$$n = 2^s \qquad\Longleftrightarrow\qquad s = \log_2 n$$

### 1.1 MUX 2:1 y 4:1

**MUX 2:1** ($s = 1$):

| $S$ | $Y$ |
|---|---|
| 0 | $I_0$ |
| 1 | $I_1$ |

$$Y = S'\,I_0 + S\,I_1$$

**MUX 4:1** ($s = 2$, selección $S_1S_0$):

| $S_1$ | $S_0$ | $Y$ |
|---|---|---|
| 0 | 0 | $I_0$ |
| 0 | 1 | $I_1$ |
| 1 | 0 | $I_2$ |
| 1 | 1 | $I_3$ |

$$Y = S_1'S_0'\,I_0 + S_1'S_0\,I_1 + S_1S_0'\,I_2 + S_1S_0\,I_3$$

En general, $Y = \sum_i m_i(S)\cdot I_i$: cada entrada de datos queda "habilitada" por el minitérmino de las líneas de selección que le corresponde. Se implementa con AND-OR: una AND por entrada y una OR final.

### 1.2 Expansión

Un MUX grande se arma con MUX más pequeños en forma de **árbol**. Por ejemplo, un **8:1** con dos 4:1 y un 2:1:

```text
 I0..I3 ──► [MUX 4:1] ──┐
            S1 S0       ├─► [MUX 2:1] ──► Y
 I4..I7 ──► [MUX 4:1] ──┘      S2
            S1 S0
```

Los bits bajos ($S_1S_0$) van a los dos 4:1 y el bit alto ($S_2$) elige entre sus salidas.

### 1.3 El MUX como generador de funciones

Un MUX con $s$ líneas de selección puede implementar **cualquier función**:

- **De $s$ variables:** las variables van a las líneas de selección y cada entrada de datos se conecta a `0` o `1`, según el valor de la función en esa fila. No hace falta ninguna compuerta adicional.
- **De $s + 1$ variables:** las $s$ primeras van a la selección y la última variable se usa **como dato**. Para cada par de filas que comparten selección, la entrada de datos recibe `0`, `1`, la variable o su complemento (esto último puede requerir un inversor).

**Ejemplo 1: la bomba, $F = \Sigma m(3, 6, 7)$ con un MUX 4:1** (selección $S_1 = A$, $S_0 = B$; dato: $C$).

| $A\,B$ | Filas | $F$ en función de $C$ | Entrada |
|---|---|---|---|
| 00 | $m_0, m_1$ | $0, 0$ → `0` | $I_0 = 0$ |
| 01 | $m_2, m_3$ | $0, 1$ → $C$ | $I_1 = C$ |
| 10 | $m_4, m_5$ | $0, 0$ → `0` | $I_2 = 0$ |
| 11 | $m_6, m_7$ | $1, 1$ → `1` | $I_3 = 1$ |

**Ejemplo 2: $F = \Sigma m(1, 2, 6, 7)$**

| $A\,B$ | Filas | $F$ en función de $C$ | Entrada |
|---|---|---|---|
| 00 | $m_0, m_1$ | $0, 1$ → $C$ | $I_0 = C$ |
| 01 | $m_2, m_3$ | $1, 0$ → $C'$ | $I_1 = C'$ |
| 10 | $m_4, m_5$ | $0, 0$ → `0` | $I_2 = 0$ |
| 11 | $m_6, m_7$ | $1, 1$ → `1` | $I_3 = 1$ |

Este último necesita un inversor para $C'$. Se evita todo el proceso de minimización de compuertas, lo que resulta útil cuando importa reducir componentes.

### 1.4 Circuitos integrados comunes

| Chip | Contenido |
|---|---|
| 74LS157 | 4 MUX 2:1 (selector común) |
| 74LS153 | 2 MUX 4:1 |
| 74LS151 | 1 MUX 8:1 |

---

## 2. Decodificadores y demultiplexores (DEMUX)

### 2.1 Decodificador $n$ a $2^n$

Recibe un código de $n$ bits y activa **exactamente una** de sus $2^n$ salidas: aquella cuyo índice coincide con el código. Cada salida es un **minitérmino** de las entradas.

**Decodificador 2:4 con habilitación $E$:**

| $E$ | $A_1$ | $A_0$ | $Y_0$ | $Y_1$ | $Y_2$ | $Y_3$ |
|---|---|---|---|---|---|---|
| 0 | X | X | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 1 | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 | 1 |

$$Y_0 = E\,A_1'A_0' \quad Y_1 = E\,A_1'A_0 \quad Y_2 = E\,A_1A_0' \quad Y_3 = E\,A_1A_0$$

Con $E = 0$ ninguna salida se activa. **Salidas activas en bajo:** en muchos chips TTL la salida seleccionada vale `0` y las demás `1` ($\overline{Y_i}$). Es la convención usual para acoplar con entradas de habilitación que son activas en bajo.

### 2.2 Demultiplexor (DEMUX)

Es el bloque **dual** del MUX: de **uno a muchos**. Toma una única línea de datos $D$ y la entrega a **una** de las $2^s$ salidas, según las $s$ líneas de selección.

$$Y_0 = D\,S_1'S_0' \quad Y_1 = D\,S_1'S_0 \quad Y_2 = D\,S_1S_0' \quad Y_3 = D\,S_1S_0$$

**Observación clave:** las ecuaciones son idénticas a las del decodificador con $E$ en lugar de $D$. Por eso **un decodificador con habilitación funciona como DEMUX**: se usa la entrada $E$ como línea de datos.

| | MUX | DEMUX |
|---|---|---|
| Sentido | Muchas entradas → una salida | Una entrada → muchas salidas |
| Entradas de datos | $2^s$ | 1 |
| Salidas | 1 | $2^s$ |
| Líneas de selección | $s$ | $s$ |

### 2.3 Expansión

Un decodificador de 3 a 8 se arma con dos de 2 a 4. El bit más significativo $A_2$ decide cuál de los dos queda habilitado:

```text
 A1 A0 ─┬──► [Dec 2:4, E = A2'] ──► Y0 .. Y3
        │
        └──► [Dec 2:4, E = A2 ] ──► Y4 .. Y7
```

### 2.4 Implementación de funciones con un decodificador

Como cada salida es un minitérmino, basta **unir con OR** las salidas de los minitérminos de la función. Con un decodificador de **salidas activas en bajo**, se usa una **NAND**:

$$F = \Sigma m(3, 6, 7) = \Big(\overline{Y_3}\cdot\overline{Y_6}\cdot\overline{Y_7}\Big)'$$

(es la NAND de las salidas $\overline{Y_3}$, $\overline{Y_6}$ y $\overline{Y_7}$). Se puede implementar un decodificador $3{:}8$ más una NAND de 3 entradas en lugar de toda una red de compuertas.

### 2.5 Aplicación: selección de dispositivos (*Chip Select*)

Un procesador usa un **bus de direcciones**: un conjunto de líneas cuyo valor indica a qué posición o dispositivo se quiere acceder. Un decodificador toma las líneas de dirección más altas y activa la línea de habilitación $\overline{CS}$ de **un solo** dispositivo (memoria, registro, puerto de E/S).

```text
 Líneas de dirección ──► [Decodificador] ──► CS0' → Memoria A
 (altas)                                    CS1' → Memoria B
                                            CS2' → Puerto de E/S
                                            ...
```

Así nunca se activan dos dispositivos a la vez sobre el mismo bus.

| Chip | Contenido |
|---|---|
| 74LS138 | Decodificador 3:8, salidas activas en bajo, con 3 entradas de habilitación |
| 74LS139 | 2 decodificadores 2:4 |

---

## 3. Codificadores

El **codificador** realiza la función inversa del decodificador: tiene $2^n$ líneas de entrada y entrega en $n$ bits el **código de la línea activa**.

### 3.1 Codificador simple

Supone que **solo una** entrada está activa a la vez. Ejemplo 4:2:

| $D_3$ | $D_2$ | $D_1$ | $D_0$ | $A_1$ | $A_0$ |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 1 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 0 | 0 | 1 | 1 |

$$A_1 = D_3 + D_2 \qquad A_0 = D_3 + D_1$$

**Problemas:** si se activan **dos** entradas a la vez la salida es incorrecta (por ejemplo, $D_3$ y $D_0$ dan `11`, igual que $D_3$ solo), y si **ninguna** está activa la salida `00` se confunde con la entrada $D_0$.

### 3.2 Codificador con prioridad

Soluciona ambos problemas: si hay varias entradas activas, **codifica la de mayor prioridad** (aquí, el índice más alto), y agrega una salida de **validez** $V$ que indica si alguna entrada está activa.

| $D_3$ | $D_2$ | $D_1$ | $D_0$ | $A_1$ | $A_0$ | $V$ |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | X | X | 0 |
| 0 | 0 | 0 | 1 | 0 | 0 | 1 |
| 0 | 0 | 1 | X | 0 | 1 | 1 |
| 0 | 1 | X | X | 1 | 0 | 1 |
| 1 | X | X | X | 1 | 1 | 1 |

$$A_1 = D_3 + D_2 \qquad A_0 = D_3 + D_2'D_1 \qquad V = D_3 + D_2 + D_1 + D_0$$

### 3.3 Aplicación y chip

El caso típico es un **teclado**: cada tecla cierra una línea y el codificador con prioridad entrega el código binario de la tecla pulsada, resolviendo qué hacer si se pulsan dos a la vez. El chip clásico es el **74LS148** (8 a 3, con entradas y salidas activas en bajo).

---

## 4. Decodificador BCD a 7 segmentos

El **display de 7 segmentos** se forma con siete LED ($a$ a $g$) dispuestos así:

```text
   ─ a ─
  f     b
   ─ g ─
  e     c
   ─ d ─
```

- **Cátodo común:** los cátodos van juntos a tierra y un segmento se enciende con `1`.
- **Ánodo común:** los ánodos van juntos a $V_{CC}$ y un segmento se enciende con `0` (salidas activas en bajo).
- Cada segmento necesita su **resistencia limitadora** de corriente (típicamente de 220 a 330 Ω) para no dañar el LED.

### 4.1 Tabla de verdad (cátodo común, 1 = encendido)

Entradas BCD $ABCD$; las combinaciones 10 a 15 son inválidas, es decir, **X** en todas las salidas.

| Dígito | $ABCD$ | $a$ | $b$ | $c$ | $d$ | $e$ | $f$ | $g$ |
|---|---|---|---|---|---|---|---|---|
| 0 | 0000 | 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| 1 | 0001 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| 2 | 0010 | 1 | 1 | 0 | 1 | 1 | 0 | 1 |
| 3 | 0011 | 1 | 1 | 1 | 1 | 0 | 0 | 1 |
| 4 | 0100 | 0 | 1 | 1 | 0 | 0 | 1 | 1 |
| 5 | 0101 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
| 6 | 0110 | 1 | 0 | 1 | 1 | 1 | 1 | 1 |
| 7 | 0111 | 1 | 1 | 1 | 0 | 0 | 0 | 0 |
| 8 | 1000 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| 9 | 1001 | 1 | 1 | 1 | 1 | 0 | 1 | 1 |

Es un circuito de **4 entradas y 7 salidas**: se simplifica **cada segmento** con su propio K-Map, usando las X (método de la Semana 4 para varias salidas).

### 4.2 Ejemplo: segmento $a$

$a = \Sigma m(0, 2, 3, 5, 6, 7, 8, 9) + d(10, 11, 12, 13, 14, 15)$

```text
          CD
 AB      00    01    11    10
 00   |  1  |  0  |  1  |  1  |
 01   |  0  |  1  |  1  |  1  |
 11   |  X  |  X  |  X  |  X  |
 10   |  1  |  1  |  X  |  X  |
```

- Filas `11` y `10` completas (celdas 8 a 15): grupo de 8 → $A$.
- Columnas `11` y `10` completas (celdas 2, 3, 6, 7, 10, 11, 14, 15): grupo de 8 → $C$.
- Bloque central ($m_5, m_7, m_{13}, m_{15}$) → $BD$.
- Esquinas ($m_0, m_2, m_8, m_{10}$) → $B'D'$.

$$a = A + C + BD + B'D'$$

Los demás segmentos se obtienen igual (ver autoevaluación para $g$).

### 4.3 Con ánodo común y chip integrado

Para un display de ánodo común hay que **invertir** las salidas (activas en bajo). Los chips comerciales ya lo hacen: el **74LS47** (salidas activas en bajo, ánodo común) y el **74LS48** (salidas activas en alto, cátodo común).

> **Cuidado:** este decodificador solo muestra correctamente valores BCD de 0 a 9. Un resultado binario de 4 bits mayor que 9 (por ejemplo, el de un sumador) **no se verá bien** sin una conversión previa.

---

## 5. Integración: un sistema sencillo

Los bloques de las semanas anteriores y de esta se combinan en un sistema de "selección, cálculo y visualización":

```text
 Puerto X ─┐
           ├► [MUX 2:1 ×4 bits] ─► B ─┐
 Puerto Y ─┘     (selecciona)          ├► [Sumador/Restador] ─► R ─► [Dec. BCD-7seg] ─► Display
                                A ─────┘        ▲ Modo                   ▲
                                                                       sólo 0 a 9
```

1. **MUX** (74LS157): elige de qué puerto proviene el segundo operando.
2. **Sumador/restador** (Semana 4): calcula $A + B$ o $A - B$ según `Modo`.
3. **Decodificador BCD a 7 segmentos:** muestra el resultado, siempre que esté en el rango 0 a 9.
4. Un **decodificador con $\overline{CS}$** puede además elegir cuál de varios dispositivos de salida recibe el resultado.

---

## Resumen de la semana

1. El **MUX** ($2^s$ entradas, $s$ selectores, 1 salida) comparte una línea entre varias fuentes. Se expande en árbol y es un **generador universal de funciones**: $s$ selectores implementan funciones de $s$ variables, o de $s+1$ con la última variable como dato.
2. El **decodificador** $n$ a $2^n$ activa una sola salida, que es un minitérmino; con habilitación $E$ también funciona como **DEMUX**. Se expande con la entrada $E$ y permite implementar funciones y generar los $\overline{CS}$ de los dispositivos.
3. El **codificador** hace la función inversa; el **codificador con prioridad** resuelve las entradas múltiples y añade la salida de validez $V$.
4. El **decodificador BCD a 7 segmentos** son 7 funciones de 4 variables, simplificadas con las X de los códigos 10 a 15. Hay que distinguir cátodo común de ánodo común.
5. Estos bloques se integran con los aritméticos de la semana anterior en sistemas de selección, cálculo y visualización.

---

## Autoevaluación

1. ¿Cuántas líneas de selección tiene un MUX 16:1 y cuántos MUX 4:1 se necesitan para armarlo?
2. Implemente $F = \Sigma m(0, 3, 5, 6)$ con un MUX 4:1 (selección $A$, $B$; dato $C$).
3. Un codificador con prioridad $4{:}2$ recibe $D_3D_2D_1D_0 = 0110$. ¿Cuánto valen $A_1$, $A_0$ y $V$?
4. Obtenga la expresión mínima del segmento $g$ del decodificador BCD a 7 segmentos.
5. Describa cómo armar un decodificador 3:8 con dos decodificadores 2:4.
6. ¿Qué pasa si a un decodificador BCD a 7 segmentos le llega `1011`?

<details>
<summary>Ver respuestas</summary>

1. $s = \log_2 16 = 4$ líneas de selección. Se necesitan 5 MUX 4:1: cuatro en la primera etapa (16 entradas) y uno en la segunda.
2. $I_0 = C'$ (filas 0, 1: $1, 0$), $I_1 = C$ (filas 2, 3: $0, 1$), $I_2 = C$ (filas 4, 5: $0, 1$) e $I_3 = C'$ (filas 6, 7: $1, 0$).
3. Está activa la entrada de mayor índice, $D_2$: $A_1 = 1$, $A_0 = 0$ (código `10`) y $V = 1$.
4. $g = \Sigma m(2,3,4,5,6,8,9) + d(10,\dots,15)$. Grupos: $A$ (celdas 8 a 15), $B'C$ (2, 3, 10, 11), $BC'$ (4, 5, 12, 13) y $CD'$ (2, 6, 10, 14): $g = A + B'C + BC' + CD'$.
5. Se conectan $A_1A_0$ a ambos; el primero se habilita con $E = A_2'$ y da $Y_0$ a $Y_3$; el segundo con $E = A_2$ y da $Y_4$ a $Y_7$.
6. $1011_2$ (11) no es un dígito BCD válido: es una condición X. Los segmentos que se encienden dependen de cómo se resolvieron las X en el diseño (o del chip); el display mostrará un símbolo arbitrario.

</details>

---
