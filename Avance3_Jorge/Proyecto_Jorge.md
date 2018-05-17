##  Proyecto final de Bioinformática 2018
### Por Jorge Cruz Nicolás

#### *Procesamiento y análisis preliminares para el género Abies en México*

### Introducción

A diferencia de otros grupos de plantas procedentes de Norteamérica al territorio que hoy es México, el género *Abies* es de una llegada relativamente reciente posiblemente entre el Mioceno
tardío y el Plioceno (Aguirre-Planter *et al.*, 2012). En taxa de reciente divergencia uno de los grandes problemas es la arbitrariedad en los niveles taxonómicos, en este sentido los métodos filogenéticos brindan de manera más objetiva relaciones de parentesco. Sin embargo, en especies no modelo que comparten polimorfismos ancestrales, inconsistencias entre árboles de genes y especies, es dificil hacer una delimitación taxónomica. Los métodos de secuenciación de nueva generación (ej: Genotyping-by-Sequencing) nos permiten la generación de miles de Single Nucleotide Polymorphism (SNPs) (Schmidt, 2017), lo que se traduce en la construcción de filogenias más robustas en especies no modelo (ej. Fitz-Gibbon et al., 2017; Massatti et al., 2016). 

Para llegar a este paso necesitamos procesar los datos crudos. Los problemas que enfrentamos con recurrencia son: datos faltantes y limitaciones en capacidades computacionales.  Cuando se trabaja con especies no modelo además la ausencia de un genoma de referencia adecuado,  En análisis análisis filogenéticos uno de los softwares más citados en la literatura para el ensamblado de novo es PyRAD de acuerdo con Eaton (2014), a partir de datos de prueba sus resultados fueron superiores a los obtenidos con  Stacks, en cuanto a los valores de consenso y la eficiencia de análisis de tiempo, porque permite:

*  Variación en el número de indels, dentro y entre muestras.
*  El ensamblado de sitios altamente divergentes.
* Capacidad de identificar traslape parcial entre secuencias . 

Este proyecto es un procesamiento preliminar con una submuestra de datos crudos generados con GBS para el género *Abies* en México, utilizando ipyrad.

### Métodos

Se realizo extracción de ADN con el DNeasy Plant Mini Kit (Qiagen) a partir de ejemplares del género *Abies*  muestreados en poblaciones naturales (Aguirre-Planter *et al.,*2000;) y ejemplares de herbario. 

El servicio de secuenciación se realizó en la Plate-Forme d’Analyses Génomiques, de l’ Institut de Biologie Integrative et des Systémes de l’Université Laval. El DNA genómico fue digestado con la enzima PstI. 


#### Programas utilizados

Se realizaron los siguientes análisis:
Inicialmente se realizo un fastqc con biocontainer/fastqc con la siguiente instrucción:

sudo docker run -rm -v /home/usuario/Documentos:/data biocontainers/fastqc fastqc gzip HI.4558.006.Sebastian4_R1.fastq.gz

#### Ensamblado de novo
Se utilizo el programa ipyrad 0.7.23 (Eaton & Overcast, 2015). Una vez instalado el programa en la computadora, se necesitan tres ingredientes:
1) Un archivo de parámetros generados en el programa.
2) Un archivo con los barcodes.txt
3) Un archivo con los fastq.gz

Inicialmente se intento correr los datos de una placa completa, por alguna razón que examinaré en próximas semanas el programa colapsaba en el paso 3. Lo que se realizo fue probar dos submuestras diferentes y al final modificando un poco los paramétros se corrio una submuestra de 17 individuos en mi computadora con la cual se presentan los análisis. 

Los parámetros modificados fueron:  el  11 que se refiere a la profundidad minima para llamar un heterocigoto, este dene ser igual al 12. Al modificar esto parametros se afectan los pasos s4 y s5. Al final de los 7 pasos (ver Script_ipyrad, para detalles), se obtuvo un archivo en formato vcf, que se utiza en análisis posteriores.


Con el vcf obtenido:

R version 3.4.4


Se utilizo con docker RaxML, con la siguiente instrucción:
docker pull cyverseuk/raxml


### Resultados

En la figura 1 se muestran los resultados del fastqc 

![](/home/usuario/Escritorio/Proyecto_Bioinfo_Jorge_Avance3/Imagenes/fastq_raw.jpg) 
**Fig 1.** Fastqc obtenido con datos crudos del género *Abies* en México.


### Discusión

Los análisis preliminares con fastqc en en general, mostraron buena calidad.


### Literatura citada

Aguirre-Planter E., Furnier G R. y Eguiarte L. E. 2000. Low levels of genetic variation within and high levels of genetic differentiation among populations of species of Abies from southern México and Guatemala. American Journal of Botany, 87: 362-371.


Aguirre-Planter, E., Jaramillo-Correa, J. P., Gómez-Acevedo, S., Khasa, D. P., Bousquet, J. & Eguiarte, L. (2012) Phylogeny, diversification rates and species boundaries of Mesoamerican firs (Abies, Pinaceae) in a genus-wide context. Molecular Phylogenetics and Evolution, 62:  263-274

Fitz-Gibbon, S., Hipp, A. L., Pham. K. K., Manos. P. S. & Sork, V. (2017). Phylogenomic inferences from reference-mapped and the novo assembled short-read sequence data using RADseq sequencing of California white oaks (Quercus section Quercus). Genome, 60: 743-755.


Massatti, R., Reznicek, A. A. & Knowles, L. L. (2016). Utilizing RADseq data for phylogenetic analysis of challenging taxonomic groups: A case study in Carex sect. Racemosae. American Journal of Botany. 103: 337-347

Schmidt-Lebuhn, A. N., Aitken, N. C. & Chuah, A. (2017). Species trees from consencus single nucleotide polymorphism (SNP) data: Testing phylogenetic approaches with simulated and empirical data. Molecular Phylogenetics and Evolution. 116: 192-201



