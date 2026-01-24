# Grupo 3 - Câncer de Pulmão (Fastamantes)

![RStudio](https://img.shields.io/badge/RStudio-package)

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

Total de pacientes do estudo: 511 pacientes.

## 📚 Referências

NOVAK, D. _et al._ **SOX2 in development and cancer biology**._Seminars in Cancer Biology_, v. 67, p. 74–82, 2020.  
DOI: https://doi.org/10.1016/j.semcancer.2019.08.007.

DU, R. _et al._ **Two decades of a protooncogene TBL1XR1: from a transcription modulator to cancer therapeutic target**._Frontiers in Oncology_, v. 14, p. 1309687, 2024.
