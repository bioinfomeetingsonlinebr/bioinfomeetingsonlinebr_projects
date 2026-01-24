# Grupo 3 - Câncer de Pulmão (Fastamantes)

![R](https://img.shields.io/badge/RStudio-4.4.1-276DC3?logo=r&logoColor=white)
![Status](https://img.shields.io/badge/status-concluído-brightgreen)

**Agosto de 2025**

## 1. Introdução

### 1.1 Gene escolhido - SOX2

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

## 2. Metodologia

### 2.1 Fonte de dados

Os dados utilizados neste estudo foram obtidos a partir do [cBioPortal](https://www.cbioportal.org/) for Cancer Genomics.

A imagem abaixo ilustra a visão geral da plataforma:

<p align="center">
  <img src="https://i.postimg.cc/vZ3RnFdp/visao-geral-do-c-Bio-Portal.png" width="600">
</p>

O conjunto selecionado foi o [Lung Squamous Cell Carcinoma (TCGA, Firehose Legacy)](https://www.cbioportal.org/study/summary?id=lusc_tcga), que reúne informações genômicas e clínicas de pacientes com carcinoma escamoso de pulmão. O gene SOX2 foi escolhido como foco da análise por apresentar a maior porcentagem de alterações no número de cópias (Copy Number Alterations - CNAs).

### 2.2 Tipos de dados utilizados

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

### 2.3 Ferramentas utilizadas

**Linguagem de programação**:

- R (versão 4.4.1)

**Pacotes principais**:

- tidyverse (v2.0.0)

- dplyr (v1.1.4)

- ggplot2 (v3.5.2)

- survival

- survminer

#### 2.3.1 Instalação dos pacotes necessários

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

#### 2.3.2 Importação dos dados

Importação dos 3 conjuntos de dados:

```
mut <- read_tsv("mutations_lung.txt", comment = "#")
cna <- read_tsv("cna_lung.txt")
clin <- read_tsv("lusc_luad_clinical_data.tsv", comment = "#")
```

#### 2.3.4 Processamento dos dados

**Visualização inicial das colunas presentes nos dados clínicos**:

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

O comando unique(mut$TBL1XR1) permite listar os diferentes tipos de mutações observadas no gene TBL1XR1 entre os pacientes, excluindo valores repetidos. Dessa forma, é possível identificar quais variantes genéticas estão presentes.

`unique(mut$TBL1XR1)`

[![mutacoes.png](https://i.postimg.cc/YCH5sN7k/mutacoes.png)](https://postimg.cc/KR0pKM4s)

Essa verificação revelou a presença de amostras classificadas como _wild-type (WT)_, registros com dados não disponíveis (NP) e um conjunto de mutações somáticas pontuais, incluindo substituições de aminoácidos e eventos de splicing. Entre as mutações identificadas, observaram-se alterações como A48S, D85H, H33R, Q264L, L387S, D232H, S422C, N353H, P452L, K276R, além de mutações múltiplas em uma mesma amostra (por exemplo, K374N/S202I e G505V/G505S) e um evento de splice-site (X256_splice).

`unique(cna$TBL1XR1)`

[![cna.png](https://i.postimg.cc/bNCn6gk1/cna.png)](https://postimg.cc/qzKgq8m7)

Com relação as alterações no número de cópias, os valores identificados correspondem às diferentes categorias de estado de cópia gênica, incluindo deleção (–1), estado neutro (0), ganho de cópia (1), amplificação gênica (2), além de registros com dados não disponíveis (NP).

#### 2.3.5 Seleção de pacientes com alterações no gene

```
com_amplificacao <- cna %>%
  filter(TBL1XR1 == 2) %>%
  mutate(mutation_type = "AMPLIFICACAO") %>%
  select(SAMPLE_ID, mutation_type)

mutados <- mut %>%
  filter(!(TBL1XR1 %in% c("WT", "NP"))) %>%
  mutate(mutation_type = "MUTACAO") %>%
  select(SAMPLE_ID, mutation_type)

dataset_mut <- bind_rows(com_amplificacao, mutados) %>%
  distinct(SAMPLE_ID, .keep_all = TRUE)

df_lung <- dataset_mut %>%
  inner_join(clin, by = c("SAMPLE_ID" = "sampleId"))

```

## 3. Resultados

### 3.1 Distribuição de pacientes por sexo

```
{r}
table(clin$SEX, useNA = "no")

clin %>%
  filter(!is.na(SEX)) %>%
  ggplot(aes(x = SEX, fill = SEX)) +
  geom_bar() +
  scale_fill_manual(values = c("Male" = "cyan4", "Female" = "darkseagreen")) +
  labs(
    title = "Distribuição de pacientes por sexo",
    x = "Sexo",
    y = "Número de pacientes",
    fill = "Sexo"
  ) +
  theme_minimal()

```

[![Rplot.png](https://i.postimg.cc/gJywWq5y/Rplot.png)](https://postimg.cc/MMTZ0RzX)

Conforme ilustrado no gráfico de distribuição por sexo, a coorte apresenta uma distribuição relativamente equilibrada entre os sexos, com discreto predomínio de pacientes do sexo masculino (240) em relação ao sexo feminino (210).

### 3.2 Status vital dos pacientes

```
ggplot(clin %>% filter(!is.na(OS_STATUS)), aes(x = OS_STATUS, fill = OS_STATUS)) +
  geom_bar() +
  scale_fill_manual(values = c("1:DECEASED" = "firebrick", "0:LIVING" = "darkseagreen3")) +
  labs(
    title = "Status Vital dos Pacientes",
    x = "Status",
    y = "Número de Pacientes",
    fill = "Status"
  ) +
  theme_minimal()
```

[![Status-vital.png](https://i.postimg.cc/gkLxcW15/Status-vital.png)](https://postimg.cc/5614pZ4q)

### 3.3 Idade ao diagnóstico de câncer de pulmão

Com o objetivo de caracterizar o perfil clínico da coorte estudada, foram realizadas análises estatísticas descritivas das variáveis idade ao diagnóstico (AGE) e tempo de sobrevida global em meses (OS_MONTHS). Essas análises permitem compreender a distribuição central e a dispersão dos dados clínicos, fornecendo uma visão geral da população avaliada

`summary(clin$AGE)`

`summary(clin$OS_MONTHS)`

Para investigar a relação entre a idade ao diagnóstico e o tipo de alteração genética, foi construída uma visualização do tipo boxplot, comparando os grupos definidos como amplificação gênica e mutações do tipo SNVs. O gráfico foi gerado utilizando a biblioteca ggplot2, considerando a variável categórica mutation_type no eixo x e a variável contínua AGE no eixo y.

```
ggplot(df_lung, aes(x = mutation_type, y = AGE)) +
  geom_boxplot(fill = "skyblue") +
  labs(
    title = "Box-plot: Idade ao diagnóstico por mutação",
    x = "Tipo de mutação (Amplificação vs SNVs)",
    y = "Idade ao diagnóstico"
  ) +
  theme_minimal()

```

[![Box-plot.png](https://i.postimg.cc/L8qw0yYV/Box-plot.png)](https://postimg.cc/JsLps5Qy)

Conforme ilustrado no boxplot da idade ao diagnóstico estratificado por tipo de mutação, observa-se que pacientes com mutações do tipo SNVs apresentam uma mediana de idade ligeiramente superior em comparação aos pacientes com amplificação gênica. O grupo com amplificação apresenta maior dispersão dos valores de idade, indicando maior heterogeneidade etária no momento do diagnóstico, enquanto o grupo com mutações SNVs apresenta distribuição mais concentrada em torno da mediana.

### 3.4 Análise de sobrevida

Inicialmente, foi criada uma variável binária para representar o evento de interesse na análise de sobrevida. Pacientes com status vital registrado como óbito foram codificados como evento (1), enquanto os demais foram considerados censurados (0). Em seguida, foi construído o objeto de sobrevivência combinando o tempo de acompanhamento em meses com a ocorrência do evento.

As curvas de sobrevida global foram então estimadas pelo método de Kaplan–Meier, estratificando os pacientes de acordo com o tipo de alteração no gene TBL1XR1 (amplificação ou mutação do tipo SNV). Essa abordagem permitiu comparar o comportamento da sobrevida entre os grupos ao longo do tempo.

Por fim, as curvas foram representadas graficamente, incluindo os intervalos de confiança, a tabela do número de pacientes em risco e o valor de p do teste de log-rank, utilizado para avaliar possíveis diferenças estatísticas entre os grupos.

```
df_lung <- df_lung %>%
  mutate(event = ifelse(OS_STATUS == "DECEASED", 1, 0))

surv_obj <- Surv(time = df_lung$OS_MONTHS, event = df_lung$event)

fit <- survfit(surv_obj ~ mutation_type, data = df_lung)

ggsurvplot(
  fit,
  data = df_lung,
  pval = TRUE,
  risk.table = TRUE,
  risk.table.y.text.col = TRUE,
  risk.table.y.text = FALSE,
  title = "TBL1XR1 e análise de sobrevida",
  legend.title = "Tipo de mutação",
  legend.labs = c("Amplificação", "SNV"),
  palette = c("firebrick", "steelblue"),
  font.main = c(12, "bold"),
  font.x = c(9), font.y = c(9),
  font.tickslab = c(9),
  xlab = "Meses",
  ylab = "Probabilidade de Sobrevivência",
  conf.int = TRUE,
  ggtheme = theme_minimal(base_size = 10)
)

```

[![Sobrevida.png](https://i.postimg.cc/rsrRH2kZ/Sobrevida.png)](https://postimg.cc/XBnvZhX9)

Com o número reduzido de pacientes e eventos, não foi possível detectar diferenças estatisticamente significativas na sobrevida global associadas ao tipo de mutação do TBL1XR1. As curvas de sobrevivência apresentaram comportamento semelhante ao longo do tempo, com elevado número de observações censuradas. É importante destacar o número reduzido de amostras utilizadas na geração do gráfico. Embora o conjunto de dados original contenha um número maior de pacientes, apenas uma fração apresenta amplificação gênica ou mutações somáticas nesse gene, limitando o tamanho dos grupos analisados.

## 📚 Referências

NOVAK, D. _et al._ **SOX2 in development and cancer biology**._Seminars in Cancer Biology_, v. 67, p. 74–82, 2020.  
DOI: https://doi.org/10.1016/j.semcancer.2019.08.007.

DU, R. _et al._ **Two decades of a protooncogene TBL1XR1: from a transcription modulator to cancer therapeutic target**._Frontiers in Oncology_, v. 14, p. 1309687, 2024.
