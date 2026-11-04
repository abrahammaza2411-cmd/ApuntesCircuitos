# APUNTES DE ESTUDIO: FUNDAMENTOS DE LÓGICA COMBINACIONAL Y DISEÑO DE CIRCUITOS

---

## 1. TÍTULO Y CONTEXTO

La lógica combinacional constituye el pilar estructural sobre el cual se erige la arquitectura del hardware computacional moderno y el procesamiento digital de información. A diferencia de los sistemas secuenciales, en la lógica combinacional las salidas del sistema responden de manera exclusiva e instantánea a las combinaciones presentes en sus entradas en un momento dado, sin guardar memoria de estados pasados ni depender de señales de reloj. Esta capacidad operacional permite desde la ejecución de cálculos aritméticos elementales en las Unidades Aritmético Lógicas (ALU) hasta la toma de decisiones complejas en sistemas de control industrial, traduciendo fenómenos del mundo físico en estados abstractos manipulables mediante leyes matemáticas rigurosas.

### Diseño de Circuitos: Unidad 1 — Análisis de Lógica Combinacional con Diseño Clásico

El presente documento constituye un compendio académico exhaustivo formulado para la asignatura **Diseño de Circuitos**, perteneciente al 2.° Ciclo de la Carrera de Computación de la Universidad Nacional de Loja, bajo la instrucción del **Ing. Luis Sinche Cueva** para el período académico septiembre 2026 – marzo 2027. El material integra el desarrollo analítico de la Unidad 1 (*"Análisis de lógica combinacional con diseño clásico"*), abarcando desde los fundamentos del sistema binario, niveles de voltaje, márgenes de ruido y conversión de bases, hasta la representación de números con signo, códigos de comunicación digital, caracterización física de la familia lógica TTL 74LS y la simplificación de funciones mediante álgebra booleana.

Para abordar con solvencia técnica el comportamiento de los circuitos digitales y la ejecución de modelos algebraicos, resulta indispensable clarificar primeramente el vocabulario especializado y las magnitudes físicas que gobiernan este campo de la ingeniería.

---

## 2. CONCEPTOS CLAVE Y DEFINICIONES (GLOSARIO)

El dominio riguroso de la ingeniería de hardware exige una comprensión clara de la terminología técnica y de las magnitudes físicas y lógicas que intervienen en el procesamiento digital. Antes de analizar esquemas de integración masiva o ecuaciones algebraicas avanzadas, es necesario establecer las definiciones fundamentales que garantizan la correcta interpretación de las señales, la inmunidad ante fallas de diseño y la estandarización de los componentes electrónicos.

* **Lógica Combinacional:** Paradigma de diseño de circuitos digitales donde el valor de las salidas depende única y exclusivamente de la combinación de los valores presentes en las entradas en ese mismo instante de tiempo, sin recurrir a elementos de almacenamiento o memoria.
* **Transistor:** Dispositivo semiconductor fundamental que opera conceptualmente como un interruptor controlado por voltaje, permitiendo o bloqueando el paso de corriente eléctrica para representar los estados lógicos base ($0$ y $1$).
* **Margen de Ruido (*Noise Margin*):** Tolerancia o colchón de seguridad en los niveles de voltaje de una familia lógica que previene que las interferencias eléctricas externas o variaciones de señal alteren la correcta interpretación de un estado lógico en el receptor.
* **Zona Indefinida:** Rango de voltaje intermedio (comprendido entre $0.8\text{ V}$ y $2.0\text{ V}$ en la familia TTL 74LS) donde la compuerta lógica receptora no garantiza la lectura fija de un $0$ o un $1$, pudiendo generar comportamientos erráticos.
* **Sistema Posicional:** Sistema de numeración en el cual cada dígito posee un valor determinado tanto por su propia magnitud como por la posición relativa que ocupa respecto a la base del sistema elevada a una potencia dada.
* **Complemento a Dos:** Convenio binario y algoritmo de cómputo empleado en hardware para representar enteros con signo, el cual permite ejecutar operaciones de resta utilizando la misma estructura física de un circuito sumador.
* **Desbordamiento (*Overflow*):** Condición de error aritmético que ocurre cuando el resultado de una operación excede el rango dinámico o la capacidad física de almacenamiento en bits del registro asignado.
* **Código Gray:** Código binario no ponderado en el cual dos números adyacentes consecutivos difieren en el estado de un solo bit, diseñado para prevenir lecturas transitorias falsas en sensores mecánicos u ópticos.
* **Código BCD (*Binary Coded Decimal*):** Estándar de codificación en el cual cada uno de los diez dígitos del sistema decimal ($0$ a $9$) se representa de forma independiente mediante un grupo fijo de $4\text{ bits}$.
* **Código ASCII:** Tabla de codificación estándar de $7\text{ bits}$ que asigna patrones binarios específicos a $128$ caracteres, incluyendo letras, números, símbolos de puntuación y comandos de control.
* **Compuerta Universal:** Compuerta lógica (específicamente NAND o NOR) que posee la propiedad teórica y práctica de poder sintetizar e implementar cualquier función booleana sin requerir otros tipos de compuertas.
* **Encapsulado DIP-14:** Formato físico de circuito integrado (*Dual In-line Package*) que consta de $14$ pines distribuidos en dos filas paralelas, estándar en los chips de la serie TTL 74LS para montaje en protoboard.

Una vez delimitado este marco conceptual, se procede a profundizar en el análisis técnico y matemático de las variables físicas que sustentan el procesamiento binario.

---

## 3. DESARROLLO DE TEMAS PRINCIPALES

### 3.1. Representación Binaria, Niveles de Voltaje y Margen de Ruido

Los sistemas de computación basan su funcionamiento en el sistema binario debido a la naturaleza física de sus componentes elementales: los transistores de unión bipolar (BJT) o de efecto de campo (FET). Un transistor opera fundamentalmente como un interruptor gobernado por voltaje. Representar la información mediante solo dos estados ($0$ y $1$) ofrece una altísima inmunidad a las perturbaciones en comparación con sistemas analógicos o de múltiples niveles, ya que solo se requiere diferenciar entre dos condiciones eléctricas claramente distinguidas: un interruptor abierto donde no circula corriente y prevalece un voltaje bajo (asignado al valor $0$), y un interruptor cerrado donde circula corriente y se establece un voltaje alto (asignado al valor $1$).

En la práctica de la ingeniería electrónica, un "$1$" o un "$0$" lógico no corresponden a un valor de voltaje rígido o exacto, sino a rangos definidos por los fabricantes para garantizar la interoperabilidad entre circuitos integrados. La familia lógica TTL 74LS (*Transistor-Transistor Logic Low-power Schottky*) establece límites claros para los voltajes que una compuerta emite (salida prometida) y los voltajes que otra compuerta es capaz de interpretar (entrada exigida).

| Especificación de Voltaje | Estado Lógico Alto (1) | Estado Lógico Bajo (0) |
| :--- | :---: | :---: |
| **Salida Prometida (Compuerta Emisora)** | $V_{OH} \ge 2.7\text{ V}$ | $V_{OL} \le 0.5\text{ V}$ |
| **Entrada Exigida (Compuerta Receptora)** | $V_{IH} \ge 2.0\text{ V}$ | $V_{IL} \le 0.8\text{ V}$ |

La disparidad prefijada entre los voltajes emitidos y los exigidos crea una franja de protección denominada **Margen de Ruido**, la cual actúa como un colchón de seguridad frente a interferencias electromagnéticas o caídas de tensión en las líneas de conexión:

* **Margen de Ruido en Nivel Alto ($N_{MH}$):**
  $$N_{MH} = V_{OH} - V_{IH} = 2.7\text{ V} - 2.0\text{ V} = 0.7\text{ V}$$
  *Significado:* Una señal en nivel alto ($1$) puede degradarse o perder hasta $0.7\text{ V}$ a lo largo del trayecto y el circuito receptor la seguirá interpretando correctamente como un $1$.

* **Margen de Ruido en Nivel Bajo ($N_{ML}$):**
  $$N_{ML} = V_{IL} - V_{OL} = 0.8\text{ V} - 0.5\text{ V} = 0.3\text{ V}$$
  *Significado:* Una señal en nivel bajo ($0$) puede absorber picos de ruido e incrementarse hasta en $0.3\text{ V}$ sin perder su condición de $0$ lógico.

```text
       SALIDA (Promete)               ENTRADA (Exige)
  5.0 V +--------------+         5.0 V +--------------+
        |              |               |              |
        |  Sale un 1   |               |  Lee un 1    |
        |  (≥ 2.7 V)   |               |  (≥ 2.0 V)   |
  2.7 V +--------------+         2.0 V +--------------+
        | Solo de paso |               | ZONA         | (0.8 V - 2.0 V)
  0.5 V +--------------+         0.8 V +--------------+ Indefinida
        |  Sale un 0   |               |  Lee un 0    |
        |  (≤ 0.5 V)   |               |  (≤ 0.8 V)   |
    0 V +--------------+           0 V +--------------+
```

El rango comprendido entre $0.8\text{ V}$ y $2.0\text{ V}$ en la entrada de la compuerta se conoce como **Zona Indefinida**. En este intervalo, la respuesta de la compuerta no está garantizada, pudiendo fluctuar aleatoriamente entre $0$ y $1$. En la tecnología TTL Bipolar, cuando una entrada se deja "flotante" (sin conectar a nada), la juntura base-emisor interna del transistor no tiene un camino de descarga a tierra; en consecuencia, tiende a flotar hacia un estado indeterminado o actuar como una antena receptora de ruido electromagnético ambiental, arrastrando el voltaje dentro de la zona indefinida y provocando conmutación de estados no deseada.

#### Caso Práctico de Falla por Ruido Inducido:
Considérese una compuerta que emite un estado bajo de $V_{OL} = 0.5\text{ V}$. Si un motor eléctrico cercano se enciende e induce un pico de ruido electromagnético de $0.4\text{ V}$ sobre la línea de conexión, el voltaje real que alcanza la entrada del siguiente chip será:

$$V_{real} = 0.5\text{ V} + 0.4\text{ V} = 0.9\text{ V}$$

Dado que $0.9\text{ V}$ se encuentra dentro de la zona indefinida ($0.8\text{ V} < 0.9\text{ V} < 2.0\text{ V}$), la compuerta receptora no garantiza la lectura de un $0$, provocando un error sistemático de procesamiento en el hardware.

---

### 3.2. Sistemas Numéricos Posicionales y Conversiones de Base

Los sistemas numéricos posicionales son la base formal para la representación, almacenamiento y direccionamiento de la información en las arquitecturas de cómputo. A diferencia de los sistemas no posicionales —como la numeración romana, donde el símbolo $\text{V}$ representa siempre el valor $5$ independientemente de su posición, haciendo sumamente complejas las operaciones aritméticas básicas—, en un sistema posicional el valor de cada dígito depende de su ubicación relativa, la cual multiplica a una potencia de la base del sistema.

En el sistema binario (base 2), cada bit representa una potencia consecutiva de $2$. Para un grupo de $8\text{ bits}$ ($1\text{ Byte}$), los pesos posicionales de derecha a izquierda corresponden estrictamente a:

$$2^7, 2^6, 2^5, 2^4, 2^3, 2^2, 2^1, 2^0 \longrightarrow 128, 64, 32, 16, 8, 4, 2, 1$$

Por ejemplo, el número binario $1011_2$ equivale en decimal a:

$$1011_2 = (1 \times 8) + (0 \times 4) + (1 \times 2) + (1 \times 1) = 8 + 0 + 2 + 1 = 11_{10}$$

#### Algoritmos de Conversión de Base:

1. **Decimal a Binario (Divisiones Sucesivas):** Se divide el número decimal entre $2$ de forma iterativa, registrando los cocientes y los restos. El proceso finaliza cuando el cociente es $0$. El equivalente binario se obtiene leyendo los restos de abajo hacia arriba (último resto como Bit Más Significativo - MSB).

   Ejemplo completo con el número $25_{10}$:
   $$
   \begin{aligned}
   25 \div 2 &= 12 \quad &\text{Resto: } 1 \\
   12 \div 2 &= 6 \quad &\text{Resto: } 0 \\
   6 \div 2 &= 3 \quad &\text{Resto: } 0 \\
   3 \div 2 &= 1 \quad &\text{Resto: } 1 \\
   1 \div 2 &= 0 \quad &\text{Resto: } 1
   \end{aligned}
   $$
   Leyendo los restos de abajo hacia arriba: $25_{10} = 11001_2$.  
   *Comprobación por pesos de potencias:* $16 + 8 + 1 = 25$.

2. **Binario a Hexadecimal (Base 16) y Octal (Base 8):** Los sistemas hexadecimal y octal actúan como notaciones abreviadas de lectura rápida para el binario.
   * **Hexadecimal:** Se agrupan los bits de $4$ en $4$ comenzando estrictamente desde la derecha. Si el grupo de la izquierda está incompleto, se rellenan las posiciones faltantes con ceros.
   * **Octal:** Se agrupan los bits de $3$ en $3$ comenzando desde la derecha, aplicando el mismo criterio de rellenado con ceros.

Para las conversiones a hexadecimal, se utiliza la tabla de equivalencias a partir del valor $10$:

| Decimal | Hexadecimal |
| :---: | :---: |
| 10 | A |
| 11 | B |
| 12 | C |
| 13 | D |
| 14 | E |
| 15 | F |

*Ejemplo de desglose con el número $157_{10} = 10011101_2$:*
* **A Hexadecimal:** $\underbrace{1001}_{9} \ \underbrace{1101}_{\text{D}} \longrightarrow 9\text{D}_{16}$.
* **A Octal:** $\underbrace{010}_{2} \ \underbrace{011}_{3} \ \underbrace{101}_{5} \longrightarrow 235_8$.

#### Resolución Completa del Reto de Conversión:

1. **Convertir $45_{10}$ a binario y hexadecimal:**
   * Divisiones sucesivas: $45 \div 2 = 22 \text{ (R1)}$, $22 \div 2 = 11 \text{ (R0)}$, $11 \div 2 = 5 \text{ (R1)}$, $5 \div 2 = 2 \text{ (R1)}$, $2 \div 2 = 1 \text{ (R0)}$, $1 \div 2 = 0 \text{ (R1)} \longrightarrow 101101_2$.
   * Agrupación hexadecimal ($4\text{ bits}$): $\underbrace{0010}_{2} \ \underbrace{1101}_{\text{D}} \longrightarrow 2\text{D}_{16}$.

2. **Convertir $\text{A}7_{16}$ a decimal:**
   * $\text{A} = 10$, $7 = 7$.
   * $(10 \times 16^1) + (7 \times 16^0) = 160 + 7 = 167_{10}$.

3. **Convertir $11100101_2$ a hexadecimal y octal:**
   * Hexadecimal ($4\text{ bits}$): $\underbrace{1110}_{\text{E}} \ \underbrace{0101}_{5} \longrightarrow \text{E}5_{16}$.
   * Octal ($3\text{ bits}$): $\underbrace{011}_{3} \ \underbrace{100}_{4} \ \underbrace{101}_{5} \longrightarrow 345_8$.

---

### 3.3. Aritmética Binaria y Representación de Números con Signo (Complemento a Dos)

Los circuitos digitales carecen de la capacidad física de almacenar un símbolo negativo "$-$" . Para procesar valores negativos sin duplicar la complejidad del hardware diseñando circuitos restadores independientes, la arquitectura de computadores emplea la representación en **Complemento a Dos**. La ventaja física crítica de esta convención es que permite reutilizar el mismo circuito sumador digital para ejecutar restas ($A - B = A + (-B)$); basta con pasar los bits del sustraendo por compuertas inyectoras e inverters (XOR/NOT) e inyectar un valor de acarreo $1$ ($C_{in} = 1$) en la primera etapa del sumador.

#### Suma Binaria Elemental:
La suma binaria sigue cuatro reglas básicas de acarreo:
* $0 + 0 = 0$
* $0 + 1 = 1$
* $1 + 1 = 10_2 \quad (\text{Escribo } 0 \text{ y llevo acarreo } 1)$
* $1 + 1 + 1 = 11_2 \quad (\text{Escribo } 1 \text{ y llevo acarreo } 1)$

Desarrollo formal de la suma $11 + 6 = 17$ en binario:

$$
\begin{array}{rccccc}
\text{Acarreo:} & 1 & 1 & 1 & & \\
& 1 & 0 & 1 & 1 & (11_{10}) \\
+ & 0 & 1 & 1 & 0 & (6_{10}) \\
\hline
& 1 & 0 & 0 & 0 & 1 & (17_{10})
\end{array}
$$

#### Principio del Complemento a Dos:
El concepto se fundamenta en la analogía de un cuentakilómetros analógico. Si un cuentakilómetros en $000$ retrocede una posición hacia atrás, marca $999$; por ende, $999$ actúa operativamente como $-1$. En un sistema binario limitado a $4\text{ bits}$, si a $0000_2$ se le resta $1$, el registro pasa a $1111_2$. Bajo el convenio de complemento a dos, $1111_2$ representa el $-1$, $1110_2$ representa el $-2$, y así sucesivamente.

El bit situado más a la izquierda (Bit Más Significativo - MSB) actúa como bit de signo:
* Si el MSB es $0$, el número es positivo.
* Si el MSB es $1$, el número es negativo.

**Rango de Representación según el Número de Bits ($N$):**
* Para $N = 4\text{ bits}$: Rango desde $-2^{N-1}$ hasta $+2^{N-1}-1 \longrightarrow -8 \text{ a } +7$.
* Para $N = 8\text{ bits}$: Rango desde $-2^{7}$ hasta $+2^{7}-1 \longrightarrow -128 \text{ a } +127$.

| Patrón (4 bits) | Valor Decimal | Patrón (4 bits) | Valor Decimal |
| :---: | :---: | :---: | :---: |
| 0000 | 0 | 1000 | -8 |
| 0001 | +1 | 1001 | -7 |
| 0010 | +2 | 1010 | -6 |
| 0011 | +3 | 1011 | -5 |
| 0100 | +4 | 1100 | -4 |
| 0101 | +5 | 1101 | -3 |
| 0110 | +6 | 1110 | -2 |
| 0111 | +7 | 1111 | -1 |

#### Algoritmo para obtener el Negativo de un Número:
1. Tomar la representación binaria del número positivo.
2. Invertir todos los bits (operación NOT o Complemento a Uno).
3. Sumar $1$ al resultado obtenido.

Obtención de $-3$ en $4\text{ bits}$ paso a paso:
$$
\begin{aligned}
+3 \text{ en binario:} &\quad 0011_2 \\
\text{Invertir bits (NOT):} &\quad 1100_2 \\
\text{Sumar 1:} &\quad 1100_2 + 1 = 1101_2 \quad (\text{representación de } -3)
\end{aligned}
$$

Comprobación de la operación $5 - 3 = 5 + (-3) = 2$:
$$
\begin{array}{rccccc}
& 0 & 1 & 0 & 1 & (+5) \\
+ & 1 & 1 & 0 & 1 & (-3) \\
\hline
\mathbf{1} & 0 & 0 & 1 & 0 & (+2)
\end{array}
$$
*Regla de Hardware:* El bit de acarreo saliente por la izquierda ($\mathbf{1}$) que excede el tamaño asignado al registro de $4\text{ bits}$ se descarta automáticamente, arrojando el resultado correcto $0010_2 = +2$.

#### Desbordamiento (*Overflow*):
El desbordamiento ocurre cuando el resultado de una operación aritmética excede la capacidad física de representación del número de bits asignado al registro. Se detecta conceptualmente cuando la suma de dos números positivos resulta en un valor negativo, o cuando la suma de dos números negativos arroja un valor positivo.

Ejemplo de falla por desbordamiento en $4\text{ bits}$ ($7 + 1$):
$$
\begin{array}{rcccc}
& 0 & 1 & 1 & 1 & (+7) \\
+ & 0 & 0 & 0 & 1 & (+1) \\
\hline
& 1 & 0 & 0 & 0 & (\text{Incorrecto: } 1000_2 = -8)
\end{array}
$$

El resultado ha invadido el bit de signo. Este fenómeno se proyecta idénticamente en el software de alto nivel. Por ejemplo, en Java, una variable de tipo `int` utiliza $32\text{ bits}$ en complemento a dos:

```java
int x = 2147483647; // Valor máximo positivo representable (2^31 - 1)
x = x + 1;         // Evalúa a -2147483648 debido al desbordamiento del bit de signo
```

---

### 3.4. Códigos Binarios: BCD, Gray y ASCII

La necesidad de comunicar los circuitos de lógica binaria pura con interfaces humano-máquina o sensores de precisión exige traducir los estados binarios a códigos de comunicación especializados.

#### Código BCD (*Binary Coded Decimal*):
El código BCD simplifica la interfaz con dispositivos de visualización (displays de 7 segmentos en relojes, calculadoras y contadores). Cada dígito del sistema decimal ($0$ al $9$) se representa de manera independiente mediante su equivalente directo en $4\text{ bits}$.

*Ejemplo de conversión de $157_{10}$ en BCD:*
$$1 \longrightarrow 0001_{BCD}, \quad 5 \longrightarrow 0101_{BCD}, \quad 7 \longrightarrow 0111_{BCD}$$
$$157_{BCD} = 0001 \ 0101 \ 0111$$

* **Diferencia técnica:** En binario puro, $157_{10}$ se codifica como $10011101_2$.
* **Restricción de diseño:** Las combinaciones comprendidas entre $1010_2$ ($10_{10}$) y $1111_2$ ($15_{10}$) no son válidas en BCD y representan estados prohibidos o invalidados por el hardware.

#### Código Gray:
El código Gray es una secuencia binaria no ponderada caracterizada porque entre dos valores consecutivos adyacentes la representación varía en un solo bit.

* **Problema del binario natural en sensores:** Al pasar del número $3$ ($011_2$) al $4$ ($100_2$), cambian $3\text{ bits}$ simultáneamente. En un encoder rotativo o perilla de precisión, si los contactos mecánicos u ópticos no conmutan al mismo milisegundo exacto, el sistema puede interpretar temporalmente lecturas falsas erráticas (tales como $111_2$ o $000_2$). El código Gray elimina por completo este riesgo transitorio.
* **Secuencia y Método del Espejo:** Para construir una tabla de código Gray se refleja inductivamente la lista previa en orden inverso, anteponiendo ceros a la mitad superior y unos a la mitad inferior:

```text
1 bit:      2 bits:     3 bits:
  0           00          000
  1           01          001
             ----         011
              11          010
              10         -----
                          110
                          111
                          101
                          100
```

Además de su uso en sensores de posición, la propiedad de cambio de un solo bit constituye el fundamento formal del ordenamiento de filas y columnas ($00, 01, 11, 10$) en los Mapas de Karnaugh.

#### Código ASCII (*American Standard Code for Information Interchange*):
Es una tabla de codificación estándar de $7\text{ bits}$ ($128$ combinaciones posibles: $2^7$) diseñada para intercambiar caracteres alfanuméricos, símbolos matemáticos y comandos de control entre equipos digitales.

*Representación de la palabra "HOLA" en ASCII:*
* **H:** $72_{10} = 0\text{x}48_{16} = 1001000_2$
* **O:** $79_{10} = 0\text{x}4\text{F}_{16} = 1001111_2$
* **L:** $76_{10} = 0\text{x}4\text{C}_{16} = 1001100_2$
* **A:** $65_{10} = 0\text{x}41_{16} = 1000001_2$

Cualquier dispositivo digital almacena la letra "A" asignando el número entero $65_{10} = 01000001_2$. Como el estándar ASCII de $7\text{ bits}$ no contempla caracteres regionales (como la 'ñ' o tildes), los sistemas modernos emplean extensiones universales como UTF-8.

---

### 3.5. Compuertas Lógicas y Circuitos Integrados TTL (Familia 74LS)

Las compuertas lógicas representan los bloques constructivos elementales que procesan funciones booleanas dentro de un circuito combinacional. En el desarrollo práctico del curso, la simulación de estas compuertas y circuitos combinacionales se efectúa oficialmente mediante el software **Logisim-evolution**, previo a su ensamble físico en protoboard.

#### Clasificación y Especificaciones Técnicas de las 7 Compuertas Lógicas:

1. **Básicas:**
   * **NOT (Inversor):** $Y = A'$. Invierte el estado lógico de entrada ($A=0 \rightarrow Y=1$; $A=1 \rightarrow Y=0$).
   * **AND:** $Y = A \cdot B$. La salida es $1$ solo si TODAS las entradas valen $1$.
   * **OR:** $Y = A + B$. La salida es $1$ si AL MENOS UNA de las entradas vale $1$.

2. **Negadas:**
   * **NAND:** $Y = (A \cdot B)'$. La salida es $0$ solo si todas las entradas valen $1$.
   * **NOR:** $Y = (A + B)'$. La salida es $1$ solo si ninguna entrada vale $1$ (todas en $0$).
   * **Compuertas Universales:** NAND y NOR poseen la propiedad de universalidad, pudiendo sintetizar cualquier función booleana sin requerir otros tipos de compuertas. El círculo en la terminal de salida simboliza negación lógica.

3. **Comparación:**
   * **XOR (OR Exclusiva):** $Y = A \oplus B$. La salida es $1$ únicamente si sus entradas son DISTINTAS. (Ejemplo práctico: control de iluminación de escalera mediante dos conmutadores independientes, donde accionar cualquiera altera el estado de la lámpara).
   * **XNOR (NOR Exclusiva):** $Y = (A \oplus B)'$. La salida es $1$ únicamente si sus entradas son IGUALES.

#### Normas de Representación Gráfica:
Existen dos simbologías internacionales normalizadas: **ANSI/IEEE** (emplea formas geométricas distintivas para cada función, norma estándar utilizada en la materia) e **IEC** (utiliza rectángulos con símbolos internos: $1$ para NOT, $\&$ para AND, $\ge 1$ para OR, y $=1$ para XOR).

```text
         ANSI / IEEE                             IEC
          (Forma)                           (Rectangular)
NOT        ---|>o---                            [ 1 ]o
AND        ---|)-                               [ & ]-
OR         ---|=)-                              [>=1]-
NAND       ---|)o-                              [ & ]o
NOR        ---|=)o-                             [>=1]o
XOR        ---||=)-                             [ =1]-
XNOR       ---||=)o-                            [ =1]o
```

#### Estructura Física y Pinado del Circuito Integrado TTL 74LS08:
El chip 74LS08 alberga cuatro compuertas AND de 2 entradas independientes en un encapsulado DIP-14.

```text
                +---\__/---+
           1A --| 1      14 |-- VCC (+5V)
           1B --| 2      13 |-- 4B
           1Y --| 3      12 |-- 4A
           2A --| 4      11 |-- 4Y
           2B --| 5      10 |-- 3B
           2Y --| 6       9 |-- 3A
     (GND) GND -| 7       8 |-- 3Y
                +----------+
```

* **Identificación de pines:** Orientando la muesca semicircular hacia arriba, el Pin 1 se ubica en la esquina superior izquierda y la numeración avanza en sentido antihorario hasta el Pin 14.
* **Conexiones de alimentación obligatorias:** Pin 14 conectado a $V_{CC} = +5\text{ V}$ y Pin 7 a Tierra (GND).

#### Desglose del Código Nomenclaturado de un Chip (`SN74LS08N`):
* **SN:** Prefijo del fabricante (Texas Instruments).
* **74:** Serie comercial para rango de temperatura de $0^\circ\text{C}$ a $70^\circ\text{C}$ ($54$ indica especificación militar).
* **LS:** Familia/Tecnología de fabricación (*Low-power Schottky TTL*; frente a la serie HC, que indica tecnología CMOS de alta velocidad, como el 74HC08, que mantiene idéntico pinado y función).
* **08:** Identificador funcional de la compuerta interna (4 compuertas AND de 2 entradas).
* **N:** Tipo de encapsulado físico (DIP plástico para protoboard).

---

### 3.6. Álgebra Booleana y Casos Prácticos de Aplicación

La simplificación mediante Álgebra Booleana optimiza el número de compuertas lógicas requeridas en un circuito combinacional, reduciendo los costos de manufactura, la superficie del circuito impreso y el consumo energético.

#### Ejercicio 1: Sistema de Control de Bomba para Cisterna y Tanque

**Planteamiento del problema:** Se requiere diseñar un sistema de control de automatización para una bomba de agua cuya salida $F$ ($1 =$ encender bomba) depende de tres señales de entrada enviadas por sensores/switches:
* **A (Tanque bajo):** El sensor envía un $1$ cuando el tanque elevado requiere agua (nivel bajo activo).
* **B (Cisterna con agua):** Sensor de presencia de agua en la cisterna.
* **C (Mando Manual):** El operador activa una señal $1$ para forzar el bombeo manual.

*Criterios de lógica de sensores y convenciones:* Los sensores físicos pueden ser normalmente abiertos ($0\text{ V}$ en reposo, $5\text{ V}$ al activarse) o normalmente cerrados. Según se detalla en las notas del curso, el diseñador puede seleccionar convenciones específicas como asociar la condición de cisterna llena a $0\text{ V}$ ($0$) y vacía a $5\text{ V}$ ($1$), o viceversa, según la arquitectura de seguridad requerida. Para este modelo de diseño, la variable $B=1$ representa la condición que convalida operativamente la presencia de agua en la cisterna, habilitando el encendido seguro de la bomba.

**Tabla de Verdad del Sistema (8 Combinaciones posibles):**

| A | B | C | F | Minitérmino / Maxitérmino | Análisis de Ingeniería |
| :-: | :-: | :-: | :-: | :---: | :--- |
| 0 | 0 | 0 | 0 | $M_0$ | Cisterna vacía, no hay demanda en tanque. |
| 0 | 0 | 1 | 0 | $M_1$ | Cisterna vacía; intento manual bloqueado por protección. |
| 0 | 1 | 0 | 0 | $M_2$ | Cisterna con agua, pero tanque lleno. |
| 0 | 1 | 1 | 1 | $m_3$ | Encendido manual con agua en cisterna. |
| 1 | 0 | 0 | 0 | $M_4$ | Tanque pide agua, pero cisterna vacía (bloqueo por aire). |
| 1 | 0 | 1 | 0 | $M_5$ | Cisterna vacía; bloqueo general. |
| 1 | 1 | 0 | 1 | $m_6$ | Llenado automático (tanque requiere agua y cisterna con agua). |
| 1 | 1 | 1 | 1 | $m_7$ | Tanque requiere agua, manual activo y cisterna con agua. |

*Análisis de Seguridad:* La combinación $m_3$ ($A=0, B=1, C=1$) permite el encendido manual si hay agua en la cisterna aunque el tanque esté lleno. El diseñador debe evaluar estas condiciones operativas para prevenir desbordamientos.

**Formulación de la Suma de Minitérminos ($m_3 + m_6 + m_7$):**
$$F = (A' \cdot B \cdot C) + (A \cdot B \cdot C') + (A \cdot B \cdot C)$$

**Proceso de Simplificación Algebraica Paso a Paso:**

1. Aplicar la **Ley de Idempotencia** ($X + X = X$) duplicando el término minitérmino $(A \cdot B \cdot C)$.  
   *Razonamiento pedagógico:* Se adiciona este término idéntico para permitir la factorización independiente por parejas tanto con el término $(A' \cdot B \cdot C)$ como con $(A \cdot B \cdot C')$:
   $$F = (A' \cdot B \cdot C) + (A \cdot B \cdot C) + (A \cdot B \cdot C') + (A \cdot B \cdot C)$$

2. Factorizar los términos comunes de cada par aplicando la **Propiedad Distributiva**:
   $$F = B \cdot C \cdot (A' + A) + A \cdot B \cdot (C' + C)$$

3. Aplicar el **Teorema del Complemento** ($X + X' = 1$):
   $$F = B \cdot C \cdot (1) + A \cdot B \cdot (1)$$
   $$F = B \cdot C + A \cdot B$$

4. Factorizar la variable común $B$:
   $$F = B \cdot (A + C)$$

*Evaluación del Resultado de Ingeniería:* La expresión booleana simplificada $F = B \cdot (A + C)$ demuestra que la bomba se activará única y exclusivamente si existe agua en la cisterna ($B = 1$) AND (el tanque requiere agua ($A = 1$) OR se activa el mando manual ($C = 1$)). Este procedimiento redujo un circuito complejo de tres compuertas AND de 3 entradas más una compuerta OR de 3 entradas, a un circuito optimizado formado por una compuerta OR de 2 entradas seguida de una compuerta AND de 2 entradas.

---

#### Ejercicio 2: Sistema de Alarma para Cinturón y Puerta de Vehículo

**Planteamiento del problema:** Una alarma sonora $F$ debe activarse ($1$) si el Motor está encendido ($A = 1$) Y [el Cinturón NO está puesto ($B = 0$) OR la Puerta está abierta ($C = 1$)].

**Definición de Variables:**
* **A:** Estado del motor ($1 =$ encendido, $0 =$ apagado).
* **B:** Estado del cinturón ($1 =$ puesto, $0 =$ no puesto $\rightarrow B' = 1$).
* **C:** Estado de la puerta ($1 =$ abierta, $0 =$ cerrada).

**Tabla de Verdad del Sistema (8 Combinaciones):**

| A | B | C | F | Minitérmino | Condición del Vehículo |
| :-: | :-: | :-: | :-: | :---: | :--- |
| 0 | 0 | 0 | 0 | - | Motor apagado, cinturón no puesto, puerta cerrada. |
| 0 | 0 | 1 | 0 | - | Motor apagado, cinturón no puesto, puerta abierta. |
| 0 | 1 | 0 | 0 | - | Motor apagado, cinturón puesto, puerta cerrada. |
| 0 | 1 | 1 | 0 | - | Motor apagado, cinturón puesto, puerta abierta. |
| 1 | 0 | 0 | 1 | $m_4 = A \cdot B' \cdot C'$ | Motor encendido, cinturón no puesto, puerta cerrada. |
| 1 | 0 | 1 | 1 | $m_5 = A \cdot B' \cdot C$ | Motor encendido, cinturón no puesto, puerta abierta. |
| 1 | 1 | 0 | 0 | - | Motor encendido, cinturón puesto, puerta cerrada (Condición segura). |
| 1 | 1 | 1 | 1 | $m_7 = A \cdot B \cdot C$ | Motor encendido, cinturón puesto, puerta abierta. |

**Formulación de la Suma de Minitérminos ($m_4 + m_5 + m_7$):**
$$F = (A \cdot B' \cdot C') + (A \cdot B' \cdot C) + (A \cdot B \cdot C)$$

**Proceso de Simplificación Algebraica Paso a Paso:**

1. Factorizar el término común $(A \cdot B')$ de los dos primeros minitérminos:
   $$F = A \cdot B' \cdot (C' + C) + A \cdot B \cdot C$$

2. Aplicar el **Teorema del Complemento** ($C' + C = 1$):
   $$F = A \cdot B' \cdot (1) + A \cdot B \cdot C = A \cdot B' + A \cdot B \cdot C$$

3. Factorizar la variable $A$:
   $$F = A \cdot (B' + B \cdot C)$$

4. Aplicar la **Ley Distributiva Absortiva** ($X' + X \cdot Y = X' + Y$, donde $B' + B \cdot C = (B' + B) \cdot (B' + C) = 1 \cdot (B' + C) = B' + C$):
   $$F = A \cdot (B' + C)$$

*Conclusión de Modelado:* La función mínima obtenida $F = A \cdot (B' + C)$ satisface rigurosamente el enunciado operacional, demostrando cómo la aplicación sistemática del Álgebra Booleana traduce requerimientos físicos en estructuras de hardware óptimas.

---

## 4. IDEAS CLAVE Y CONCLUSIONES IMPORTANTES

Esta sección compendia los puntos clave, postulados teóricos y reglas operativas fundamentales que el estudiante debe dominar para la evaluación académica de la asignatura:

1. **Fundamento Físico Binario:** Los sistemas computacionales adoptan el paradigma binario por la estabilidad operativa de los transistores como conmutadores de voltaje. En la familia TTL 74LS, cualquier voltaje recibido en el rango de $0.8\text{ V}$ a $2.0\text{ V}$ cae en la Zona Indefinida, cuya causa principal en laboratorio son las entradas flotantes o desconectadas.
2. **Márgenes de Inmunidad al Ruido:** Representan la franja de tolerancia a interferencias entre componentes. En la familia TTL 74LS, el Margen de Ruido en Nivel Alto es de $0.7\text{ V}$ ($2.7\text{ V} - 2.0\text{ V}$) y el Margen de Ruido en Nivel Bajo es de $0.3\text{ V}$ ($0.8\text{ V} - 0.5\text{ V}$).
3. **Conversión de Bases:** Para convertir cadenas binarias a bases compuestas por potencias de dos, se agrupan bits desde la derecha: en bloques de $3\text{ bits}$ para Octal y en bloques de $4\text{ bits}$ para Hexadecimal (mapeando de la A a la F los valores $10$ a $15$).
4. **Operación en Complemento a Dos:** Algoritmo de representación de enteros con signo mediante la secuencia: Invertir bits (NOT) y sumar $1$. Permite ejecutar restas reutilizando el hardware de sumadores binarios estándar, descartando el acarreo sobrante a la izquierda.
5. **Detección de Desbordamiento (*Overflow*):** Falla aritmética producida cuando el resultado de una operación excede el rango dinámico de los $N\text{ bits}$ del registro, provocando un cambio erróneo en el bit de signo al sumar dos números del mismo signo.
6. **Criterio de Selección de Códigos:**
   * **BCD:** Codifica dígitos decimales de $0$ a $9$ en grupos de $4\text{ bits}$; óptimo para displays (excluye combinaciones de $1010_2$ a $1111_2$).
   * **Gray:** Varía un solo bit entre estados adyacentes; elimina errores transitorios en encoders rotativos y define la adyacencia geométrica en Mapas de Karnaugh.
   * **ASCII:** Estándar de $7\text{ bits}$ ($128$ caracteres) para representación de datos alfanuméricos e instrucciones de control.
7. **Propiedades de Compuertas:** NAND y NOR son compuertas universales capaces de sintetizar cualquier red booleana. XOR opera como comparador de desigualdad (salida $1$ ante entradas distintas), clave para sumadores y sistemas de conmutación.
8. **Pinado Estándar TTL:** En circuitos integrados con encapsulado DIP-14 (como el 74LS08), los pines se numeran en sentido antihorario desde la muesca central. Las conexiones obligatorias de alimentación son Pin 14 a $V_{CC}$ ($+5\text{ V}$) y Pin 7 a Tierra (GND).
9. **Metodología de Simplificación:** La aplicación sistemática de teoremas booleanos simplifica ecuaciones complejas (por ejemplo, reduciendo la lógica de bombeo a $F = B \cdot (A + C)$ o la alarma vehicular a $F = A \cdot (B' + C)$), minimizando la cantidad de compuertas físicas, optimizando costos y maximizando la eficiencia energética del sistema.
