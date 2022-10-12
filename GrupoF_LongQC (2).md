### Grupo F 
## Análise de Qualidade de Sequenciamentos de Terceira Geração Através do LongQC

O LongQC é uma ferramenta de controle de qualidade computacionalmente eficiente para identificar problemas antes de uma análise completa. Leituras longas de sequenciadores de terceira geração têm alta taxa de erro (~15%).

- Eficiente no uso de recuros;
- Não requer genoma de referência;
- FastQC e PRINSEQ não são totalmente otimizados para as long reads geradas no TGS;
- Base-level quality score pode apresentar grandes variações.

#### _Dessa forma, mapear leituras de volta para referências supervisionando o perfil de erro é uma alternativa interessante para o controle de qualidade de Sequenciamentos TGS._

## Instalação

- É recomendado que se use o Docker para rodar o LongQC, dessa forma, foi feito o download da plataforma pelo link https://www.docker.com/get-started/

- Garantiu-se que o sistema estaria rodando WSL 2 através das instruções em https://docs.microsoft.com/pt-br/windows/wsl/install-manual#step-3---enable-virtual-machine-feature

- Também foi feita a instação do Ubuntu 22.04 seguindo as instruções em https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-10#1-overview

- Então, seguindo os passos para usar a imagem docker do LongQC presentes em https://github.com/yfukasawa/LongQC

## Análise de Dados

- Foram baixados os dados de Cronobacter universalis em: https://trace.ncbi.nlm.nih.gov/Traces/index.html?view=run_browser&acc=SRR2154343&display=download. Baixar preferencialmente em formtato FASTQ, o padrão para análises no LongQC.
- Os comandos utilizados no terminal Ubuntu 22.04.1 LTS foram:
- '> wget https://raw.githubusercontent.com/yfukasawa/LongQC/master/Dockerfile'

- '> docker build -t longqc .''

- '> mkdir downloads'

- '> mv C_Universalis_PacBio.fastq downloads'

- '> docker run -it -v /home/afonso/downloads:/input -v /home/afonso/downloads:/output longqc sampleqc -x pb-sequel -o /output/out_crono /input/C_Universalis_PacBio.fastq'

## A síntese dos resultados obtidos está no arquivo "Web_Summary"