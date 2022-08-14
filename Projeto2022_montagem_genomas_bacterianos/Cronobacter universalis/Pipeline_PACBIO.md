**Pipeline da montagem de genoma da Cronobacter universalis**


1. **Baixar as amostras**

Opcão: Manualmente pelo site do SRA

Endereço: https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&page_size=10&acc=SRR2154343&display=data-access 

Ir no terminal e baixar via wget:

    wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR2154343/SRR2154343
        

2. **Converter dado SRA para fastq**

    **1. Baixar SRAtoolkit**

    ```
    wget http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.4.1/sratoolkit.2.4.1-ubuntu64.tar.gz

    tar xzvf sratoolkit.2.4.1-ubuntu64.tar.gz
    ```

    **2. Converter SRA para fastq usando fastq-dump**

    ```
    ~/sratoolkit.2.4.1-ubuntu64/bin/fastq-dump --split-files SRR2154343 --outdir ~/Cronobacter/
    ```

3. **Análise de qualidade pelo fastqc**
    
    **1. Baixar fastqc**

    ```
    wget --no-check-certificate https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip

    unzip fastqc_v0.11.9.zip
    ```

    **2. Disponibilizar o comando fastqc**

    ```
    chmod u+x ~/FastQC/fastqc
    ```

    **3. Executar o comando fastqc**

    ```
    ~/FastQC/fastqc SRR2154343_1.fastq --outdir ~/Cronobacter/
    ```

    **4. Copiar arquivo html para o computador para tornar possível a abertura**

    Observação: Executar comando fora do servidor. 

    ```
    scp -r Rafaella@200.239.101.201:/home/Rafaella/Cronobacter/SRR2154343_1_fastqc.html ~/Downloads
    ```

    Comparar com o exemplo para PACBIO no próprio site do Babraham Bioinformatics:

    1 - https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

    2 - Example Reports

    3 - PacBio

3. **Análise pelo assembly-stats**
    1. **Converter fastq para fasta**
        
        Baixar ferramenta que converte
        ```
        git clone https://github.com/lh3/seqtk.git;
        cd seqtk; make
        ```
        Fazer a conversão:
        ```
        ./seqtk/seqtk seq -a SRR2154343_1.fastq > SRR2154343_1.fa
        ```

    2. **assembly_stats**
    
        Baixar a ferramenta
        ```
        pip install assembly-stats
        ```
        Executar a ferramenta
        ```
        assembly_stats SRR2154343_1.fa > SRR2154343_assembly_stats.txt
        ```
4. **Análise de qualidade com fastp**

    Referência: https://github.com/OpenGene/fastp
    
    Observações:
    - -i: input
    - -o: output apos trimagem
    - --dedup: calcular taxa de deduplicação
    - -q: phred de qualidade mínimo
    - --overrepresentation_analysis: calcular overrepresentação
    - -h: nome do arquivo html que será gerado como relatório
    
    ```
    fastp -i SRR2154343_1.fastq -o out.fastq --dedup -q 15 --overrepresentation_analysis -h fastp.html
    ```
