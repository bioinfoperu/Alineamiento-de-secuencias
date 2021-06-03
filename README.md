# Alineamiento de secuencias
<h2> Hola <img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="30">
Soy Sandra Justino! <img src="https://media.giphy.com/media/mGcNjsfWAjY5AEZNw6/giphy.gif" width="50"></h2>


**En esta sección aprenderemos a alinear secuencias cortas o largas con una secuencia de referencia:**

>Una representación simplificada del alineamiento de lecturas

![read](https://user-images.githubusercontent.com/84040152/119897276-c7a2a980-bf05-11eb-9f5c-99d27062108a.png)

* La primera fila representa la secuencia de referencia y debajo están las lecturas alineadas. Las lecturas A, D y F coinciden perfectamente con la referencia. La lectura tiene una sola base de diferencia con la referencia, contiene una base de nucleótido G en lugar de la base de nucleótido A de la referencia (lugar indicado por el símbolo *).

Hay una variedad de softwares que son utilizados para alinear, una lista completa se puede encontrar en la página de EBI HTS Mapper(colocar el link)

## Práctica ##

Conoceremos los pasos para realizar alineamientos de lecturas cortas y largas utilizando **BWA y novoAlign**

## 1.1 Alineación de lecturas cortas

En esta sección realizaremos la alineación de lecturas utilizando el sotware **BWA**

BWA es un paquete de software para mapear lecturas de secuenciamiento frente a un genoma de referencia, como el genoma humano. Consta de tres algoritmos: BWA-backtrack, BWA-SW y BWA-MEM. El primer algoritmo esta diseñado para lecturas de secuenciad de Illumina de hasta 100 pb, mientras los dos restantes para secuencias más largas que van desde 70 pb hasta unas pocas megabases. BWA-MEM y BWA-Sw comparten caracteristicas similares, como el soporte de lecturas y alineación quimérica, pero BWA-MEM genealmente se recomienda ya que es más rápido y más preciso. BWA-MEM también tiene un mejor rendimiento que BWA-backtrack para lecturas de Illumina de 70-100 pb. 

Para todos los algoritmos, BWA primero necesita construir el índice FM para el genoma de referencia (el comando de índice). Los algoritmos de alineación se invocan con diferentes subcomandos: aln/samse/sampe para BWA-backtrack, bwasw para BWA-SW y mem para el algoritmo BWA-MEM.

Instalacin de BWA
git clone https://github.com/lh3/bwa.git
cd bwa; make

Referencias Bibliográficas

Li H. (2013) Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. arXiv:1303.3997v2 [q-bio.GN]. (if you use the BWA-MEM algorithm or the fastmap command, or want to cite the whole BWA package)



