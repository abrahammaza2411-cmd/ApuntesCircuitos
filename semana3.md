[semana3.md](https://github.com/user-attachments/files/34536524/semana3.md)
### Apuntes Académicos: Semana 3 – Simplificación de Funciones Lógicas mediante Mapas de Karnaugh y Optimización de Circuitos

##### 1\. Contextualización y Propósito de la Simplificación Lógica

En el marco de la Unidad 1 ( **Análisis de Lógica Combinacional con Diseño Clásico** ) de la asignatura  **Diseño de Circuitos** , la simplificación de expresiones booleanas constituye un pilar estratégico para la ingeniería de hardware digital. Si bien la formalización del álgebra de Boole permite la transformación axiomática de expresiones lógicas mediante teoremas de adyacencia, absorción y consenso, la manipulación algebraica pura carece de un algoritmo determinista directo que garantice, en todos los casos, la obtención de la forma mínima absoluta. Esta restricción metodológica exige la transición estructurada hacia técnicas de minimización visual, como los Mapas de Karnaugh (K-Maps), los cuales ofrecen un procedimiento matricial determinista para identificar adyacencias geométricas, eliminar términos redundantes y optimizar la topología física de las redes combinacionales.  
Los objetivos centrales de la Semana 3 sintetizan este salto cualitativo en el diseño combinacional clásico:

* **Minimización mediante Mapas de Karnaugh (K-Maps)**  para funciones lógicas de 2, 3 y 4 variables.  
* **Tratamiento estratégico de condiciones "no importa" (**  ***Don't Care***  **)**  en la optimización sintáctica.  
* **Optimización de esquemas lógicos**  orientada a la reducción estricta de la cantidad de compuertas y literales en hardware.  
* **Principios de implementación física**  sustentados en el uso exclusivo de compuertas lógicas universales (NAND y NOR).

El paso de la manipulación algebraica tradicional a los métodos de agrupación visual resuelve directamente los desafíos de eficiencia en el hardware, minimizando el conteo de compuertas, la superficie de silicio requerida y los retardos de propagación en la implementación física.

##### 2\. Metodología de Minimización Visual: Mapas de Karnaugh (K-Maps) y Condiciones  *Don't Care*

La minimización visual busca transformar las relaciones booleanas tabuladas en patrones geométricos dentro de una matriz celdelar. En la ingeniería de circuitos digitales, la reducción de la complejidad lógica incide de forma directa en el área ocupada por las compuertas físicas, el consumo energético y la velocidad de respuesta del sistema combinacional.

###### *Organización del Mapa y Código Gray*

Los Mapas de Karnaugh estructuran los subespacios booleanos de 2, 3 y 4 variables aprovechando el principio de adyacencia lógica. Para asegurar que celdas contiguas en el mapa difieran exclusivamente en un solo literal, las filas y columnas se ordenan rigurosamente mediante el  **Código Gray**  ( \$00, 01, 11, 10\$ ). A diferencia de la secuencia binaria ponderada convencional —donde la transición del estado  \$01\_2\$  al  \$10\_2\$  implica la conmutación simultánea de dos bits—, el Código Gray garantiza una variación monobític entre celdas adyacentes. Esta propiedad matemática sustenta el teorema de adyacencia algebraico ( \$X Y \+ X Y' \= X\$ ) y evita errores o falsas lecturas transitorias en la conmutación de sensores físicos.  
Adicionalmente, la topología del Mapa de Karnaugh posee  **adyacencia toroidal (o de bordes adyacentes /**  ***wrap-around***  **)** : las celdas ubicadas en los extremos superior e inferior, así como las de las columnas izquierda y derecha, se consideran físicamente continuas, permitiendo la consolidación de grupos que abarcan las esquinas y bordes de la matriz.

###### *ESTRUCTURA MATRICIAL PARA MAPAS DE KARNAUGH DE 3 Y 4 VARIABLES*

*Mapa de Karnaugh de 3 variables (*  *\$A, B, C*\$  *donde las columnas representan*  *\$BC*\$  *en Código Gray):*  
       BC  
 A    00   01   11   10  
    \+----+----+----+----+  
 0  \| m0 \| m1 \| m3 \| m2 \|  
    \+----+----+----+----+  
 1  \| m4 \| m5 \| m7 \| m6 \|  
    \+----+----+----+----+

*Mapa de Karnaugh de 4 variables (*  *\$A, B, C, D*\$  *donde filas representan*  *\$AB*\$  *y columnas*  *\$CD*\$  *en Código Gray):*  
        CD  
 AB   00   01   11   10  
    \+----+----+----+----+  
 00 \| m0 \| m1 \| m3 \| m2 \|  
    \+----+----+----+----+  
 01 \| m4 \| m5 \| m7 \| m6 \|  
    \+----+----+----+----+  
 11 \|m12 \|m13 \|m15 \|m14 \|  
    \+----+----+----+----+  
 10 \| m8 \| m9 \|m11 \|m10 \|  
    \+----+----+----+----+

###### *Reglas de Agrupación Visual*

La obtención de formas lógicas reducidas exige agrupar minitérminos o maxitérminos adyacentes en subconjuntos cuyos tamaños sean estrictamente potencias de dos ( \$2^n \= 1, 2, 4, 8, 16\$ ):

* **Suma de Productos (SOP):**  Se agrupan los unos lógicos correspondientes a los minitérminos ( \$\\sum m\$ ). Las agrupaciones resultantes determinan términos producto donde las variables constantes se conservan y las variables que conmutan entre  \$0\$  y  \$1\$  quedan eliminadas.  
* **Producto de Sumas (POS):**  Se agrupan los ceros lógicos correspondientes a los maxitérminos ( \$\\prod M\$ ). El mapa produce un producto de sumas minimizado, complementario al esquema SOP.

###### *Tratamiento de Condiciones "No Importa" ( Don't Care )*

En diversos diseños combinacionales existen combinaciones de entrada que nunca ocurren físicamente o cuyos valores de salida resultan irrelevantes para la correcta operación del sistema. Dichos estados se definen como condiciones  *Don me importa*  o  *Don't Care*  ( \$X\$ ). En el proceso de minimización visual, el diseñador posee la libertad táctica de asumir cada condición  \$X\$  como un  \$1\$  o como un  \$0\$  según convenga. Al incluirlas estratégicamente como unos, se forman agrupaciones geométricas más amplias ( \$2^n\$ ), reduciendo un número mayor de literales y minimizando de manera sustancial la cantidad de compuertas finales.  
Agrupación Estándar de Minitérminos ( \$\\sum m\$ ),Inclusión de Condiciones  Don't Care  ( \$X\$ )  
Definición y Alcance:  Restringida estrictamente a los estados de salida activos donde la función lógica toma el valor de  \$1\$  ( \$\\sum m\$ ).,"Definición y Alcance:  Incorpora combinaciones de entrada no especificadas o físicamente imposibles ( \$X\$ ), asignándoles arbitrariamente valor  \$1\$  o  \$0\$ ."  
Tamaño de Grupos:  Agrupaciones limitadas exclusivamente por la disposición de los minitérminos  \$1\$  presentes en la matriz.,Tamaño de Grupos:  Maximiza la dimensión de los bloques visuales ( \$2^n\$ ) al combinar términos  \$X\$  con los unos lógicos adyacentes.  
"Impacto en Hardware:  Produce términos con mayor cantidad de literales, requiriendo un número superior de compuertas y entradas activas.","Impacto en Hardware:  Elimina un número óptimo de variables, minimizando de forma drástica el conteo de compuertas lógicas y líneas de interconexión."  
La rigurosa aplicación de estas reglas de minimización visual establece el nexo metodológico entre el análisis formal de funciones y la implementación eficiente de redes lógicas mediante familias de compuertas universales.

##### 3\. Optimización Práctica e Implementación con Compuertas Universales

El objetivo de la simplificación combinacional en la ingeniería digital es traducir las expresiones algebraicas obtenidas en diagramas esquemáticos físicamente realizables, robustos y eficientes. La optimización técnica concluye con la adaptación del esquema mínimo a los circuitos integrados disponibles industrialmente.

###### *Principio de Compuertas Lógicas Universales*

Dentro de la microelectrónica digital, un principio fundamental establece que cualquier función o circuito combinacional puede construirse de forma exclusiva utilizando compuertas  **NAND**  o compuertas  **NOR** . La universalidad de las compuertas NAND y NOR simplifica los procesos de fabricación y ensamblado de tarjetas de circuito impreso (PCB), permitiendo reemplazar redes heterogéneas de compuertas (NOT, AND, OR) por un único tipo de circuito integrado dentro de la familia estándar TTL (como el chip 74LS00 para NAND o 74LS02 para NOR).

###### *Estudio de Caso: Sistema de Control de Tanque y Cisterna*

Para ilustrar la simplificación y su correspondencia con los elementos físicos de captura, se evalúa el circuito de control de una bomba de agua operado por tres variables de entrada:

* \$A\$ : Estado del sensor en el tanque ( \$1 \= \\text{tanque vacío / nivel bajo}\$ ).  
* \$B\$ : Estado del sensor en la cisterna ( \$1 \= \\text{cisterna con agua}, 0 \= \\text{cisterna sin agua}\$ ).  
* \$C\$ : Estado del conmutador de control manual ( \$1 \= \\text{orden manual activada}\$ ).

###### *Vinculación Lógico-Física de Sensores*

La interpretación de la variable  \$A\$  depende directamente de la tecnología del sensor seleccionado:

1. **Sensor Activo en Alto (Normally Open / Vacío envía**  **\$5\\text{V}**\$  **):**  Se configura para entregar  \$5\\text{V}\$  (1 lógico) cuando el tanque requiere agua (vacío) y  \$0\\text{V}\$  (0 lógico) cuando alcanza su nivel máximo. Bajo este criterio, la condición de activación conduce a la función  \$F \= B(A \+ C)\$ .  
2. **Sensor Activo en Bajo (Normally Closed / Lleno envía**  **\$5\\text{V}**\$  **):**  Entrega  \$5\\text{V}\$  (1 lógico) en estado lleno y  \$0\\text{V}\$  (0 lógico) cuando detecta el nivel bajo. En esta variante física, el literal debe complementarse en la función lógica, resultando en  \$F \= B(\\neg A \+ C)\$ .

Considerando el sensor configurado para emitir  \$5\\text{V}\$  ( \$1\$ ) al estar vacío y  \$0\\text{V}\$  ( \$0\$ ) al estar lleno, el comportamiento tabulado en la tabla de verdad determina que la salida de activación ( \$F \= 1\$ ) requiere la presencia de agua en la cisterna ( \$B \= 1\$ ) bajo tres combinaciones específicas de entrada. Estas combinaciones se asocian a los minitérminos  \$m\_3 (011\_2)\$ ,  \$m\_6 (110\_2)\$  y  \$m\_7 (111\_2)\$ :  
\$\$\\text{Expresión Canónica SOP: } F \= (\\neg A B C) \+ (A B \\neg C) \+ (A B C)\$\$

###### *Demostración y Derivación Algebraica Paso a Paso:*

1. **Aplicación del teorema de idempotencia (**  **\$X \+ X \= X**\$  **):**  Se duplica el minitérmino  \$A B C\$  para asociarlo de forma adyacente con cada uno de los otros dos términos producto:  \$\$F \= (\\neg A B C \+ A B C) \+ (A B \\neg C \+ A B C)\$\$  
2. **Aplicación de la propiedad distributiva (factorización de términos adyacentes):**  Se extraen los factores comunes en cada subgrupo algebraico:  \$\$F \= B C (\\neg A \+ A) \+ A B (\\neg C \+ C)\$\$  
3. **Aplicación del postulado del complemento (**  **\$\\neg X \+ X \= 1**\$  **) y elemento neutro (**  **\$X \\cdot 1 \= X**\$  **):**   \$\$F \= B C (1) \+ A B (1) \= B C \+ A B\$\$  
4. **Factorización final por término común**  **\$B**\$  **:**   \$\$F \= B(A \+ C)\$\$

**Criterio de Ingeniería y Análisis de Casos Límite:**  
*"La función minimizada final*  *\$F \= B(A \+ C)*\$  *reduce la topología original a una compuerta OR de dos entradas y una compuerta AND final. No obstante, la selección final de los componentes electromecánicos debe evaluarse bajo criterios de seguridad industrial. Por ejemplo, la presencia del minitérmino*  *\$m\_3 (011\_2)*\$  *implica que si la cisterna posee agua (*  *\$B=1*\$  *) y el operador activa el mando manual (*  *\$C=1*\$  *), la bomba se encenderá aun cuando el tanque no esté en nivel bajo (*  *\$A=0*\$  *). Aunque la lógica booleana lo especifique correctamente según la tabla de verdad, el diseñador del circuito debe determinar si este comportamiento representa un riesgo de desbordamiento en el entorno de aplicación real."*  
El dominio de las metodologías de minimización visual y el dominio de esquemas basados en compuertas universales revisados en esta tercera semana consolidan la fase de diseño combinacional clásico, permitiendo abordar con rigor el diseño de bloques aritméticos y combinacionales complejos en las subsiguientes semanas del curso.  
