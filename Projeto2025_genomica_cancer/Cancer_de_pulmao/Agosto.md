# Grupo 3 - Câncer de Pulmão

## Introdução

## Metodologia
Fonte de dados: Os dados utilizados neste estudo foram obtidos a partir do [cBioPortal](https://www.cbioportal.org/) for Cancer Genomics. O conjunto selecionado foi o [Lung Squamous Cell Carcinoma (TCGA, Firehose Legacy)](https://www.cbioportal.org/study/summary?id=lusc_tcga), que reúne informações genômicas e clínicas de pacientes com carcinoma escamoso de pulmão. O gene SOX2 foi escolhido como foco da análise por apresentar a maior porcentagem de alterações no número de cópias (Copy Number Alterations - CNAs).

 Total de pacientes do estudo: 511 pacientes.

 ### Tipos de dados baixados:

Para a análise, foram baixados 3 principais tipos de estudos

* Dados clínicos: formato tsv.

* Dados de alterações no número de cópias (CNAs): formato txt → Registra variações no número de cópias gênicas, possibilitando identificar pacientes com amplificação de SOX2, alteração de maior frequência neste estudo.

* Dados de mutações: formato txt → Inclui mutações somáticas identificadas em pacientes, permitindo avaliar a presença ou ausência de alterações no gene SOX2.

### Ferramentas utilizadas: 

* Linguagem R (v4.4.1)

    * Bibliotecas:
        
        * tidyverse (v2.0.0)
        * dplyr (v1.1.4)
        * ggplot2 (v3.5.2)
     
### Resultados

Dados clínicos: 61 atributos.



    
