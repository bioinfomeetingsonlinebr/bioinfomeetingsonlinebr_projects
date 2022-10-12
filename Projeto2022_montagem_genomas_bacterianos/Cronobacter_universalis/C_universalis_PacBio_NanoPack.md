# Análise de dados brutos de _Cronobacter_ _universalli_ por NanoPack
Descrição com os códigos utilizados na linha de comando utilizada para analisar o SRA de de _Cronobacter_ _universalli_. Utilizando como referência de Moine et al, encontramos o SRA (SRR2154343) que adotasse a tecnologia de sequenciamento de leituras longas PacBio, esta leitura faz parte do BioProject (PRJNA291037) que sequenciou e realizou montagem de genomas de seis espécies do gênero Cronobacter, incluido _Cronobacter_ universalli_. Ambos os dados de acesso estão localizados também no NCBI, eles podem ser baixados manualmente, atraves de linhas de comando e com ferramentas que servem de auxilio.

### Baixando o SRA de _Cronobacter_ _universalli_
Para baixar o SRA, utilizamos uma ferramenta conhecida como 'ffq', ela retorna um arquivo em .json, através dele é possível conseguir o link onde baixamos pelo comando 'wget'. Ela tem como principal objetivo encontrar de forma mais fácil os links. Para baixar essa ferramenta na linha de comando:
```
pip install ffq
```
Uma vez baixado, é necessário informarmos o identificador de acesso (SRR2154343), assim recuperamos o link FTP que contém as leituras em formato fastq.
```
ffq SRR2154343
```
```
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR215/003/SRR2154343/SRR2154343_subreads.fastq.gz
```
### Baixando o pacote NanoPack
O NanoPack é um pacote de scripts em Pyhton, ele contém diversas ferramentas que auxiliam na vizualização da qualidade de dados brutos de leituras longas, também é possível fazer o processamento desses dados, porém nós o utilizamos apenas para análise de qualidade utilizando `NanoPlot` que cria gráficos de leitura fastq,`NanoStat` que cria um resumo estatístico das leituras e `nanoQC` que gera gráficos para a análise dos nucleotídeos e a distribuição da qualidade das leituras. Para baixar esse pacote na linha de comando:
```
pip install NanoPack
```
Assim que o pacote foi baixado nós realizamos as análises com os seguintes comandos citados anteriormente:
### NanoPlot
```
NanoPlot --verbose --N50 --fastq SRR2154343_subreads.fastq -o nanoplot
```
### NanoStat
```
NanoStat --fastq SRR2154343_subreads.fastq -n nanostat
```
### NanoQC
```
nanoQC SRR2154343_subreads.fastq -o nanoQC
```
### Referências
Wouter De Coster, Svenn D'Hert, Darrin T Schultz, Marc Cruts, Christine Van Broeckhoven, NanoPack: visualização e processamento de dados de sequenciamento de leitura longa, Bioinformática , Volume 34, Edição 15, 01 de agosto de 2018, Páginas 2666–2669, 
<a href="https://doi.org/10.1093/bioinformatics/bty149" class="uri">https://doi.org/10.1093/bioinformatics/bty149</a>

Moine, D., Kassam, M., Baert, L., Tang, Y., Barretto, C., Ngom Bru, C., Klijn, A., & Descombes, P. (2016). Fully Closed Genome Sequences of Five Type Strains of the Genus Cronobacter and One Cronobacter sakazakii Strain. Genome announcements, 4(2), e00142-16. <a href="https://doi.org/10.1128/genomeA.00142-16" class="uri">https://doi.org/10.1128/genomeA.00142-16</a>

Gálvez-Merchán, A., Kyung, M., Pachter, L., Booeshaghi, A. bioRxiv 2022.05.18.492548; doi: <a href="https://doi.org/10.1101/2022.05.18.492548" class="uri">https://doi.org/10.1101/2022.05.18.492548</a>


