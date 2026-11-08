### Apuntes Teórico-Prácticos: Semana 4 \- Diseño de Circuitos Combinacionales y Bloques Aritméticos

##### 1\. Introducción al Diseño Combinacional y Contexto de la Semana 4

En el marco de la Unidad 1 ("Análisis de Lógica Combinacional con Diseño Clásico") de la asignatura  **Diseño de Circuitos**  de la Carrera de Computación (UNL), la Semana 4 constituye el nexo fundamental entre la minimización algebraica abstracta y la síntesis estructural de hardware funcional. Tras haber formalizado los teoremas del álgebra de Boole, la generación de formas canónicas y la optimización visual mediante Mapas de Karnaugh, el programa académico evoluciona hacia la construcción de bloques funcionales de cómputo. Los objetivos estratégicos de esta etapa comprenden:

1. El dominio riguroso de la metodología estándar del diseño combinacional clásico.  
2. La síntesis e implementación de sumadores binarios fundamentales (Medio Sumador /  *Half Adder*  y Sumador Completo /  *Full Adder* ).  
3. El diseño de circuitos restadores y el aprovechamiento de la representación en complemento a dos.  
4. La implementación de comparadores de magnitud binaria.

Esta transición desde compuertas lógicas elementales (AND, OR, NOT, XOR, XNOR) hacia subsistemas aritméticos representa el puente de ingeniería crítico hacia las Unidades Aritmético Lógicas (ALU) en arquitecturas de procesadores digitales modernos. Sin la capacidad de estructurar bloques que ejecuten adición, sustracción y comparación a nivel de bit, resulta imposible materializar la ejecución de instrucciones de cómputo sobre sustratos semiconductores físicos. La correcta abstracción de estos bloques aritméticos requiere, de manera imperativa, la adopción de una metodología de síntesis sistemática.

##### 2\. Metodología Estándar del Procedimiento de Diseño Combinacional Clásico

El desarrollo de hardware digital en el nivel de integración combinacional exige abandonar de manera absoluta el ensamblaje intuitivo o empírico de redes lógicas. La aplicación de una metodología formal de ingeniería garantiza que la arquitectura sintetizada responda de forma estricta a la especificación funcional, optimizando el área física de silicio y reduciendo el retardo de propagación de las señales.  
El procedimiento de diseño de circuitos combinacionales clásicos se estructura rigurosamente en  **cinco pasos secuenciales** :

1. **Especificación del problema:**  Formulación verbal clara y sin ambigüedades de los requerimientos operacionales y funcionales del sistema digital a diseñar.  
2. **Asignación de variables:**  Identificación cuantitativa y cualitativa de las  \$n\$  variables de entrada y las  \$m\$  variables de salida, asignando simbología alfanumérica formal a cada línea de señal.  
3. **Tabla de Verdad:**  Construcción exhaustiva del mapeo de comportamiento de las  \$m\$  salidas para las  \$2^n\$  combinaciones binarias posibles de la entrada.  
4. **Obtención y Simplificación de Funciones:**  Derivación de las funciones booleanas canónicas en Suma de Productos (SOP /  *Minterms* ) o Producto de Sumas (POS /  *Maxterms* ), seguida de su minimización formal mediante teoremas algebraicos o Mapas de Karnaugh.  
5. **Esquema del Circuito:**  Trazado del esquema estructural optimizado utilizando la notación gráfica estandarizada de compuertas lógicas bajo la norma ANSI/IEEE.

\+-------------------------------------------------------+  
\|  1\. Especificación del Problema                       \|  
\|     (Formulación clara de requerimientos funcionales)   \|  
\+---------------------------+---------------------------+  
                            \|  
                            v  
\+---------------------------+---------------------------+  
\|  2\. Asignación de Variables                           \|  
\|     (Definición de n entradas y m salidas binarias)   \|  
\+---------------------------+---------------------------+  
                            \|  
                            v  
\+---------------------------+---------------------------+  
\|  3\. Tabla de Verdad                                   \|  
\|     (Mapeo exhaustivo de las 2^n combinaciones)       \|  
\+---------------------------+---------------------------+  
                            \|  
                            v  
\+---------------------------+---------------------------+  
\|  4\. Obtención y Simplificación de Funciones           \|  
\|     (SOP/POS canónicos \-\> Minimización Karnaugh/Boole) \|  
\+---------------------------+---------------------------+  
                            \|  
                            v  
\+---------------------------+---------------------------+  
\|  5\. Esquema del Circuito                              \|  
\|     (Diagrama lógico con símbolos normados ANSI/IEEE) \|  
\+-------------------------------------------------------+

La observancia estricta de esta metodología de cinco etapas maximiza el rendimiento físico de la red lógica. Al minimizar el número de literales y compuertas, se reduce directamente el número de etapas de transistores en cascada, lo que disminuye el tiempo de retardo de propagación ( \$t\_{pd}\$ ) de la señal, minimiza la disipación de potencia dinámica y previene riesgos lógicos ( *hazards* ) en las salidas. Esta estrategia metodológica encuentra su aplicación directa inmediata en la concepción de los circuitos de adición binaria.

##### 3\. Bloques Lógicos Aritméticos Fundamentales: Sumadores (Half Adder y Full Adder) y Detección de Desbordamiento

La suma binaria constituye la operación matemática nuclear sobre la cual se fundamenta la totalidad del procesamiento numérico en computadoras. Operaciones más complejas, como la sustracción, la multiplicación, la división y el cálculo de direcciones de memoria, se derivan del bloque sumador.

###### *El Medio Sumador ( Half Adder )*

El  **Medio Sumador**  es un circuito combinacional que realiza la adición de dos bits de entrada ( \$A\$  y  \$B\$ ), generando una suma parcial ( \$S\$ ) y un acarreo de salida ( \$C\_{out}\$ ). Este bloque no posee la capacidad de procesar un acarreo proveniente de una etapa previa.

* **Tabla de Verdad del Half Adder:**

\$A\$,\$B\$,\$S\$  (Suma),\$C\_{out}\$  (Acarreo)  
0,0,0,0  
0,1,1,0  
1,0,1,0  
1,1,0,1

* **Ecuaciones Booleanas Simplificadas:**   \$\$S \= A \\bar{B} \+ \\bar{A} B \= A \\oplus B\$\$   \$\$C\_{out} \= A \\cdot B\$\$

La salida de suma ( \$S\$ ) corresponde al operador exclusivo  **XOR** , en tanto que el acarreo ( \$C\_{out}\$ ) se obtiene mediante el operador  **AND** .  
            Medio Sumador (Half Adder)  
             \+------------------+  
    A \------\>\|                  \|-------\> S \= A ⊕ B  
             \|    Half Adder    \|  
    B \------\>\|                  \|-------\> Cout \= A · B  
             \+------------------+

###### *El Sumador Completo ( Full Adder )*

El  **Sumador Completo**  resuelve la limitación estructural del Half Adder al integrar una tercera entrada binaria: el acarreo de entrada ( \$C\_{in}\$ ). Esta adición permite interconectar múltiples módulos en cascada para procesar operandos de arbitraria longitud en bits.

* **Tabla de Verdad del Full Adder:**

\$A\$,\$B\$,\$C\_{in}\$,\$S\$  (Suma),\$C\_{out}\$  (Acarreo)  
0,0,0,0,0  
0,0,1,1,0  
0,1,0,1,0  
0,1,1,0,1  
1,0,0,1,0  
1,0,1,0,1  
1,1,0,0,1  
1,1,1,1,1

* **Ecuaciones Booleanas Deductivas:**   \$\$S \= A \\oplus B \\oplus C\_{in}\$\$   \$\$C\_{out} \= A \\cdot B \+ C\_{in} (A \\oplus B) \= A \\cdot B \+ A \\cdot C\_{in} \+ B \\cdot C\_{in}\$\$

           Sumador Completo (Full Adder)  
             \+------------------+  
    A \------\>\|                  \|  
    B \------\>\|    Full Adder    \|-------\> S \= A ⊕ B ⊕ Cin  
  Cin \------\>\|                  \|-------\> Cout \= AB \+ Cin(A ⊕ B)  
             \+------------------+

###### *Detección de Desbordamiento por Hardware ( Overflow )*

En la aritmética binaria con signo bajo representación en  **Complemento a Dos** , un sistema de  \$n\$  bits permite representar valores numéricos en el rango estricto:  
\$\$\\text{Rango} \= \-2^{n-1}, \+2^{n-1} \- 1\$\$  
Para un sistema de 4 bits, el rango operativo abarca desde  \$-8\$  ( \$1000\_2\$ ) hasta  \$+7\$  ( \$0111\_2\$ ). El  **desbordamiento**  ( *overflow* ) es la condición anómala que ocurre cuando el resultado numérico de una operación excede la capacidad de almacenamiento física del registro de destino.  
Un caso crítico ocurre al realizar la adición de dos números de igual signo:  \$\$\\text{Ejemplo con 4 bits: } (+7) \+ (+1) \\longrightarrow 0111\_2 \+ 0001\_2 \= 1000\_2 \\quad (\\text{Resultado en Complemento a 2: } \-8 \\neq \+8)\$\$  
El hardware detecta esta falla aritmética mediante la evaluación de los acarreos en el bit más significativo (MSB). Específicamente, existe desbordamiento ( \$V \= 1\$ ) si y solo si el acarreo entrante al MSB ( \$C\_{in\\\_msb}\$ ) difiere del acarreo saliente de dicho bit ( \$C\_{out\\\_msb}\$ ):  
\$\$V \= C\_{in\\\_msb} \\oplus C\_{out\\\_msb}\$\$  
Equivalentemente, en términos de los bits de signo de los operandos ( \$A\_{msb}, B\_{msb}\$ ) y del resultado ( \$S\_{msb}\$ ), la condición se formula como:  
\$\$V \= A\_{msb} B\_{msb} \\bar{S}*{msb} \+ \\bar{A}*{msb} \\bar{B}*{msb} S*{msb}\$\$  
Bloque Lógico,Entradas,Salidas,Incorpora  \$C\_{in}\$ ?,Dominio de Aplicación  
Half Adder,"2 ( \$A, B\$ )","2 ( \$S, C\_{out}\$ )",No,Adición del Bit Menos Significativo (LSB).  
Full Adder,"3 ( \$A, B, C\_{in}\$ )","2 ( \$S, C\_{out}\$ )",Sí,Adición de bits intermedios y MSB en procesadores multibit.  
El análisis de la arquitectura demuestra que, mientras el Half Adder está confinado al procesamiento del LSB, el Full Adder es el componente indispensable para la construcción de sumadores paralelos de propagación de acarreo ( *Ripple Carry Adders* ) de 8, 16, 32 o 64 bits. Esta misma infraestructura física se reconfigura para realizar operaciones de sustracción.

##### 4\. Bloques Aritméticos Avanzados: Restadores y Comparadores de Magnitud

Las Unidades Aritmético Lógicas (ALU) basan su capacidad de procesamiento y control en el cálculo de diferencias y en la evaluación de condiciones relativas entre operandos binarios.  
                         UNIDAD ARITMÉTICO LÓGICA (ALU)  
\+---------------------------------------------------------------------------------+  
\|                                                                                 \|  
\|   \+------------------------------------+    \+-------------------------------+   \|  
\|   \|         BLOQUES RESTADORES         \|    \|    COMPARADORES DE MAGNITUD   \|   \|  
\|   \| Diferencia (D) y Arrastre (Bout)   \|    \|    Banderas: A\>B, A\<B, A=B    \|   \|  
\|   \+------------------------------------+    \+-------------------------------+   \|  
\|                                                                                 \|  
\|            Reutilización de Hardware: Sumador \+ Inversión Controlada            \|  
\+---------------------------------------------------------------------------------+

###### *Restadores Binarios Lógicos*

La sustracción directa de dos bits  \$A\$  (minuendo) y  \$B\$  (sustraendo) define las funciones de  **Diferencia**  ( \$D\$ ) y  **Arrastre de salida**  ( *Borrow out* ,  \$B\_{out}\$ ).

* **Medio Restador (**  ***Half Subtractor***  **):**   \$\$D \= A \\oplus B\$\$   \$\$B\_{out} \= \\bar{A} \\cdot B\$\$  
* **Restador Completo (**  ***Full Subtractor***  **):**  Incorpora el arrastre de entrada ( \$B\_{in}\$ ):  \$\$D \= A \\oplus B \\oplus B\_{in}\$\$   \$\$B\_{out} \= \\bar{A} B \+ \\bar{A} B\_{in} \+ B B\_{in}\$\$

###### *Sumador/Restador Integrado en Complemento a Dos*

En lugar de construir hardware restador independiente (lo cual duplicaría el área de silicio y la complejidad del chip), los procesadores computacionales aplican el postulado del complemento a dos:  
\$\$A \- B \= A \+ (-B) \= A \+ \\bar{B} \+ 1\$\$  
Esta equivalencia permite reconfigurar un sumador multibit mediante compuertas  **XOR**  que actúan como inversores controlados en las entradas del sustraendo, configurando la línea de control  \$\\text{Modo}\$  ( \$\\text{Modo} \= 0 \\rightarrow \\text{Suma}\$ ,  \$\\text{Modo} \= 1 \\rightarrow \\text{Resta}\$ ):  
                   Arquitectura Sumador / Restador Reconfigurable  
                                  \+-------------------+  
                       A\_i \------\>\|                   \|  
                                  \|    Full Adder     \|-------\> D\_i / S\_i  
     B\_i \-------\>+-------\\        \|                   \|  
                 \|  XOR   )------\>\|                   \|  
     Modo \----+-\>\|\_\_\_\_\_\_\_/        \+-------------------+  
              \|                             ^  
              \|                             \|  
              \+-----------------------------+ (Cin inicial \= Modo)

Cuando  \$\\text{Modo} \= 1\$ , la compuerta XOR efectúa la inversión bit a bit ( \$\\bar{B}*i\$ ) e inyecta un  \$1\$  en el acarreo de entrada del LSB ( \$C*{in\\\_0} \= 1\$ ), ejecutando la adición en complemento a dos en una única pasada de propagación. Esta reutilización arquitectónica reduce drásticamente el conteo de compuertas, disminuye el consumo de energía dinámica y simplifica el enrutamiento físico en la ALU.

###### *Comparadores de Magnitud Binaria*

Un  **Comparador de Magnitud**  es un bloque combinacional que evalúa dos palabras binarias de  \$n\$  bits ( \$A\$  y  \$B\$ ) y genera tres banderas ( *flags* ) lógicas de salida mutuamente excluyentes:  \$A \> B\$ ,  \$A \< B\$  y  \$A \= B\$ .  
La condición de  **igualdad bit a bit**  entre los bits  \$A\_i\$  y  \$B\_i\$  se procesa mediante compuertas  **XNOR**  (notación ANSI):  
\$\$x\_i \= (A\_i \\oplus B\_i)' \= A\_i \\odot B\_i\$\$  
Para dos palabras binarias de  \$n\$  bits, la salida de  **igualdad global**  ( \$A \= B\$ ) exige que todos los pares de bits sean idénticos, formulándose formalmente mediante la conjunción lógica de todas las compuertas XNOR:  
\$\$(A \= B) \= \\prod\_{i=0}^{n-1} x\_i \= x\_{n-1} \\cdot x\_{n-2} \\cdots x\_1 \\cdot x\_0\$\$  
Las condiciones de  **desigualdad de magnitud**  se derivan inspeccionando los bits desde el más significativo (MSB) hacia el menos significativo (LSB):  
\$\$(A \> B) \= A\_{n-1} \\bar{B}*{n-1} \+ x*{n-1} A\_{n-2} \\bar{B}*{n-2} \+ x*{n-1} x\_{n-2} A\_{n-3} \\bar{B}*{n-3} \+ \\dots \+ \\left( \\prod*{i=1}^{n-1} x\_i \\right) A\_0 \\bar{B}\_0\$\$  
\$\$(A \< B) \= \\bar{A}*{n-1} B*{n-1} \+ x\_{n-1} \\bar{A}*{n-2} B*{n-2} \+ x\_{n-1} x\_{n-2} \\bar{A}*{n-3} B*{n-3} \+ \\dots \+ \\left( \\prod\_{i=1}^{n-1} x\_i \\right) \\bar{A}\_0 B\_0\$\$  
La derivación matemática de estos bloques combinacionales culmina con su validación física e instrumental dentro del laboratorio.

##### 5\. Parámetros Operativos de la Práctica de Laboratorio (APE 04\)

La contrastación experimental de las ecuaciones lógicas y la simulación del comportamiento dinámico del hardware constituyen fases indispensables en el diseño de circuitos digitales antes de la síntesis sobre silicio.  
\===============================================================================  
                    PARÁMETROS OPERATIVOS DE LA PRÁCTICA  
\===============================================================================  
  • Sesión Práctica:     APE 04 (Análisis y Práctica de Entorno / Laboratorio)  
  • Horario Programado:  Martes 27 de octubre · 07:30 – 09:30  
  • Entorno de Trabajo:  Simulador Logisim-evolution y bancada física TTL  
\===============================================================================

La validación práctica en entornos de simulación como Logisim-evolution y sobre protoboards mediante circuitos integrados de la familia TTL (tales como el 74LS08 para compuertas AND, 74LS86 para XOR o sumadores integrados) permite medir cuantitativamente los tiempos de retardo de propagación ( \$t\_{pd}\$ ), verificar experimentalmente las tablas de verdad planteadas y garantizar la ausencia de fallas lógicas previamente a la implementación en sistemas incrustados o microprocesadores.  
