# Grupo 3 - Câncer de Pulmão (Fastamantes)

![R](https://img.shields.io/badge/RStudio-4.4.1-276DC3?logo=r&logoColor=white)
![Status](https://img.shields.io/badge/status-concluído-brightgreen)

**Agosto de 2025**

## Introdução

### Gene escolhido - SOX2

_SRY-box transcription factor 2_

Foi escolhido devido a maior frequência de alteração no número de cópias do tipo amplificação

- Papel de reprogramação de células somáticas em células-tronco pluripotentes induzidas;
- É expresso muito cedo na fase de desenvolvimento embrionário;
- Papel na emergência celular pluripotente na fase embrionária ⟶ ausência = letalidade do embrião;
- Ao longo do desenvolvimento fetal impacta na diferenciação das células do Sistema Nervoso Central e Periférico.

A desregulação da expressão do gene contribui para o desenvolvimento do câncer, pois o gene controla:

- Proliferação de células;

- Transição epitélio-mesenquimal;

- Migração;

- Invasão;

- Metástase;

- Formação de esfera e colônia;

- Iniciação do tumor;

- Formação de células tronco cancerígenas;

- Resistência a apoptose e terapias.

## Metodologia

### 1. Fonte de dados

Os dados utilizados neste estudo foram obtidos a partir do [cBioPortal](https://www.cbioportal.org/) for Cancer Genomics.

A imagem abaixo ilustra a visão geral da plataforma:

<p align="center">
  <img src="https://i.postimg.cc/vZ3RnFdp/visao-geral-do-c-Bio-Portal.png" width="600">
</p>

O conjunto selecionado foi o [Lung Squamous Cell Carcinoma (TCGA, Firehose Legacy)](https://www.cbioportal.org/study/summary?id=lusc_tcga), que reúne informações genômicas e clínicas de pacientes com carcinoma escamoso de pulmão. O gene SOX2 foi escolhido como foco da análise por apresentar a maior porcentagem de alterações no número de cópias (Copy Number Alterations - CNAs).

### 2. Tipos de dados utilizados

Para a realização da análise, foram utilizados três principais tipos de dados, obtidos a partir do cBioPortal:

**(1) Dados clínicos**

Formato: `.tsv`

Contêm informações demográficas, clínicas e de sobrevida dos pacientes.

**(2) Dados de alterações no número de cópias (CNAs)**

Formato: `.txt`

Registram variações no número de cópias gênicas, possibilitando a identificação de pacientes com amplificação do gene SOX2, a alteração mais frequente observada neste estudo.

**(3) Dados de mutações somáticas (SNVs)**

Formato: `.txt`

Incluem mutações somáticas identificadas nos pacientes, permitindo avaliar a presença ou ausência de alterações no gene SOX2.

### 3. Ferramentas utilizadas

**Linguagem de programação**:

- R (versão 4.4.1)

**Pacotes principais**:

- tidyverse (v2.0.0)

- dplyr (v1.1.4)

- ggplot2 (v3.5.2)

- survival

- survminer

#### 3.1 Instalação dos pacotes necessários

```
# Instalação dos pacotes

if (!requireNamespace("tidyverse", quietly = TRUE)) {
  install.packages("tidyverse")
}
if (!requireNamespace("survival", quietly = TRUE)) {
  install.packages("survival")
}
if (!requireNamespace("survminer", quietly = TRUE)) {
  install.packages("survminer")
}

library(tidyverse)
library(survival)
library(survminer)


```

#### 3.2 Importação dos dados

Importação dos 3 conjuntos de dados:

```
mut <- read_tsv("mutations_lung.txt", comment = "#")
cna <- read_tsv("cna_lung.txt")
clin <- read_tsv("lusc_luad_clinical_data.tsv", comment = "#")
```

#### 3.3 Processamento dos dados

Visualização inicial das colunas presentes nos dados clínicos:

```
# Visualização inicial

colnames(clin)
```

[![colunas-dclinico.png](https://i.postimg.cc/DyzpNCJq/colunas-dclinico.png)](https://postimg.cc/RqjT6LTF)

Resumo dos dados importados:

- Mutações: 1.344 registros
- CNAs: 1.344 registros
- Dados clínicos: 500 pacientes, 39 variáveis

Para compreender a natureza das alterações genéticas presentes no gene TBL1XR1, foi realizada uma exploração inicial dos dados de mutações e de alterações no número de cópias gênicas (CNAs). Nesse contexto, os comandos unique(mut$TBL1XR1) e unique(cna$TBL1XR1) foram utilizados com o objetivo de identificar todos os valores distintos registrados para o gene em cada um dos conjuntos de dados.

`unique(mut$TBL1XR1)`

O comando unique(mut$TBL1XR1) permite listar os diferentes tipos de mutações observadas no gene TBL1XR1 entre os pacientes, excluindo valores repetidos. Dessa forma, é possível identificar quais variantes genéticas estão presentes.

Essa verificação revelou a presença de amostras classificadas como _wild-type (WT)_, registros com dados não disponíveis (NP) e um conjunto de mutações somáticas pontuais, incluindo substituições de aminoácidos e eventos de splicing. Entre as mutações identificadas, observaram-se alterações como A48S, D85H, H33R, Q264L, L387S, D232H, S422C, N353H, P452L, K276R, além de mutações múltiplas em uma mesma amostra (por exemplo, K374N/S202I e G505V/G505S) e um evento de splice-site (X256_splice).

`unique(cna$TBL1XR1)`

Com relação as alterações no número de cópias, os valores identificados correspondem às diferentes categorias de estado de cópia gênica, incluindo deleção (–1), estado neutro (0), ganho de cópia (1), amplificação gênica (2), além de registros com dados não disponíveis (NP).

## Resultados

## 📚 Referências

NOVAK, D. _et al._ **SOX2 in development and cancer biology**._Seminars in Cancer Biology_, v. 67, p. 74–82, 2020.  
DOI: https://doi.org/10.1016/j.semcancer.2019.08.007.

DU, R. _et al._ **Two decades of a protooncogene TBL1XR1: from a transcription modulator to cancer therapeutic target**._Frontiers in Oncology_, v. 14, p. 1309687, 2024.
