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
```
git clone https://github.com/lh3/bwa.git
cd bwa; make
```
Proceso de alineación
> Opcional: Indexar la secuencia de referencia >ecoliK12MG1655_ensembl.fna.fai

Es preferible indexar el archivo fasta de referencia, especialmente cuando se tiene múltiples secuencias como referencias. El indice .fai permite un acceso eficiente en el archivo de alineación a regiones arbitrarias dentro de estas secuencias de referencia.

Crear indice de la secuencia de referencia

```
samtools faidx ecoliK12MG1655_ensembl.fna
```
Este comando producirá el siguiente archivo de índice de referencia


![image](https://user-images.githubusercontent.com/84040152/120679534-e3a2cf80-c45e-11eb-9646-2351837882a3.png)

Crear indice de referencia BWA 

```
bwa index ecoliK12MG1655_ensembl.fna
```
Este comando producirá los siguientes archivos de índice bwa: 
> ecoliK12MG1655_ensembl.fna.amb, ecoliK12MG1655_ensembl.fna.ann, ecoliK12MG1655_ensembl.fna.bwt, ecoliK12MG1655_ensembl.fna.pac, ecoliK12MG1655_ensembl.fna.sa

![image](https://user-images.githubusercontent.com/84040152/120681893-7b092200-c461-11eb-902a-7ad559743db2.png)

Alineamiento de las lecturas de secuenciamiento con la secuencia de referencia

Alineamiento de lecturas con BWA

```
bwa mem ecoliK12MG1655_ensembl.fna GA2_R1.fastq GA2_R2.fastq > aln-pe.bwa.sam 2> bwa.log
```

![image](https://user-images.githubusercontent.com/84040152/120687966-2cab5180-c468-11eb-84fd-e039173db37d.png)

![image](https://user-images.githubusercontent.com/84040152/120689383-aee84580-c469-11eb-83c4-d69eecf76700.png)


Ver archivo SAM 

Para ver el archivo file en el terminal

Ver archivo BWA SAM

```
less -S aln-pe.bwa.sam
```

![image](https://user-images.githubusercontent.com/84040152/120689536-d6d7a900-c469-11eb-9022-b6ade002cfd8.png)

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

![image](https://user-images.githubusercontent.com/84040152/120689661-f53da480-c469-11eb-9afe-68751d9cba0c.png)

La sección de encabezado se indica con el símbolo @.

Referencias Bibliográficas

Li H. (2013) Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. arXiv:1303.3997v2 [q-bio.GN]. (if you use the BWA-MEM algorithm or the fastmap command, or want to cite the whole BWA package)



