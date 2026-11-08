### Apuntes Académicos de Diseño de Circuitos: Semana 1 — Sistemas Numéricos, Códigos y Compuertas Lógicas

#### Introducción Ejecutiva

El procesamiento digital de información en la arquitectura moderna de computadores se fundamenta en la abstracción de variables físicas continuas a estados discretos binarios. En el nivel más bajo del hardware, la información no existe como conceptos abstractos, sino como niveles de voltaje manipulados por millones de transistores operando como interruptores controlados por tensión. Comprender la transición teórica y práctica desde los rangos de voltaje operacional hasta la estructuración matemática de datos es un requisito indispensable para el diseño de sistemas combinacionales. Esta unidad temática establece las bases operativas de la electrónica digital, abarcando desde los márgenes de ruido físico hasta los sistemas numéricos posicionales, códigos de representación de información, aritmética en complemento a dos, funciones lógicas fundamentales e integración física en circuitos integrados comerciales de la familia TTL.

#### 1\. Fundamentos de Representación Binaria y Margen de Ruido

La necesidad de emplear la lógica binaria ( \$0\$  y  \$1\$ ) en lugar de sistemas de múltiples niveles analógicos radica en la robustez e inmunidad frente a las perturbaciones del entorno. Las señales analógicas continuas sufren degradación sistemática por atenuación y ruido electromagnético. Al abstraer el comportamiento físico mediante transistores que actúan estrictamente como interruptores abiertos (corte) o cerrados (saturación), el sistema solo requiere distinguir entre dos estados de voltaje bien diferenciados: nivel BAJO ( \$0\$ ) y nivel ALTO ( \$1\$ ).

##### Rangos de Voltaje y Zona Indefinida en la Familia 74LS

En los circuitos integrados digitales, un estado lógico no corresponde a un valor numérico de voltaje único o exacto, sino a rangos de tolerancia perfectamente delimitados por el fabricante. Para la familia lógica TTL de bajo consumo Schottky (74LS), la hoja de especificaciones establece los siguientes límites operativos:  
Parámetro / Estado Lógico,Rango de Voltaje Especificado  
Salida en ALTO (  \$V\_{OH}\$  ),Promete  \$\\ge 2.7\\text{ V}\$  (hasta  \$+5.0\\text{ V}\$ )  
Entrada en ALTO (  \$V\_{IH}\$  ),Exige  \$\\ge 2.0\\text{ V}\$  
Entrada en BAJO (  \$V\_{IL}\$  ),Exige  \$\\le 0.8\\text{ V}\$  
Salida en BAJO (  \$V\_{OL}\$  ),Promete  \$\\le 0.5\\text{ V}\$  (desde  \$0.0\\text{ V}\$ )  
El intervalo comprendido entre  \$0.8\\text{ V}\$  y  \$2.0\\text{ V}\$  constituye la denominada  **Zona Indefinida** . Dentro de esta franja, el comportamiento del circuito integrado es totalmente impredecible: el chip no garantiza si interpretará la señal como un  \$0\$  o como un  \$1\$ , pudiendo oscilar, generar estados erráticos o incrementar el consumo de corriente. La causa técnica más común para que un nodo caiga en la zona indefinida es dejar una terminal de entrada flotante (sin conectar) o provocar un acoplamiento resistivo/capacitivo deficiente entre etapas lógicas.

##### Análisis del Margen de Ruido ("Colchón de Seguridad")

El margen de ruido es la tolerancia máxima a las interferencias eléctricas que un circuito digital puede resistir antes de que una señal válida cambie su interpretación lógica al transmitirse entre etapas. Se calcula de forma independiente para ambos niveles de voltaje:

* **Margen de Ruido de Nivel ALTO (**  **\$\\text{NM}\_H**\$  **):**   \$\$\\text{NM}*H \= V*{OH} \- V\_{IH} \= 2.7\\text{ V} \- 2.0\\text{ V} \= 0.7\\text{ V}\$\$  
* **Margen de Ruido de Nivel BAJO (**  **\$\\text{NM}\_L**\$  **):**   \$\$\\text{NM}*L \= V*{IL} \- V\_{OL} \= 0.8\\text{ V} \- 0.5\\text{ V} \= 0.3\\text{ V}\$\$

Un nivel lógico en ALTO garantizado a  \$2.7\\text{ V}\$  puede sufrir caídas o interferencias de hasta  \$0.7\\text{ V}\$  en el trayecto y continuará siendo interpretado correctamente por la compuerta receptora como un  \$1\$  (al mantenerse  \$\\ge 2.0\\text{ V}\$ ). De manera análoga, un  \$0\$  lógico garantizado a  \$0.5\\text{ V}\$  dispone de un colchón de seguridad de  \$0.3\\text{ V}\$ .  
**Escenario práctico de falla por ruido inducido:**  Supóngase una compuerta que emite un estado BAJO nominal de  \$0.5\\text{ V}\$ . Si la línea de interconexión transcurre cerca de un motor eléctrico que induce un pico de ruido de  \$0.4\\text{ V}\$ , el voltaje efectivo recibido en la entrada de la siguiente compuerta será:  \$\$0.5\\text{ V} \+ 0.4\\text{ V} \= 0.9\\text{ V}\$\$  Dado que  \$0.9\\text{ V}\$  supera el límite  \$V\_{IL} \= 0.8\\text{ V}\$  e ingresa en la zona indefinida, el receptor no garantiza la lectura de un estado BAJO, provocando una falla catastrófica de interpretación lógica en el sistema.  
Una vez formalizada la necesidad física de abstraer los voltajes en estados binarios discretos, resulta indispensable estudiar la estructura matemática que permite organizar dichos bits en sistemas numéricos posicionales.

#### 2\. Sistemas Numéricos Posicionales y Métodos de Conversión

La representación de datos en la ciencia de la computación se apoya en los sistemas numéricos posicionales. A diferencia de los sistemas no posicionales (como la numeración romana, donde el símbolo 'V' representa el valor 5 independientemente de su ubicación en la cifra), en un sistema posicional el valor de cada dígito depende directamente de su posición relativa multiplicada por la potencia respectiva de la base del sistema.

##### Estructura del Sistema Binario y Pesos Posicionales

El sistema binario utiliza como base el número  \$2\$ , requiriendo únicamente dos símbolos:  \$\\{0, 1\\}\$ .

* **Bit (**  ***Binary Digit***  **):**  Unidad mínima de información digital.  
* **Byte:**  Conjunto contiguo de 8 bits.

El valor numérico de una secuencia binaria se obtiene mediante la suma de los productos de cada bit por su peso posicional  \$2^n\$ , ordenados de derecha a izquierda comenzando desde  \$2^0\$ :  
Potencia,\$2^7\$,\$2^6\$,\$2^5\$,\$2^4\$,\$2^3\$,\$2^2\$,\$2^1\$,\$2^0\$  
Valor Decimal del Peso,128,64,32,16,8,4,2,1  
**Evaluación del valor binario**  **\$1011\_2**\$  **:**   \$\$1011\_2 \= (1 \\times 2^3) \+ (0 \\times 2^2) \+ (1 \\times 2^1) \+ (1 \\times 2^0) \= 8 \+ 0 \+ 2 \+ 1 \= 11\_{10}\$\$

##### Metodología de Conversión entre Sistemas Numéricos

###### *Decimal a Binario (Divisiones Sucesivas)*

Para convertir un número entero decimal a su equivalente binario, se aplica el método de divisiones sucesivas entre 2 hasta obtener un cociente de 0\. La secuencia de residuos resultante se lee estrictamente en orden inverso (de abajo hacia arriba).  
**Ejemplo de conversión de**  **\$25\_{10}**\$  **a binario:**  
Operación de División,Cociente,Residuo  
\$25 \\div 2\$,12,1  (LSB \- Bit menos significativo)  
\$12 \\div 2\$,6,0  
\$6 \\div 2\$,3,0  
\$3 \\div 2\$,1,1  
\$1 \\div 2\$,0,1  (MSB \- Bit más significativo)  
Ordenando los residuos de abajo hacia arriba:  \$25\_{10} \= 11001\_2\$ .  *Comprobación por pesos:*   \$16 \+ 8 \+ 1 \= 25\$ .

###### *Binario a Hexadecimal y Octal*

Los sistemas Hexadecimal (base 16\) y Octal (base 8\) se emplean como abreviaturas compactas del sistema binario. Dado que  \$16 \= 2^4\$  y  \$8 \= 2^3\$ , un dígito hexadecimal resume exactamente 4 bits (nibble), mientras que un dígito octal resume 3 bits.  
En el sistema Hexadecimal, los valores del 10 al 15 se representan mediante letras mayúsculas:  
Decimal,10,11,12,13,14,15  
Hexadecimal,A,B,C,D,E,F  
**Reglas de conversión:**

1. **Agrupamiento:**  Se agrupa la cadena binaria desde la derecha hacia la izquierda en bloques de 4 bits para hexadecimal o de 3 bits para octal.  
2. **Completado de ceros:**  Si el grupo del extremo izquierdo no completa la cantidad de bits requerida, se agregan ceros a la izquierda.  
3. **Sustitución:**  Se reemplaza cada grupo por su símbolo equivalente en la base destino.

**Conversión del valor**  **\$157\_{10} \= 10011101\_2**\$  **:**

* **A Hexadecimal (grupos de 4 bits):**   \$\$1001 \\quad 1101\_2 \\longrightarrow 9 \\quad \\text{D}*{16} \\implies 9\\text{D}*{16}\$\$   *(Donde*  *\$1001\_2 \= 9*\$  *y*  *\$1101\_2 \= 13\_{10} \= \\text{D}*\$  *)*  
* **A Octal (grupos de 3 bits):**   \$\$010 \\quad 011 \\quad 101\_2 \\longrightarrow 2 \\quad 3 \\quad 5\_8 \\implies 235\_8\$\$   *(Nótese la adición de un cero a la izquierda para completar el grupo de 3 bits:*  *\$010\_2 \= 2*\$  *,*  *\$011\_2 \= 3*\$  *,*  *\$101\_2 \= 5*\$  *)*

La estructuración de números positivos establece las bases de la cuantificación binaria, pero para procesar operaciones aritméticas completas es indispensable abordar la representación matemática de valores negativos en el hardware.

#### 3\. Aritmética Binaria y Representación con Signo (Complemento a Dos)

En los circuitos digitales físicos no existe un símbolo " \$-\$ " (menos) ni un componente electrónico para almacenarlo. Todo dato debe codificarse utilizando exclusivamente los estados discretos  \$0\$  y  \$1\$ . Por este motivo, se adopta el estándar de  **Complemento a Dos** , un convenio matemático que permite representar enteros con signo dentro de un ancho de palabra fijo y realizar operaciones de resta utilizando la misma estructura del sumador.

##### Reglas de la Suma Binaria

La adición binaria sigue los principios de la aritmética posicional, generando unidades de acarreo ( *carry* ) hacia la columna superior inmediata cuando el resultado excede la base.  
Combinación,Resultado,Acarreo ( Carry )  
\$0 \+ 0\$,\$0\$,0  
\$0 \+ 1\$,\$1\$,0  
\$1 \+ 0\$,\$1\$,0  
\$1 \+ 1\$,\$0\$,"1 (resultado ""10"")"  
\$1 \+ 1 \+ 1\$,\$1\$,"1 (resultado ""11"")"  
**Demostración de adición con acarreo (**  **\$11\_{10} \+ 6\_{10} \= 17\_{10}**\$  **):**  \\begin{array}{rccccc} & 1 & 1 & 1 & & \\text{(Acarreos)} \\ & 1 & 0 & 1 & 1\_2 & (11) \\

* & 0 & 1 & 1 & 0\_2 & (6) \\ \\hline \\mathbf{1} & \\mathbf{0} & \\mathbf{0} & \\mathbf{0} & \\mathbf{1}\_2 & (\\mathbf{17}) \\end{array}

##### Mecánica del Complemento a Dos

Para calcular el equivalente negativo de un número en Complemento a Dos, se aplica un procedimiento de dos pasos fundamentales:

1. **Inversión lógica (Complemento a Uno):**  Cambiar todos los ceros por unos y los unos por ceros.  
2. **Suma de la unidad:**  Adicionar  \$1\$  al valor invertido.

**Ejemplo para 4 bits (Obtener**  **\$-3\_{10}**\$  **a partir de**  **\$+3\_{10}**\$  **):**

* Representación de  \$+3\_{10}\$  en 4 bits: 0011  
* Complemento a uno (invertir bits): 1100  
* Sumar 1: 1100 \+ 0001 \= 1101 (Representación binaria de  \$-3\_{10}\$ )

###### *Rangos de Representación*

El bit ubicado en el extremo izquierdo (Bit Más Significativo o MSB) actúa como indicador de signo: un  **\$0**\$  indica un número positivo y un  **\$1**\$  indica un número negativo. Para una longitud de palabra de  \$n\$  bits, el rango dinámico representable se define como  \$-2^{n-1}, \+2^{n-1} \- 1\$ :

* **Para 4 bits (**  **\$n=4**\$  **):**  Rango de  \$-8\$  a  \$+7\$ .  
* **Para 8 bits (**  **\$n=8**\$  **):**  Rango de  \$-128\$  a  \$+127\$ .

Código (4 bits),Valor Decimal,Código (4 bits),Valor Decimal  
0000,0,1000,\$-8\$  
0001,\$+1\$,1001,\$-7\$  
0010,\$+2\$,1010,\$-6\$  
0011,\$+3\$,1011,\$-5\$  
0100,\$+4\$,1100,\$-4\$  
0101,\$+5\$,1101,\$-3\$  
0110,\$+6\$,1110,\$-2\$  
0111,\$+7\$,1111,\$-1\$  
**Ventaja de Hardware:**  La principal ventaja de este sistema es que elimina la necesidad de implementar un circuito restador independiente. La resta  \$A \- B\$  se computa mediante la suma  \$A \+ (-B)\$ . Cualquier bit sobrante de acarreo que supere el ancho de palabra a la izquierda se descarta automáticamente.  
*Demostración de resta (*  *\$5 \- 3 \= 2*\$  *en 4 bits):*  \\begin{array}{rcl} 0101\_2 & (+5) \\

* \\quad 1101\_2 & (-3) \\ \\hline \\mathbf{1} , 0010\_2 & \\longrightarrow \\text{El acarreo del MSB se descarta } \\implies \\mathbf{0010\_2} \= \+2\_{10} \\quad \\checkmark \\end{array}

##### Evaluación del Desbordamiento ( *Overflow* )

El fenómeno de desbordamiento ( *overflow* ) ocurre cuando el resultado de una operación aritmética supera los límites lógicos del rango dinámico permitido por el ancho de bits. En estas circunstancias, el bit de signo se corrompe y produce una interpretación numéricamente errónea.  
**Ejemplo de desbordamiento en 4 bits (**  **\$7 \+ 1**\$  **):**  \\begin{array}{rcl} 0111\_2 & (+7) \\

* \\quad 0001\_2 & (+1) \\ \\hline \\mathbf{1000\_2} & \\longrightarrow \\text{Representa } \\mathbf{-8} \\text{ en Complemento a Dos} \\quad \\boldsymbol{\\times} \\end{array}

La adición de dos valores positivos generó un resultado negativo debido a que  \$+8\$  no cabe en el rango dinámico de 4 bits (cuyo máximo es  \$+7\$ ).  
**Conexión con la Arquitectura de Software:**  
Este comportamiento a nivel de hardware se traslada directamente a los lenguajes de programación de alto nivel (como Java o C) que utilizan enteros con signo en complemento a dos de 32 bits (int, con rango de  \$-2147483648\$  a  \$+2147483647\$ ). Al sobrepasar el límite máximo positivo, la variable sufre un desbordamiento idéntico al de 4 bits:  
int x \= 2147483647;  
x \= x \+ 1; // x pasa a valer \-2147483648 debido al desbordamiento en hardware

La representación numérico-aritmética se complementa con códigos de propósito específico diseñados para optimizar interfaces físicas y la transmisión de datos.

#### 4\. Códigos de Representación de Información: BCD, Gray y ASCII

Además de los sistemas posicionales puros, la ingeniería de computadores emplea codificaciones binarias especializadas para resolver requerimientos de la interfaz hombre-máquina, lectura de sensores mecánicos y procesamiento de texto.

##### Análisis Comparativo de Códigos Especializados

###### *1\. Código BCD (Decimal Codificado en Binario / BCD 8421\)*

El código BCD representa cada dígito de un número decimal ( \$0\$  al  \$9\$ ) de forma independiente mediante un grupo de 4 bits binarios.

* **Propósito:**  Interfaz directa con pantallas de cristal líquido, displays numéricos de 7 segmentos, calculadoras y contadores.  
* **Combinaciones no válidas:**  Dado que un dígito decimal no excede el valor  \$9\$ , las secuencias binarias de  \$1010\_2\$  ( \$10\_{10}\$ ) a  \$1111\_2\$  ( \$15\_{10}\$ )  **son inválidas**  en BCD.

*Comparación de la representación del número*  *\$157\_{10}*\$  *:*

* **Binario Puro:**   \$157\_{10} \= 10011101\_2\$  
* **BCD Natural:**   \$157\_{10} \= \\underbrace{0001}*{1} \\quad \\underbrace{0101}*{5} \\quad \\underbrace{0111}*{7}*{\\text{ BCD}}\$

###### *2\. Código Gray*

El código Gray es una codificación no ponderada en la que dos números consecutivos adyacentes difieren en la posición de  **un solo bit** , independientemente del tamaño de la palabra.  
**Análisis de Transición:**  En el sistema binario natural, al pasar del valor decimal  \$3\$  (011) al  \$4\$  (100),  **los tres bits cambian de estado simultáneamente** . En un sensor rotativo mecánico o encoder óptico de alta velocidad, las imperfecciones de fabricación impiden que los 3 transistores o fotodiodos conmuten al mismo instante exacto. Si un bit conmuta antes que los demás, el sensor reportará transitoriamente valores erróneos (por ejemplo, 011  \$\\rightarrow\$  001  \$\\rightarrow\$  100). En cambio, en el código Gray la misma transición del  \$3\$  al  \$4\$  modifica únicamente un bit (011  \$\\rightarrow\$  010), eliminando por completo los fallos de lectura espurios.  
Decimal,Binario Natural,Código Gray,Transición Gray  
0,000,000,-  
1,001,001,Cambia bit 0  
2,010,011,Cambia bit 1  
3,011,010,Cambia bit 0  
4,100,110,Cambia bit 2

* **Generación por Reflejo (Espejo):**  Se construye duplicando en orden inverso la secuencia de 1 bit, anteponiendo ceros a la mitad superior y unos a la inferior. Esta adyacencia de 1 bit es la base metodológica para la ordenación geométrica de las variables en los Mapas de Karnaugh.

###### *3\. Código ASCII ( American Standard Code for Information Interchange )*

El código ASCII representa caracteres alfanuméricos y comandos de control digital mediante una estructura estandarizada de  **7 bits** , ofreciendo  \$2^7 \= 128\$  combinaciones.

* **Almacenamiento en hardware:**  Cuando un sistema capta la tecla  **'A'** , la memoria almacena el entero  \$65\_{10}\$ , equivalente a  \$41\_{16}\$  o en binario al byte  \$01000001\_2\$ .  
* **Limitación Regional:**  Al estar restringido a 128 posiciones, el ASCII estándar excluye símbolos regionales como la 'ñ' o las tildes, motivando el uso de estándares extendidos como UTF-8 en software moderno.

Comprendidas las diversas representaciones de datos, corresponde analizar la estructura de los dispositivos encargados de su procesamiento lógico elemental: las compuertas lógicas.

#### 5\. Compuertas Lógicas Fundamentales y Estándares de Simbología

Las compuertas lógicas son los bloques constitutivos básicos de la electrónica combinacional. Implementan las funciones fundamentales del Álgebra de Boole, evaluando una o más entradas para producir una única salida lógica de acuerdo con reglas estrictas de operación.

##### Síntesis Teórica y Tablas de Verdad de las 7 Compuertas Lógicas

###### *Compuertas Básicas*

###### *1\. NOT (Inversor)*

Realiza el complemento lógico de la entrada.

* **Expresión Booleana:**   \$Y \= A'\$

\$A\$,\$Y\$  
0,1  
1,0

###### *2\. AND (Multiplicación Lógica)*

La salida toma el valor  \$1\$   **únicamente si todas**  sus entradas están en nivel ALTO.

* **Expresión Booleana:**   \$Y \= A \\cdot B\$

\$A\$,\$B\$,\$Y\$  
0,0,0  
0,1,0  
1,0,0  
1,1,1

###### *3\. OR (Suma Lógica)*

La salida toma el valor  \$1\$  si  **al menos una**  de sus entradas está en nivel ALTO.

* **Expresión Booleana:**   \$Y \= A \+ B\$

\$A\$,\$B\$,\$Y\$  
0,0,0  
0,1,1  
1,0,1  
1,1,1

###### *Compuertas Negadas*

###### *4\. NAND (NOT-AND)*

Produce la inversión de una compuerta AND. La salida es  \$0\$   **únicamente si todas**  las entradas están en nivel ALTO.

* **Expresión Booleana:**   \$Y \= (A \\cdot B)'\$

\$A\$,\$B\$,\$Y\$  
0,0,1  
0,1,1  
1,0,1  
1,1,0

###### *5\. NOR (NOT-OR)*

Produce la inversión de una compuerta OR. La salida es  \$1\$   **únicamente si ninguna**  de sus entradas está activa ( \$0\$ ).

* **Expresión Booleana:**   \$Y \= (A \+ B)'\$

\$A\$,\$B\$,\$Y\$  
0,0,1  
0,1,0  
1,0,0  
1,1,0

###### *Compuertas de Comparación*

###### *6\. XOR (OR Exclusiva)*

La salida es  \$1\$   **si las entradas son distintas**  entre sí.

* **Expresión Booleana:**   \$Y \= A \\oplus B\$  
* **Aplicación física:**  Conmutador de luz de escalera (dos interruptores independientes donde cambiar la posición de cualquiera invierte el estado de la lámpara).

\$A\$,\$B\$,\$Y\$  
0,0,0  
0,1,1  
1,0,1  
1,1,0

###### *7\. XNOR (Equivalencia / NOR Exclusiva)*

La salida es  \$1\$   **si las entradas son iguales**  entre sí.

* **Expresión Booleana:**   \$Y \= (A \\oplus B)'\$

\$A\$,\$B\$,\$Y\$  
0,0,1  
0,1,0  
1,0,0  
1,1,1

##### Comparativa de Estándares de Simbología

Existen dos normativas internacionales diferenciadas para la representación gráfica de las compuertas lógicas en esquemas de diseño:

* **Norma ANSI/IEEE Standard 91-1984:**  Emplea  **formas distintivas**  características (entrada plana y curva posterior para AND; forma arqueada en punta para OR). Es el estándar primario utilizado en el desarrollo de la asignatura.  
* **Norma IEC 60617-12 (International Electrotechnical Commission):**  Emplea  **bloques rectangulares**  uniformes con símbolos internos descriptivos:  
* **NOT:**  Rectángulo con el símbolo 1 y un  **burbuja/círculo de negación**  en la terminal de salida.  
* **AND / NAND:**  Rectángulo con el símbolo &. La compuerta NAND mantiene el símbolo & pero añade la  **burbuja de negación**  a la salida.  
* **OR / NOR:**  Rectángulo con el símbolo ≥1. La compuerta NOR añade la  **burbuja de negación**  a la salida.  
* **XOR / XNOR:**  Rectángulo con el símbolo \=1. La compuerta XNOR añade la  **burbuja de negación**  a la salida.

Estas construcciones lógicas abstractas se materializan en circuitos integrados reales, cuya arquitectura debe identificarse mediante hojas de datos.

#### 6\. Integración Física y Lectura de Hojas de Datos (Serie 74LS08)

La implementación física de las compuertas lógicas se realiza mediante circuitos integrados (chips) de la familia 74\. La correcta integración de estos componentes exige la interpretación técnica de las hojas de datos ( *datasheets* ) comerciales y el conocimiento formal de la geometría de sus encapsulados.

##### Arquitectura del Encapsulado DIP-14

El tipo de encapsulado estándar para pruebas en laboratorio es el  **DIP-14**  ( *Dual In-line Package*  de 14 pines), compuesto por dos filas paralelas de 7 pines con un espaciado regular de 0.1 pulgadas.  
                  \+---U---+  
         1A \-- 1 \|1     14\| \-- VCC (+5V)  
         1B \-- 2 \|2     13\| \-- 4B  
         1Y \-- 3 \|3     12\| \-- 4A  
         2A \-- 4 \|4     11\| \-- 4Y  
         2B \-- 5 \|5     10\| \-- 3B  
         2Y \-- 6 \|6      9\| \-- 3A  
   (GND) 0V \-- 7 \|7      8\| \-- 3Y  
                 \+--------+  
         CI 74LS08 (Cuádruple AND)

###### *Convención para el Conteo e Identificación de Pines*

1. **Orientación:**  Se sostiene el integrado con la muesca semicircular de referencia apuntando hacia arriba.  
2. **Pin 1:**  Se localiza en la esquina superior izquierda, contiguo a la muesca.  
3. **Sentido de conteo:**  Se efectúa un recorrido en  **sentido antihorario** : los pines del 1 al 7 descienden por el costado izquierdo, el pin 8 se encuentra en el extremo inferior derecho, y los pines del 9 al 14 ascienden por el costado derecho.

###### *Asignación de Pines del 74LS08 (Cuádruple Compuerta AND de 2 Entradas)*

El chip contiene internamente cuatro compuertas AND independientes:

* **Alimentación Principal:**  
* **Pin 14 (**  **\$V\_{CC}**\$  **):**  Alimentación positiva a  \$+5\\text{ V}\$ .  
* **Pin 7 (**  **\$\\text{GND}**\$  **):**  Masa de referencia a  \$0\\text{ V}\$ .  
* **Distribución de las 4 Compuertas:**  
* Compuerta 1: Entradas en pines 1 (1A) y 2 (1B); Salida en pin 3 (1Y).  
* Compuerta 2: Entradas en pines 4 (2A) y 5 (2B); Salida en pin 6 (2Y).  
* Compuerta 3: Entradas en pines 9 (3A) y 10 (3B); Salida en pin 8 (3Y).  
* Compuerta 4: Entradas en pines 12 (4A) y 13 (4B); Salida en pin 11 (4Y).

##### Decodificación de Códigos de Fabricante (Nomenclatura)

Los códigos comerciales impresos en el encapsulado obedecen a un estándar estructural estricto. Tomando como ejemplo la serie comercial  **SN 74 LS 08 N** :

* **SN**  **(Prefijo de Fabricante):**  Identifica a la firma fabricante (SN corresponde a Texas Instruments).  
* **74**  **(Rango de Aplicación y Temperatura):**  Indica el rango comercial de operabilidad ( \$0^\\circ\\text{C}\$  a  \$70^\\circ\\text{C}\$ ). El prefijo 54 se reserva para grado militar ( \$-55^\\circ\\text{C}\$  a  \$125^\\circ\\text{C}\$ ).  
* **LS**  **(Tecnología y Familia Lógica):**  Define la tecnología de construcción interna. LS indica  *Low-power Schottky*  (tecnología bipolar TTL de bajo consumo y alta velocidad). En caso de presentarse la sigla HC ( *High-speed CMOS* ), la función y la disposición de pines se mantienen idénticas, pero la variante pertenece a la tecnología CMOS de menor consumo estático.  
* **08**  **(Identificador Funcional):**  Designa la función lógica interna (cuatro compuertas AND de 2 entradas).  
* **N**  **(Tipo de Encapsulado Físico):**  Especifica el formato del empaque. La letra N representa el encapsulado DIP plástico para inserción en placa o protoboard ( *Through-Hole* ).

##### Resumen Sintético de la Semana 1

Durante esta unidad temática se han consolidado los cimientos operativos del hardware digital:

1. La abstracción física del voltaje en estados discretos ( \$0\$  y  \$1\$ ) delimitados por rangos operacionales ( \$V\_{OH}, V\_{IH}, V\_{IL}, V\_{OL}\$ ) y márgenes de ruido ( \$0.7\\text{ V}\$  en ALTO y  \$0.3\\text{ V}\$  en BAJO en la familia 74LS).  
2. Los métodos de conversión y representación en sistemas posicionales binario, octal y hexadecimal.  
3. El cálculo de resta y la codificación de números negativos mediante  **Complemento a Dos** , así como el análisis del desbordamiento ( *overflow* ) en hardware y su manifestación en lenguajes de alto nivel.  
4. Las aplicaciones de los códigos de interfaz  **BCD**  (para displays),  **Gray**  (adyacencia de 1 bit para encoders y Mapas de Karnaugh) y  **ASCII**  (para texto).  
5. La formalización teórica, tablas de verdad y comparación de estándares gráficos (ANSI/IEEE e IEC) de las 7 compuertas lógicas.  
6. La arquitectura del encapsulado DIP-14 y el patillaje del circuito integrado  **74LS08** .

Este marco conceptual establece la base requerida para abordar en la Semana 2 el  **Álgebra de Boole, los Axiomas de Huntington y la obtención formal de Formas Canónicas**  (SOP y POS).  
