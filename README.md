# Alineamiento de secuencias
<h2> Hola <img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="30">
Soy Sandra Justino! <img src="https://media.giphy.com/media/mGcNjsfWAjY5AEZNw6/giphy.gif" width="50"></h2>


**En esta sección aprenderemos a alinear secuencias cortas o largas con una secuencia de referencia:**

>Una representación simplificada del alineamiento de lecturas de secuenciamiento

![read](https://user-images.githubusercontent.com/84040152/119897276-c7a2a980-bf05-11eb-9f5c-99d27062108a.png)

* La primera fila representa la secuencia de referencia y debajo están las lecturas alineadas. Las lecturas A, D y F coinciden perfectamente con la referencia. La lectura tiene una sola base de diferencia con la referencia, contiene una base de nucleótido G en lugar de la base de nucleótido A de la referencia (lugar indicado por el símbolo *).

Hay una variedad de softwares que son utilizados para alinear, una lista completa se puede encontrar en la página de EBI HTS Mapper(colocar el link)

## Práctica ##

Conoceremos los pasos para realizar alineamientos de lecturas cortas y largas utilizando **BWA y novoAlign**

## 1.1 Alineación de lecturas cortas

En esta sección realizaremos la alineación de lecturas utilizando el sotware **BWA**

BWA es un paquete de software para mapear lecturas de secuenciamiento frente a un genoma de referencia, como el genoma humano. Consta de tres algoritmos: **BWA-backtrack**, **BWA-SW** y **BWA-MEM**. El primer algoritmo esta diseñado para lecturas de secuenciad de Illumina de hasta 100 pb, mientras los dos restantes para secuencias más largas que van desde 70 pb hasta unas pocas megabases. **BWA-MEM** y **BWA-SW** comparten caracteristicas similares, como el soporte de lecturas y alineación quimérica, pero **BWA-MEM** genealmente se recomienda ya que es más rápido y más preciso. **BWA-MEM** también tiene un mejor rendimiento que **BWA-backtrack** para lecturas de Illumina de 70-100 pb. 

Para todos los algoritmos, **BWA** primero necesita construir el índice FM para el genoma de referencia (el comando de índice). Los algoritmos de alineación se invocan con diferentes subcomandos: **aln/samse/sampe** para **BWA-backtrack**, **bwasw** para **BWA-SW** y **mem** para el algoritmo **BWA-MEM** .En esta práctica usaremos el algoritmo **BWA-MEM**.

## Requerimientos

### Instalación de BWA
```
~$ git clone https://github.com/lh3/bwa.git
~$ cd bwa; make
```
## Proceso de alineación 

> Opcional: Indexar la secuencia de referencia `ecoliK12MG1655_ensembl.fna`

Es preferible indexar el archivo fasta de referencia, especialmente cuando se tiene múltiples secuencias como referencias. 

* Crear indice de la secuencia de referencia

```
~$ samtools faidx ecoliK12MG1655_ensembl.fna
```
Este comando producirá el siguiente archivo de índice de referencia

![image](https://user-images.githubusercontent.com/84040152/120679534-e3a2cf80-c45e-11eb-9646-2351837882a3.png)


* Crear indice de referencia BWA 
```
~$ bwa index ecoliK12MG1655_ensembl.fna
```

Este comando producirá los siguientes archivos de índice bwa: 

`ecoliK12MG1655_ensembl.fna.amb`, `ecoliK12MG1655_ensembl.fna.ann`, `ecoliK12MG1655_ensembl.fna.bwt`, `ecoliK12MG1655_ensembl.fna.pac` y  `ecoliK12MG1655_ensembl.fna.sa`

![image](https://user-images.githubusercontent.com/84040152/120681893-7b092200-c461-11eb-902a-7ad559743db2.png)

## Alineamiento de las lecturas de secuenciamiento con la secuencia de referencia

* Alineamiento de lecturas con BWA
```
~$ bwa mem ecoliK12MG1655_ensembl.fna GA2_R1.fastq GA2_R2.fastq > aln-pe.bwa.sam 2> bwa.log
```

![image](https://user-images.githubusercontent.com/84040152/120687966-2cab5180-c468-11eb-84fd-e039173db37d.png)

![image](https://user-images.githubusercontent.com/84040152/120689383-aee84580-c469-11eb-83c4-d69eecf76700.png)


* Ver archivo SAM 

* Para ver el archivo file en el terminal

* Ver archivo BWA SAM
```
~$ less -S aln-pe.bwa.sam
```
![image](https://user-images.githubusercontent.com/84040152/120689536-d6d7a900-c469-11eb-9022-b6ade002cfd8.png)


La sección de alineación consta de varias líneas delimitadas por TAB. Cada línea describe una alineación.
![image](https://user-images.githubusercontent.com/84040152/120689661-f53da480-c469-11eb-9afe-68751d9cba0c.png)


Una mejor vista de los campos es como que se muestra en el ejemplo transpuesto a continuación:
```
@SQ SN:GCA_000005845.2:Chromosome:1:4641652:1 LN:4641652
@PG ID:bwa PN:bwa VN:0.7.13-r1126 CL:bwa mem ecoliK12MG1655_ensembl.fna GA2_R1.fastq GA2_R2.fastq
EAS20_8_6_100_1000_1413 83 GCA_000005845.2:Chromosome:1:4641652:1 3849483 60 100M = 3849364 -219
CGGCAGCGCCAGACAGAATGGCGTAAAGCGCGACAGTTCGTCCGGCAATCCCAACTGGAGCCAGAGACTGATAACAAACAGCAGCAAGTACCAGACCAGA
F@>CCFFE/HFHHHHFFF5F@DFBED@CDBFCEHHDHDHH@BF?BFHE5HHGHHG;===6HHHHHHGHHHHEHHHHHHHGIIHHHHGGHFFFFBFFFFBB 
NM:i:0 MD:Z:100 AS:i:100 XS:i:0

EAS20_8_6_100_1000_1413 163 GCA_000005845.2:Chromosome:1:4641652:1 3849364 60 100M = 3849483 219
GAGAGCAATAAATCCACCGGATGATCGCGCCAGGTTTGACTGGCGATCAGCGCGATGGCGTTCATCAACGTCGCAATCAGCGCCCCTTGCCAACCATAGT
AEGE>FHFHCEGG@EFHEHHHFEFCHHGF@HFHIDHGDHHHDH=F?EEFH@BHHG>>F;F=FDCDFE6BBBFEEC7D6=D6E?:?GFEHBGGCCGE@AFB 
NM:i:0 MD:Z:100 AS:i:100 XS:i:0
```

La sección de encabezado se indica con el símbolo @

```
VN           Versión formato SAM
SO           Orden de clasificacin de alieamiento
S            Diccionario de secuencias de referencia (información)
SN           Nombre de la secuencia de referencia
LN           Longitud de la secuencia de referencia
PG           Programa
ID           Identificador del registro del programa
PN           Nombre del programa
VN           Versión del programa
CL           Linea de comando
```

```
Field        Value
QNAME        EAS20_8_6_100_1000_1413
FLAG         83
RNAME        GCA_000005845.2:Chromosome:1:4641652:1
POS          3849483
MAPQ         60
CIGAR        100M
MRNM/        =
RNEXT
MPOS/        3849364
PNEXT
ISIZE/TLEN   -219
SEQ           CGGCAGCGCCAGACAGAATGGCGTAAAGCGCGACAGTTC
              GTCCGGCAATCCCAACTGGAGCCAGAGACTGATAAC
              AAACAGCAGCAAGTACCAGACCAGA
QUAL          F@>CCFFE/HFHHHHFFF5F@DFBED@
              CDBFCEHHDHDHH@BF?BFHE5HHGHHG;===6HHHHHHG
              HHHHEHHHHHHHGIIHHHHGGHFFFFBFFFFBB
TAG(s)        NM:i:0 MD:Z:100 AS:i:100 XS:i:0
```

* Una explicación básica sobre los campos de formato SAM que se observa arriba
```
Campo         Breve descripción
QNAME
FLAG
RNAME
POS
MAPQ          Calidad del mapeo
CIGAR
RNEXT
PNEXT
TLEN
SEQ
QUAL
TAGs

```
Puede encontrar una explicación más detallada del formato SAM en http://samtools.github.io/hts-specs/

`Los siguientes campos suelen estar marcados para evaluar las lecturas de alineación:`

**1. FLAG**

Este campo contiene el número de bandera para los tipos de alineación de lecturas. Por ejemplo, un par de lectura marcado como "2" es un par de lecturas asignadas correctamente. Las banderas se pueden comprobar
utilizando la herramienta picard explica banderas en http: // broadinstitute.github.io/picard/explain-flags.html

**2. CIGAR**

La cadena CIGAR es una representación de mapeo de secuencia simplificada. La cadena muestra la alineación en el número de bases que se alinean (coinciden / no coinciden) con la referencia, eliminado de la referencia, las inserciones que no están en la referencia y el recorte fácil/difícil de las lecturas de la secuencia de ser alineado con la referencia. Se proporciona una tabla CIGAR simple.

`Tabla 1. Tabla CIGAR simplificada`
```
Op    Descripción
M     Coincidencia de alineación (puede ser una coincidencia de secuencia o una falta de coincidencia)
I     Inserción a la referencia
D     Deleción de la referencia
N     Región omitida de la referencia
S     Recorte fácil (secuencias recortadas presentes en SEQ)
H     Recorte difícil (secuencias recortadas NO presentes en SEQ)
```
*Por ejemplo*
![image](https://user-images.githubusercontent.com/84040152/120715694-963c5780-c48a-11eb-9eb6-b02b514a325f.png)

* En este ejemplo observamos:

`POS -> 3`
`CIGAR -> 5M1D7M1I17M`

El POS indica la posición base en la referencia; en este ejemplo, la lectura B comienza en la posición tres con cinco coincidencias. En la posición nueve, tiene una deleción (resaltada en negro; no está presente en la secuencia de la lectura de la lectura). Siete coincidencias desde la posición 10 antes de una inserción (resaltada en verde; no presente en la referencia). Luego le siguen por 17 coincidencias, incluidas la sustitución en la posición 26. 

Se puede encontrar una tabla CIGAR completa en formato SAM en http://samtools.github.io/hts-specs/

**3. QUAL**

QUAL es un valor que indica la precisión de cada base en la secuencia (SEQ). La calidad se calcula en base a la probabilidad de que una base sea errónea, p, utilizando la fórmula Phred Quality score (http://en.wikipedia.org/wiki/Phred_quality_score): 

![image](https://user-images.githubusercontent.com/84040152/120720717-2f22a100-c492-11eb-94f0-f2b496bb6c9d.png)

En el formato SAM, el valor 'p' se añade con 33 (esto es para permitir que el valor esté dentro del rango de impresión ASCII legible). El campo QUAL para SAM utiliza la siguiente fórmula:

![image](https://user-images.githubusercontent.com/84040152/120721018-a6f0cb80-c492-11eb-9918-20df9b70f981.png)

**4. MAPQ**

MAPQ es el valor de la calidad del mapeo, redondeado al entero más cercano. Utiliza la siguiente fórmula:

![image](https://user-images.githubusercontent.com/84040152/120721107-cee02f00-c492-11eb-8e29-c0c132ccfaf8.png)

Va del valor 0 al 255. Tenga cuidado y consulte la documentación del programa sobre cómo debe interpretarse MAPQ porque diferentes alineadores utilizan diferentes valores de MAPQ. 

## Conversión de SAM en BAM

* Convertir SAM a BAM
`Para convertir el formato SAM en formato BAM, utilizamos SAMTOOLS.`

```
~$ samtools view -uS -o aln-pe.bwa.bam aln-pe.bwa.sam
```
![image](https://user-images.githubusercontent.com/84040152/120726090-f4723600-c49c-11eb-9fe2-0f4c4bdb5851.png)

* Ordenar alineaciones BAM

La clasificación de archivos BAM es un proceso que permite ordenar las lecturas alineadas en la posición alineada con el genoma de referencia. Un archivo BAM ordenado 
suele ser un requisito para algunas, si no la mayoría, de las herramientas de análisis. La clasificación de las lecturas de las lecturas con respecto a la posición de referencia ayuda a aumentar la eficiencia de la lectura, el procesamiento y la compactación del tamaño del archivo.

`La clasificación del archivo BAM puede realizarse con SAMTOOLS.`

**1. Ordenar la alineación con el orden de las coordenadas de referencia**
```
~$ samtools sort -T aln.tmp.sort -o aln-pe.bwa_sorted.bam aln-pe.bwa.bam
```
![image](https://user-images.githubusercontent.com/84040152/120726150-12d83180-c49d-11eb-8dfb-fbcba3199bf8.png)

**2. Alineación del índice**
```
~$ samtools index aln-pe.bwa_sorted.bam
```
![image](https://user-images.githubusercontent.com/84040152/120726333-76625f00-c49d-11eb-9bdc-a90000b349bd.png)

*Esto producirá un archivo de índice BAM*
```
~$ aln-pe.bwa_sorted.bam.bai
```
![image](https://user-images.githubusercontent.com/84040152/120726374-8da14c80-c49d-11eb-96dc-0437b709053f.png)

**3. Marcar/eliminar duplicados**

```
~$ samtools rmdup aln-pe.bwa_sorted.bam aln-pe.bwa_rmdup.bam 2> samtools_rmdup.log
```
![image](https://user-images.githubusercontent.com/84040152/120726455-b88ba080-c49d-11eb-8b2a-7d2a7b333940.png)

* Vuelva a realizar la indexación en el archivo BAM de salida
```
~$ samtools index aln-pe.bwa_rmdup.bam
```
![image](https://user-images.githubusercontent.com/84040152/120726518-db1db980-c49d-11eb-8c77-f4e686f4d052.png)

![image](https://user-images.githubusercontent.com/84040152/120726545-eec92000-c49d-11eb-9dd4-afaafd18f2f5.png)

## 1.2 Alternativo: novoAlign & novoSort




Referencias Bibliográficas

Li H. (2013) Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. arXiv:1303.3997v2 [q-bio.GN]. (if you use the BWA-MEM algorithm or the fastmap command, or want to cite the whole BWA package)



