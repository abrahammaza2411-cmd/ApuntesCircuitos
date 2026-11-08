### APUNTES ACADÉMICOS INTEGRALES: SEMANA 2 \- ÁLGEBRA DE BOOLE Y FORMAS CANÓNICAS

* **Fecha:**  25 de Octubre de 2026

###### *1.2 Objetivos de Aprendizaje y Alcance Operativo (Semana 2\)*

* **Transformación Sistemática:**  Traducir requerimientos y especificaciones en enunciado verbal a representaciones formales unívocas mediante tablas de verdad.  
* **Representación Canónica Dual:**  Formular e interpretar funciones lógicas a través de las representaciones canónicas de Suma de Productos ( \$\\text{SOP} / \\Sigma m\$ ) y Producto de Sumas ( \$\\text{POS} / \\Pi M\$ ).  
* **Minimización Algebraica de Circuitos:**  Optimizar funciones booleanas complejas aplicando sistemáticamente los axiomas de Huntington, teoremas algebraicos y leyes de dualidad.

La rigurosidad matemática del álgebra booleana permite mapear variables físicas y señales discrecionalmente modeladas a expresiones operativas optimizadas, garantizando la correcta síntesis de redes combinacionales.

##### 2\. FUNDAMENTOS TEÓRICOS: ÁLGEBRA DE BOOLE Y POSTULADOS DE HUNTINGTON

La formalización del comportamiento discreto en los circuitos digitales descansa sobre un sistema algebraico definido en el conjunto  \$\\{0, 1\\}\$ , operado mediante tres funciones primitivas: la suma lógica ( \$\\text{OR}\$ ,  \$+\$ ), el producto lógico ( \$\\text{AND}\$ ,  \$\\cdot\$ ) y el complemento ( \$\\text{NOT}\$ ,  \$'\$ ). Los postulados de Huntington establecen la base axiomática que garantiza la coherencia en la manipulación simbólica de dichas variables. En el diseño de arquitecturas de hardware, observar la jerarquía de precedencia operacional resulta crítico: una alteración en el orden de evaluación distorsiona el árbol sintáctico de la función, derivando en un mapeo erróneo de las compuertas lógicas e induciendo estados indeterminados en los buses de salida.

###### *2.1 Mapeo Fundamental entre Circuitos Lógicos y Expresiones Algebraicas*

Existe un isomorfismo estructural directo entre los componentes físicos de una red combinacional y las variables o compuertas del modelo algebraico booleano:  
Componente del Circuito Digital,Representación Matemática Booleana  
Cable de entrada,"Variable ( \$A, B, C\$ )"  
Compuerta  \$\\text{AND}\$,Producto ( \$\\cdot\$ )  
Compuerta  \$\\text{OR}\$,Suma ( \$+\$ )  
Compuerta  \$\\text{NOT}\$,Complemento ( \$'\$ )  
Salida del circuito,Valor evaluado de la función ( \$Y\$  o  \$F\$ )  
Para la correcta evaluación formal de expresiones booleanas y su posterior traducción a hardware, se establece la jerarquía estricta de precedencia operacional:  
\$\$\\text{1º Operador NOT } (') \\longrightarrow \\text{2º Operador AND } (\\cdot) \\longrightarrow \\text{3º Operador OR } (+)\$\$

###### *2.2 Sistema Axiomático: Postulados de Huntington, Teoremas y Principio de Dualidad*

El cálculo proposicional e inequívoco sobre el conjunto  \$\\{0, 1\\}\$  se estructura a través de los axiomas y teoremas de Huntington tabulados a continuación:  
Nombre del Postulado / Teorema,Forma OR (Suma),Forma AND (Dual)  
Identidad,\$X \+ 0 \= X\$,\$X \\cdot 1 \= X\$  
Complemento,\$X \+ X' \= 1\$,\$X \\cdot X' \= 0\$  
Conmutativa,\$X \+ Y \= Y \+ X\$,\$XY \= YX\$  
Distributiva,\$X(Y \+ Z) \= XY \+ XZ\$,\$X \+ YZ \= (X \+ Y)(X \+ Z)\$  
Elemento nulo,\$X \+ 1 \= 1\$,\$X \\cdot 0 \= 0\$  
Idempotencia,\$X \+ X \= X\$,\$X \\cdot X \= X\$  
Involución,\$(X')' \= X\$,—  
Absorción,\$X \+ XY \= X\$,\$X(X \+ Y) \= X\$  
Absorción del complemento,\$X \+ X'Y \= X \+ Y\$,\$X(X' \+ Y) \= XY\$  
Adyacencia,\$XY \+ XY' \= X\$,\$(X \+ Y)(X \+ Y') \= X\$  
Consenso,\$XY \+ X'Z \+ YZ \= XY \+ X'Z\$,\$(X+Y)(X'+Z)(Y+Z) \= (X+Y)(X'+Z)\$

###### *Principio de Dualidad*

El  **Principio de Dualidad**  sostiene que cualquier identidad algebraica booleana preserva su validez matemática si se reemplazan de manera simultánea los operadores de suma y producto ( \$+ \\leftrightarrow \\cdot\$ ), así como las identidades constantes ( \$0 \\leftrightarrow 1\$ ), manteniendo las variables lógicas en su forma original (sin aplicarles complemento).

###### *2.3 Demostraciones Algebraicas Clave y Leyes de De Morgan*

A partir de las reglas axiomáticas, es posible verificar identidades booleanas que difieren del álgebra convencional. A continuación se demuestra rigurosamente la ley distributiva especial de la suma sobre el producto:  \$A \+ BC \= (A \+ B)(A \+ C)\$ .  
Partiendo de la expresión desplegada del miembro derecho:  
\$\$\\text{Expresión de partida: } (A \+ B)(A \+ C)\$\$

1. Aplicación del postulado  **Distributivo** :  \$\$= AA \+ AC \+ AB \+ BC\$\$  
2. Aplicación del teorema de  **Idempotencia**  ( \$AA \= A\$ ):  \$\$= A \+ AC \+ AB \+ BC\$\$  
3. Factorización del literal común  \$A\$  mediante el postulado  **Distributivo** :  \$\$= A(1 \+ C \+ B) \+ BC\$\$  
4. Aplicación del teorema del  **Elemento Nulo**  ( \$1 \+ C \+ B \= 1\$ ) e  **Identidad**  ( \$A \\cdot 1 \= A\$ ):  \$\$= A(1) \+ BC \= A \+ BC \\quad \\blacksquare\$\$

###### *Leyes de De Morgan e Implementación con Compuertas Universales*

Las  **Leyes de De Morgan**  formalizan la negación de expresiones compuestas mediante la regla operativa:  *"romper la barra de negación principal y cambiar el operador lógico central"* . Para un sistema de tres variables:  
\$\$(A \\cdot B \\cdot C)' \= A' \+ B' \+ C'\$\$  
\$\$(A \+ B \+ C)' \= A' \\cdot B' \\cdot C'\$\$  
Desde la perspectiva de la ingeniería de la microelectrónica, las leyes de De Morgan constituyen la herramienta analítica fundamental para la transformación de bloques de lógica  \$\\text{AND-OR}\$  hacia topologías integradas basadas exclusivamente en compuertas  **NAND**  o  **NOR** . Dado que con compuertas  \$\\text{NAND}\$  (o únicamente  \$\\text{NOR}\$ ) es posible construir cualquier circuito combinacional (propiedad de universalidad), De Morgan permite a los diseñadores simplificar el proceso de fabricación en silicio y optimizar la disposición física de los transistores en la celda integrada.  
Esta formalización matemática de reglas y transformaciones axiomáticas constituye la base analítica necesaria para abordar la interpretación de requerimientos lógicos en problemas reales de ingeniería.

##### 3\. ESPECIFICACIÓN LÓGICA Y CONSTRUCCIÓN DE TABLAS DE VERDAD

El modelado de sistemas digitales requiere traducir especificaciones verbales hacia descripciones lógicas deterministas. El lenguaje natural presenta inconsistencias de interpretación; un sesgo en la lectura de un enunciado modifica la asignación de minitérminos en la tabla de verdad, alterando la función lógica y generando un circuito que no responde a los requerimientos de diseño.

###### *3.1 Análisis de Ambigüedad en Enunciados: El Caso de la Sirena y Sensores*

Considere la especificación funcional:  *"La sirena suena cuando se activan dos sensores"* , implementada en un sistema de monitoreo con tres sensores discretos ( \$A, B, C\$ ). El enunciado admite dos interpretaciones lógicas distintas:  
                  ┌─────────────────────────────────────────────────────────┐  
                  │ ¿La sirena suena cuando se activan dos sensores?        │  
                  └────────────────────────────┬────────────────────────────┘  
                                               │  
                      ┌────────────────────────┴────────────────────────┐  
                      ▼                                                 ▼  
        \[ Lectura 1: Exactamente 2 \]                       \[ Lectura 2: Al menos 2 \]  
    • Activación exclusiva en m(3, 5, 6\)               • Activación inclusiva en m(3, 5, 6, 7\)  
    • F(1,1,1) \= 0 (Sirena apagada)                    • F(1,1,1) \= 1 (Sirena encendida)  
    • Inadecuado para seguridad                        • Criterio real de ingeniería

* **Lectura 1 (Exactamente dos sensores activos):**  Limita la activación de la función exclusivamente a las combinaciones donde dos entradas equivalen a  \$1\$ . Las filas correspondientes a esta condición son  \$F \= \\Sigma m(3, 5, 6)\$ . Bajo esta premisa, si coinciden tres sensores activos ( \$111\$ , minitérmino  \$m\_7\$ ), la salida evalúa en  \$0\$ . En sistemas de detección de incendios o intrusión, apagar la alarma ante la activación total de los sensores constituye una falla de diseño inadmisible.  
* **Lectura 2 (Al menos dos sensores activos):**  Define la condición de activación como la presencia de dos o más señales en nivel alto. Las filas asociadas son  \$F \= \\Sigma m(3, 5, 6, 7)\$ . Esta interpretación cumple con los requerimientos técnicos reales exigidos en entornos de seguridad industrial.

###### *3.2 Criterios de Diseño Eléctrico y Selección de Sensores (Caso Cisterna, Tanque y Control Manual)*

En la automatización de procesos hidráulicos, la polarización lógica de los transductores condiciona la tabla de verdad. Considere un sistema de bombeo dependiente de tres variables de entrada:

* \$A\$ : Estado del Tanque Alto ( \$1 \\rightarrow\$  nivel bajo, requiere llenado de agua;  \$0 \\rightarrow\$  nivel lleno).  
* \$B\$ : Estado de la Cisterna ( \$1 \\rightarrow\$  cisterna con agua / llena;  \$0 \\rightarrow\$  cisterna vacía / sin agua).  
* \$C\$ : Comando de activación Manual ( \$1 \\rightarrow\$  encendido manual solicitado;  \$0 \\rightarrow\$  inactivo).

La función lógica de bombeo  \$F \= B(A+C)\$  gobierna la bomba. A continuación se presenta la tabla de verdad estructurada formalmente con los vectores de entrada, los términos intermedios y las salidas asignadas:  
\$A\$,\$B\$,\$C\$,\$A+C\$,\$F\$,Término Asignado  
0,0,0,0,0,\$M\_0\$  
0,0,1,1,0,\$M\_1\$  
0,1,0,0,0,\$M\_2\$  
0,1,1,1,1,\$m\_3\$  
1,0,0,1,0,\$M\_4\$  
1,0,1,1,0,\$M\_5\$  
1,1,0,1,1,\$m\_6\$  
1,1,1,1,1,\$m\_7\$

###### *Análisis de Ingeniería Digital de Sistemas de Control*

Al examinar la fila  \$m\_3\$  ( \$A=0, B=1, C=1 \\implies F=1\$ ), la tabla indica que la bomba se enciende debido a la orden manual ( \$C=1\$ ) sumada a la disponibilidad efectiva de agua en la cisterna ( \$B=1\$ ), aun cuando el tanque alto no requiere suministro ( \$A=0\$ ). El diseñador de hardware debe evaluar si este comportamiento es deseable para la aplicación (por ejemplo, para pruebas de mantenimiento) o si requiere bloqueos lógicos adicionales que prevengan sobrellenados en el depósito superior.  
La estructuración rigurosa de las tablas de verdad permite dar el paso fundamental hacia la formalización algebraica normalizada mediante formas canónicas.

##### 4\. FORMAS CANÓNICAS: SUMA DE PRODUCTOS (SOP) Y PRODUCTO DE SUMAS (POS)

Las formas canónicas son las representaciones algebraicas estandarizadas y unívocas de una función booleana extraída de una tabla de verdad. Permiten expresar la misma lógica mediante dos visiones duales: la caracterización del espacio de estados donde la salida se activa ( \$F=1\$ ), o el mapeo de las condiciones de inactividad ( \$F=0\$ ).

###### *4.1 Definición Matemática de Minitérminos ( \$m\_i\$ ) y Maxitérminos ( \$M\_i\$ )*

* **Minitérmino (**  **\$m\_i**\$  **):**  Término de producto lógico ( \$\\text{AND}\$ ) que contiene las  \$n\$  variables de la función. Una variable se incluye en su forma directa (sin complementar) si en la fila de evaluación toma el valor  \$1\$ , y en su forma complementada si toma el valor  \$0\$ . Un minitérmino evalúa en  \$1\$  únicamente para el índice decimal asignado a dicha fila.  
* **Maxitérmino (**  **\$M\_i**\$  **):**  Término de suma lógica ( \$\\text{OR}\$ ) que abarca las  \$n\$  variables del dominio. Una variable aparece en su forma directa (sin complementar) si en la fila toma el valor  \$0\$ , y complementada si toma el valor  \$1\$ . Un maxitérmino evalúa en  \$0\$  exclusivamente para el índice de su combinación.

Ambas representaciones cumplen la relación de complementariedad dual  \$M\_i \= (m\_i)'\$ .  
Para un dominio de 3 variables ( \$A, B, C\$ ), los 8 estados canónicos posibles se parametrizan en la siguiente tabla:  
Subíndice ( \$i\$ ),Valores ( \$A\\ B\\ C\$ ),Minitérmino equivalente ( \$m\_i\$ ),Maxitérmino equivalente ( \$M\_i\$ )  
0,0 0 0,\$A'B'C'\$,\$A \+ B \+ C\$  
1,0 0 1,\$A'B'C\$,\$A \+ B \+ C'\$  
2,0 1 0,\$A'BC'\$,\$A \+ B' \+ C\$  
3,0 1 1,\$A'BC\$,\$A \+ B' \+ C'\$  
4,1 0 0,\$AB'C'\$,\$A' \+ B \+ C\$  
5,1 0 1,\$AB'C\$,\$A' \+ B \+ C'\$  
6,1 1 0,\$ABC'\$,\$A' \+ B' \+ C\$  
7,1 1 1,\$ABC\$,\$A' \+ B' \+ C'\$

###### *4.2 Suma de Productos (SOP /  \$\\Sigma m\$ ): Enfoque en Estados Activos ( \$F=1\$ )*

La regla para construir la forma canónica  \$\\text{SOP}\$  requiere seleccionar únicamente las filas de la tabla de verdad donde la función evalúa en  \$F \= 1\$ . Se extrae el minitérmino ( \$m\_i\$ ) de cada fila activa y se enlazan todos los términos mediante el operador de suma lógica ( \$\\text{OR}\$ ).  
Tomando como referencia la función de la sirena de seguridad bajo la especificación de al menos dos sensores ( \$F \= 1\$  en las filas 3, 5, 6 y 7), la sumatoria reducida y su despliegue canónico se expresan como:  
\$\$F \= \\Sigma m(3, 5, 6, 7)\$\$  
\$\$F \= A'BC \+ AB'C \+ ABC' \+ ABC\$\$

###### *4.3 Producto de Sumas (POS /  \$\\Pi M\$ ): Enfoque en Estados Inactivos ( \$F=0\$ )*

La construcción de la forma canónica  \$\\text{POS}\$  consiste en registrar las filas donde la función evalúa en  \$F \= 0\$ . Por cada fila inactiva, se toma el maxitérmino ( \$M\_i\$ ) correspondiente y se combinan mediante el operador de producto lógico ( \$\\text{AND}\$ ).  
Para la función de la sirena, las condiciones inactivas se presentan en los índices 0, 1, 2 y 4\. Su representación productoria reducida y despliegue canónico corresponden a:  
\$\$F \= \\Pi M(0, 1, 2, 4)\$\$  
\$\$F \= (A \+ B \+ C)(A \+ B \+ C')(A \+ B' \+ C)(A' \+ B \+ C)\$\$

###### *Demostración de Complementariedad de Índices*

En una función booleana de  \$n\$  variables, el espacio de estados  \$2^n\$  se compone de la unión disjunta de los índices de minitérminos y maxitérminos. Para  \$n=3\$ , el conjunto universo de índices es  \$U \= \\{0, 1, 2, 3, 4, 5, 6, 7\\}\$ . Los subíndices de la forma productoria  \$\\Pi M\$  corresponden al complemento del conjunto de minitérminos  \$\\Sigma m\$ :  
\$\$\\text{Si } \\text{SOP} \= \\{3, 5, 6, 7\\} \\implies \\text{POS} \= U \\setminus \\text{SOP} \= \\{0, 1, 2, 4\\}\$\$  
La obtención de estas expresiones canónicas representa una formulación completa del sistema lógico; sin embargo, implementarlas directamente resultaría en esquemas de hardware sobredimensionados, exigiendo procesos de simplificación algebraica.

##### 5\. MÉTODOS DE SIMPLIFICACIÓN ALGEBRAICA Y CASOS PRÁCTICOS DE APLICACIÓN

La minimización de expresiones booleanas constituye una fase fundamental en la arquitectura de hardware digital. Las formas canónicas puras suelen incluir redundancia operacional. Aplicar rigurosamente los postulados y teoremas permite reducir el número de términos y literales de la función, reduciendo el consumo de compuertas lógicas integradas, disminuyendo el costo del encapsulado del circuito impreso y optimizando la velocidad de propagación física de las señales.

###### *5.1 Procedimiento de Minimización Paso a Paso*

A continuación se desarrolla en detalle el proceso de reducción algebraica para la función lógica:  
\$\$F \= A \+ A'B \+ AB'C\$\$

1. **Aplicación del Teorema de Absorción del Complemento:**  Evaluando la interacción entre los dos primeros términos ( \$A \+ A'B\$ ), la identidad de absorción del complemento ( \$X \+ X'Y \= X \+ Y\$ ) reduce la subexpresión a  \$(A \+ B)\$ :  \$\$F \= (A \+ B) \+ AB'C \\quad \\text{Por Absorción del Complemento: } A \+ A'B \= A \+ B \\text{}\$\$  
2. **Reordenamiento por Propiedad Conmutativa:**  Se reordenan los sumando para agrupar las variables con el literal  \$A\$ :  \$\$F \= A \+ AB'C \+ B \\quad \\text{Por Propiedad Conmutativa}\$\$  
3. **Aplicación del Teorema de Absorción Estándar:**  Analizando el bloque  \$A \+ A(B'C)\$  bajo la regla  \$X \+ XY \= X\$  (tomando  \$Y \= B'C\$ ), el término  \$AB'C\$  es absorbido por  \$A\$ :  \$\$F \= A \+ B \\quad \\text{Por Teorema de Absorción: } A \+ A(B'C) \= A \\text{} \\quad \\blacksquare\$\$

###### *Evaluación del Impacto Técnico en Hardware*

La transformación efectuada demuestra la importancia de la minimización lógica previa al diseño físico:  
    Expresión Canónica / Compleja                       Expresión Optimizada  
    • 3 Términos / 5 Literales                          • 1 Término / 2 Literales  
    • Requiere: NOT, ANDs de múltiples entradas u OR    • Requiere: 1 Compuerta OR de 2 entradas

El proceso simplifica un circuito redundante de tres términos y cinco literales a una única compuerta  \$\\text{OR}\$  de dos entradas, eliminando etapas de retardo y optimizando el espacio en silicio.

###### *5.2 Síntesis de Casos Prácticos y Talleres de Aplicación*

###### *Taller 1: Alarma de Seguridad de Vehículo (Sistema Motor/Cinturón/Puerta)*

Para el circuito de alerta en el tablero de un automóvil, las variables discretas de entrada se definen como:

* \$A\$ : Estado del Motor ( \$1 \\rightarrow\$  encendido).  
* \$B\$ : Estado del Cinturón de Seguridad ( \$0 \\rightarrow\$  no abrochado;  \$1 \\rightarrow\$  abrochado).  
* \$C\$ : Estado de la Puerta del Conductor ( \$1 \\rightarrow\$  abierta).

La especificación establece que la alarma debe sonar ( \$F=1\$ ) si el motor está encendido ( \$A=1\$ )  **Y**  el cinturón no está abrochado ( \$B=0\$ ),  **O**  si la puerta se encuentra abierta ( \$C=1\$ ).  
La función algebraica traduce el requerimiento a la siguiente expresión lógica:  
\$\$\\text{Alarma} \= A \\cdot B' \+ C\$\$

###### *Taller 2: Sistema de Luz de Pasillo con Interruptor Maestro*

En el diseño del control de iluminación para una zona de paso, se parametrizan los siguientes conmutadores:

* \$A, B\$ : Conmutadores situados en los extremos del pasillo (cuya conmutación de estado sigue un comportamiento XOR).  
* \$M\$ : Interruptor maestro de control.

La especificación exige que cuando el interruptor maestro esté activo ( \$M=1\$ ), la luz debe permanecer apagada de forma prioritaria e incondicional, deshabilitando la acción de los conmutadores  \$A\$  y  \$B\$ . La función lógica resultante se modela multiplicando la condición del pasillo por el complemento del maestro ( \$M'\$ ):  
\$\$\\text{Luz} \= (A \\oplus B) \\cdot M'\$\$  
La aplicación de las reglas algebraicas simplificadoras constituye el paso previo y obligatorio para optimizar redes combinacionales complejas. Este dominio de la simplificación por postulados provee la fundamentación analítica requerida antes de abordar técnicas de optimización visual avanzadas, tales como los Mapas de Karnaugh.  
