### Apuntes Sintéticos: Semana 5 – Encaminamiento, Visualización de Datos y Bloques Combinacionales Complejos

#### Introducción a la Lógica Combinacional Avanzada de la Semana 5

La Semana 5 marca la culminación de la Unidad 1 («Análisis de Lógica Combinacional con Diseño Clásico») en la asignatura Diseño de Circuitos de la Carrera de Computación (UNL). Tras haber establecido las bases teórico-prácticas del hardware digital —desde el comportamiento del transistor como interruptor controlado por voltaje dentro de las zonas definidas de la familia lógica TTL (como la serie 74LS08), el tratamiento algebraico mediante axiomas de Huntington y simplificación por Mapas de Karnaugh, hasta la arquitectura de bloques aritméticos elementales ( *Half Adder* ,  *Full Adder* , restadores y comparadores de magnitud)—, el diseño digital exige un escalamiento hacia subsistemas lógicos de mayor nivel de abstracción. La transición estratégica desde la manipulación de bits aislados hacia el empleo de bloques combinacionales complejos responde a la necesidad imperativa de gestionar el flujo masivo de información, multiplexar señales en buses compartidos y estructurar las interfaces de interacción entre la lógica de cómputo y el entorno físico.  
Esta evolución en la jerarquía del diseño lógico permite trascender la implementación de ecuaciones booleanas puras para abordar la ingeniería de la ruta de datos ( *datapath* ), donde la selección de canales y el enrutamiento controlado constituyen la espina dorsal del procesamiento en la arquitectura de computadores.

#### Multiplexores (MUX) y Demultiplexores (DEMUX): Selección y Encaminamiento de Datos

En las arquitecturas de computadores modernas, interconectar múltiples módulos de procesamiento de forma dedicada provocaría un crecimiento exponencial e inviable de líneas de transmisión físicas. Para mitigar la congestión del cableado y optimizar el uso de buses de datos compartidos, la lógica combinacional implementa dispositivos de conmutación electrónica denominados Multiplexores (MUX) y Demultiplexores (DEMUX). Estos bloques dirigen el tráfico de señales digitales administrando eficientemente el ancho de banda del canal de interconexión.  
La relación fundamental de estos dispositivos se rige por la escala binaria entre las líneas de control o selección ( \$s\$ ) y el número de canales de datos ( \$n\$ ), donde  \$n \= 2^s\$  (o  \$s \= \\log\_2 n\$ ):

* **Multiplexor (MUX):**  Bloque combinacional conmutador de  *muchos a uno* . Recibe  \$n \= 2^s\$  entradas de datos ( \$I\_0, I\_1, \\dots, I\_{n-1}\$ ) y, mediante el código binario aplicado a  \$s\$  líneas de selección, encamina el valor lógico de una única entrada seleccionada hacia una línea de salida global ( \$Y\$ ).  
* **Demultiplexor (DEMUX):**  Bloque combinacional conmutador de  *uno a muchos* . Realiza la función dual al MUX; toma una única línea de entrada de datos ( \$E\$ ) y la distribuye hacia una de las  \$n \= 2^s\$  líneas de salida posibles ( \$Y\_0, Y\_1, \\dots, Y\_{n-1}\$ ), según el valor presente en las  \$s\$  líneas de control.

Criterio de Comparación,Multiplexor (MUX),Demultiplexor (DEMUX)  
Función Principal,Selección de canales (conmutación muchas entradas a una salida),Distribución de canales (conmutación una entrada a muchas salidas)  
Entradas de Datos,\$n \= 2^s\$  líneas de entrada físicas,\$1\$  sola línea de entrada de datos  
Líneas de Selección,\$s \= \\log\_2 n\$  líneas de control binario,\$s \= \\log\_2 n\$  líneas de control binario  
Salidas,\$1\$  sola línea de salida de datos,\$n \= 2^s\$  líneas de salida independientes

##### Impacto Arquitectónico y Optimización de Recursos

El valor fundamental de los multiplexores y demultiplexores en la arquitectura de hardware abarca dos dimensiones críticas: la optimización del cableado físico y la síntesis lógica universal. En primer lugar, la capacidad de conmutar múltiples fuentes de datos sobre un único bus reduce de forma drástica la cantidad de líneas de interconexión y pines en los circuitos integrados, garantizando la viabilidad del diseño modular.  
En segundo lugar, se demuestra el teorema fundamental de la lógica combinacional:  **un Multiplexor actúa como un generador universal de funciones booleanas** . Un MUX de  \$s\$  líneas de selección (poseedor de  \$2^s\$  entradas) puede sintetizar de forma directa cualquier función lógica de  \$k \= s \+ 1\$  variables (o incluso  \$k \= s\$  variables conectando constantes  \$0\$  y  \$1\$  a las entradas) sin requerir compuertas NOT, AND o OR adicionales. Esta propiedad elimina las etapas complejas de minimización de compuertas discretas en aplicaciones donde el espacio y el conteo de componentes están restringidos.  
La selección física del canal de datos prepara el escenario para la subsiguiente etapa del procesamiento: la interpretación y adaptación del formato numérico de las señales transmitidas hacia interfaces estándar.

#### Decodificadores y Codificadores: Conversión de Representaciones de Información

Los datos procesados internamente por los circuitos aritmético-lógicos se expresan en código binario puro o complemento a dos. Sin embargo, para interactuar con dispositivos periféricos, teclados, arreglos de memoria o elementos de visualización, se requiere una traducción continua entre distintas formas de representación digital. Los codificadores y decodificadores operan como traductores formales dentro del flujo de datos del sistema.

* **Codificadores:**  Circuitos combinacionales que convierten una señal activa procedente de un conjunto de  \$2^n\$  líneas de entrada (donde idealmente solo una línea está activa a la vez) en un código binario condensado de  \$n\$  bits en la salida.  
* **Decodificadores:**  Circuitos combinacionales que reciben un código binario de  \$n\$  bits de entrada y activan exactamente una entre  \$2^n\$  líneas de salida. En aplicaciones industriales e integrados TTL, las salidas suelen ser activas en bajo (Active-LOW, p. ej.,  \$\\overline{Y\_i} \= 0\$ ), optimizando el acoplamiento con líneas de habilitación de chips ( *Chip Select* ,  \$\\overline{CS}\$ ).

La conversión de representaciones atiende a restricciones físicas y operativas del hardware:

* **Decimal Codificado en Binario (BCD):**  Representa cada dígito decimal ( \$0\$  al  \$9\$ ) mediante un cuarteto binario ( \$0000\_2\$  a  \$1001\_2\$ ). Debido a que  \$4\$  bits permiten  \$16\$  combinaciones ( \$2^4\$ ), las combinaciones binarias restantes  \$1010\_2, 1011\_2, 1100\_2, 1101\_2, 1110\_2\$  y  \$1111\_2\$  ( \$10\_{10}\$  a  \$15\_{10}\$ ) representan estados inválidos/no utilizados. En el diseño combinacional clásico de decodificadores para displays de 7 segmentos (p. ej., excitadores de cátodo o ánodo común), estas 6 combinaciones se tratan como condiciones de «no importa» ( *Don't Care* ), permitiendo simplificar de forma sustancial las ecuaciones lógicas mediante Mapas de Karnaugh.  
* **Código Gray:**  Código no ponderado cuya propiedad fundamental exige que entre dos valores numéricos adyacentes solo cambie  **un bit** . En codificadores mecánicos y perillas rotativas ( *rotary encoders* ), la transición en binario natural de  \$3\_{10}\$  a  \$4\_{10}\$  ( \$011\_2 \\rightarrow 100\_2\$ ) implica la conmutación simultánea de 3 bits; a nivel físico, los microdesfasajes mecánicos hacen que el sensor lea estados ambiguos transitorios ( *glitches*  o lecturas falsas). El código Gray (construido algebraicamente mediante la técnica del espejo) elimina los errores de conmutación al garantizar transiciones monobit.  
* **Código ASCII:**  Formato estándar de 7 bits que mapea 128 caracteres alfanuméricos y señales de control, permitiendo la traducción de cadenas de texto a buses binarios de hardware.

Transición Binaria Natural (Riesgo de Glitch por conmutación múltiple):  
  011\_2  \---\> \[ 001\_2 / 000\_2 / 111\_2 \] \---\>  100\_2  (¡3 bits conmutan simultáneamente\!)

Transición Código Gray (Sin Glitch, cambio monobit garantizado):  
  010\_Gray (3) \-----------------------------\> 110\_Gray (4) (¡Solo conmuta 1 bit\!)

##### Casos de Aplicación Clave

* **Decodificación de Direcciones de Memoria:**  Un decodificador de  \$n\$  a  \$2^n\$  toma el bus de direcciones del procesador y activa la línea específica de Habilitación de Chip ( \$\\overline{CS}\$  \-  *Chip Select* ) de un banco de memoria RAM, ROM o registro de E/S específico.  
* **Interfaz para Visualización de Datos (Display de 7 Segmentos):**  Decodificadores BCD a 7 segmentos que convierten cuartetos binarios válidos ( \$0000\_2\$ – \$1001\_2\$ ) en combinaciones de excitación para los leds ( \$a, b, c, d, e, f, g\$ ) de un display.  
* **Codificación de Teclados y Matrices de Entradas:**  Codificadores de prioridad que transforman el cierre mecánico de un botón en un código numérico binario utilizable por la CPU.  
* **Sensores de Posición Angular:**  Codificadores ópticos y mecánicos basados en discos con patrón de código Gray para evitar lecturas espurias en la medición de velocidad y posición.

##### Interfaz de Hardware y Traducción Eficiente de Código

La decodificación y codificación eficaces resuelven la brecha entre la abstracción binaria interna del computador y las interfaces físicas de entrada/salida. Sin la decodificación de direcciones por hardware, la arquitectura carecería de un mecanismo determinista para seleccionar celdas de memoria o registros sin colisiones en el bus; del mismo modo, la decodificación BCD optimizada permite traducir magnitudes computadas a formatos visuales sin sobrecargar la unidad central con algoritmos de conversión por software. La velocidad de respuesta del sistema digital depende directamente de la eficiencia de estos bloques combinacionales traducidos a silicio.  
Al integrar los módulos de selección (MUX/DEMUX) con los convertidores de formato (decodificadores/codificadores), se sientan las bases para interconectarlos con los módulos aritméticos en un subsistema unificado de procesamiento.

#### Integración de Bloques Aritméticos y Combinacionales Avanzados

El potencial del diseño lógico combinacional se alcanza mediante la articulación jerárquica de bloques funcionales simples para conformar la ruta de datos ( *datapath* ) de una Unidad Aritmético-Lógica (ALU) y su sistema de control subyacente. Las arquitecturas de computadores no funcionan mediante puertas aisladas, sino mediante la orquestación coordinada de módulos aritméticos, de enrutamiento y de traducción.  
La integración modular articula los siguientes componentes fundamentales:

1. **Bloques Aritméticos:**  El  *Half Adder*  ( \$HA\$ ) suma 2 bits generando Suma ( \$S\$ ) y Acarreo ( \$C\$ ), mientras que el  *Full Adder*  ( \$FA\$ ) procesa 3 bits incorporando el acarreo de entrada ( \$C\_{in}\$ ) y produciendo  \$C\_{out}\$ . Estos se encadenan para formar sumadores/restadores en paralelo con detección de desbordamiento de signo ( \$V\$ ) en aritmética de complemento a dos, acoplados a comparadores de magnitud que evalúan relaciones  \$A \> B\$ ,  \$A \< B\$  y  \$A \= B\$ .  
2. **Bloques de Encaminamiento (MUX/DEMUX):**  Actúan como conmutadores de la ruta de datos. Un MUX de entrada selecciona si la ALU procesará un dato proveniente del banco de registros, de la memoria principal o de una constante; a la salida, un DEMUX o una estructura de bus canaliza el resultado aritmético hacia el destino correcto.  
3. **Bloques de Conversión (Decodificadores):**  Interpretan la palabra de instrucción ( *opcode* ) decodificando las líneas de control que habilitan la operación exacta dentro de la ALU (Suma, Resta, Comparación) y activan los decodificadores de visualización o almacenamiento para presentar el resultado final.

                               RUTA DE DATOS (DATAPATH) INTEGRADA  
                          \+------------------------------------------+  
                          \|                                          \|  
Entradas (Reg A, B) \----\> \| \[MUX Entrada\] \-\> \[ALU: HA/FA/Comp\]      \|  
                          \|                       \|                  \|  
Líneas Control/Opcode \--\> \| \[Decodificador\] \----\> (Operación)        \|  
                          \|                       \|                  \|  
                          \|                       v                  \|  
                          \|  \[DEMUX Salida / Decodificador BCD\]     \|  
                          \+-----------------------+------------------+  
                                                  \|  
                                                  v  
                                Display 7-Seg / Bus Memoria (CS)

##### Escalamiento Modular y Fundamento de la Ruta de Datos

La integración sintética de bloques combinacionales avanzados demuestra cómo la complejidad computacional se construye sobre capas sucesivas de abstracción. La combinación de circuitos aritméticos ( \$HA/FA\$ ) con estructuras de control de flujo (MUX/DEMUX) y traducción de interfaz (decodificadores) consolida la infraestructura física real sobre la cual la CPU ejecuta instrucciones binarias. Esta metodología modular permite escalar el diseño desde compuertas lógicas individuales hasta procesadores completos, garantizando alta velocidad de propagación, bajo consumo de compuertas y una gestión estructurada de los datos a nivel de bus.  
Con la integración funcional del encaminamiento mediante MUX/DEMUX, la conversión de representaciones con decodificadores/codificadores y su acoplamiento con la aritmética digital al cierre de la Semana 5, se completa el alcance teórico-práctico de la Unidad 1 («Análisis de Lógica Combinacional con Diseño Clásico»), dotando al estudiante del marco conceptual e ingenieril para abordar las arquitecturas lógicas secuenciales y la estructura interna de las computadoras.  
