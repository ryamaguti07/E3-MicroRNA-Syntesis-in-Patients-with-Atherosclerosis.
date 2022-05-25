# `Síntese de MicroRNA em Pacientes com Aterosclerose`
# `MicroRNA Syntesis in Patients with Atherosclerosis`

# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação *IA376L - Deep Learning aplicado a Síntese de Sinais*,
oferecida no primeiro semestre de 2022, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).


|Nome  | RA | Especialização|
|--|--|--|
| Renan Yamaguti  | 262731  |Msc. Eng. de Computação|

# Introdução
Doenças cardiovasculares são uma das principais causas de óbito no mundo, dentre elas, destaca-se a Aterosclerose (ou Enrijecimento das artérias) com mais de 2 milhões de casos anuais no Brasil. A Aterosclerose é caracterizada pela a criação de uma placa de gordura na camada íntima média das artérias, causando obstrução do fluxo sanguíneo.

A Aterosclerose é uma doença inflamatória crônica, ou seja, o tratamento pode ajudar, mas é uma doença que não tem cura. Tradicionalmente, ela não apresenta sintomas até a haver a obstrução do fluxo sanguíneo ou quando a placa de colesterol se rompe. Em ambos casos, levando a paradas cardíacas e/ou AVCs.

As causas da criação dessa placa de colesterol podem ser:

- Sedentarismo
- Obesidade
- Pressão Alta
- Fatores Genéticos

Diferentemente dos outros fatores de risco, os fatores genéticos não podem ser alterados com mudança no estilo de vida. Um dos marcadores biológicos que tem sido alvo de pesquisas recentes são os microRNA (ou miRNA). Os microRNA são pequenos pedaços de RNA não codificante capazes de regular a expressão de gênica através da degradação ou repressão da tradução de moléculas-alvo de RNA mensageiro.

No laboratório de cardiologia da FCM (Faculdade de Ciências Médicas) da Unicamp, as pesquisas relacionadas que relacionam a Aterosclerose concentração-se na identificação de microRNAs e sua correlação com a placa de gordura característica da Aterosclerose.

Porém, a quantificação de microRNAs é um processo caro, desempenhado através de a tecnologia *open array* . Tornando, assim, custoso um grande número de pacientes para os experimentos. Como alternativa à ausência de pacientes reais, esse trabalho vem propor a síntese de novos pacientes utilizando uma  [CTGAN](https://proceedings.neurips.cc/paper/2019/file/254ed7d2de3b23ab10936522dd547b78-Paper.pdf"CTGAN") para dados tabulares e mantendo as características da população e suas correlações.

CTGAN é uma subcategoria de GAN (*Generative Adversal Network*) porém voltada para dados tabulares. GANs são redes neurais compostas por dois componentes, o gerador e o descriminador que são treinados simultaneamente. Dados tabulares, diferentemente de imagens e áudios, que são contínuas, podem conter dados de tipos variados (inteiros, decimais, categóricos), diferentes formas de distribuição (multimodal, gaussiano, não-gaussiano, etc.) e podem ser muito desbalanceados. Nesse contexto, é foi necessário a utilização de uma CTGAN, criando um modo específico para normalização para superar as distribuições não-gaussianas e multidimensional. O treinamento também é feito exemplo por exemplo, para que a CTGAN explore todos os possíveis valores de cada categoria.


### Objetivo
Modelos generativos tem sido muito popular nos últimos anos nas áreas de geração de imagens, texto, áudio, séries temporais e dados tabulares. Nesse projeto será abordado o desenvolvimento de uma [CTGAN](https://proceedings.neurips.cc/paper/2019/file/254ed7d2de3b23ab10936522dd547b78-Paper.pdf "CTGAN") para geração de novos pacientes com Ateroscleroses e seus miRNAs com a finalidade de aumentar o número de exemplos para o estudo.

A partir desses novos dados, será investigado também a qualidade dos dados gerados, e como as correlações já observadas se comportam com a adição de dados artificias.

Por fim, será desenvolvido um preditor para determinar o tamanho da placa de gordura na artéria de cada paciente a partir de suas expressões de miRNA. A hipótese é que a partir dos pacientes artificiais seja possível fazer um *data augmentation * para ter resultados mais precisos.

# Metodologia
A metodologia desse trabalho conta com uma parceria com o laboratório cardiovascular da FCM, e os dados são coletados a partir de pessoas reais.
### Banco de dados
Os dados foram coletados de 178 pacientes, e foram analisadas 84 características (genéticas, fisiológicas e comportamentais). Abaixo a tabela com as variáveis a suas descrições

| Característica | Tipo | Descrição|
|--|--|--|
| HC | inteiro | número de identificação do paciente
|Status| Categórico | Status 

### Abordagem
Esse trabalho será abordado em quatro fases:

1. Coleta e ocultação/deleção dos dados sensíveis: todo dado que possa identificar uma pessoa é retirado deste trabalho;
2. Síntese de dados: Utilizar a abordagem de CTGAN para gerar um conjunto de pacientes artificiais com as 84 características;
3. Validação: Utilizar cálculos já estabelecidos (IMC, por exemplo) e profissionais do laboratório cardiovascular da FCM para validar os dados gerados;
4. Regressão: Criação e treino de uma rede neural que faz regressão duas vezes, uma com a adição dos dados sintéticos e outra sem, com o objetivo de obter um erro menor com os novos pacientes.

### Avaliação
Para a avaliar os dados gerados, vai ser utilizado além da validação descrita na subseção acima (cálculos já conhecidos, e validação de profissionais da área da saúde), vão ser comparados as métricas de regressão clássicas, como RSME (*Root Square Mean Error*). A hipótese é que com a adição dos pacientes sintéticos provenientes da CTGAN, o classificador tenha um desempenho melhor.

### Ferramentas
As ferramentas que são utilizadas nesse trabalho são:

- [Google Colab](https://colab.research.google.com/ "Google Colab")
- [Pytorch](https://pytorch.org/ "Pytorch")
- [CTGAN](https://github.com/sdv-dev/CTGAN "CTGAN") (Implementação do Autor)
- [SPSS](https://www.ibm.com/br-pt/analytics/spss-statistics-software "SPSS")

# Resultados
O primeiro passo da metodologia, remover os dados sensíveis, foi feita utilizando o SPSS e não será demonstrado. Foi removido os nomes, datas de nascimento e foi gerado um novo identificador aleatório para esse trabalho.

Ao adaptar a CTGAN para a aplicação no contexto de miRNA, a pasta "reports" contém os primeiros pacientes gerados.

Foram gerados 15 novos pacientes, apesar dos dados gerados estarem com os mesmos tipos de dados que os originais, alguns pacientes não poderiam existir, como os exemplos: um paciente com concentração de ureia no sangue de 57mg/dL, um paciente com IMC errado (sua altura e seu peso não batem com o IMC).

# Conclusão
A síntese de dados tem ganho muita popularidade devido a seus desempenhos nos campos de geração de imagem, texto e dados tabulares. Esse trabalho vem apresentar a utilização de uma CTGAN para gerar dados na área da saúde, em específico para a geração de pacientes com Aterosclerose no contexto de estudo da relação dos miRNA com a placa de gordura característica da doença.

O desenvolvimento desse projeto já conta com pacientes sintéticos gerados, porém os pacientes gerados apresentam problemas em sua geração (pacientes que não poderiam existir).

Nas próximas semanas, a geração desses novos dados será otimizada e será criado um preditor para determinar, utilizando os miRNA, a espessura da placa de gordura. Esse preditor será treinado com os dados originais e depois com a adição dos dados sintéticos, para atingir uma maior precisão.

# Cronograma
| Atividade |27/04|04/05|11/05|18/05|25/05|02/06|09/06|16/06|23/06|30/06
|-|-|-|-|-|-|-|-|-|-|-
| Revisão Bibliográfica |x|x|x|x||||||
| Coleta e ocultação/deleção dos dados sensíveis||x|x|||||||
| Síntese de dados||||x|x|x||||
| Validação|||||x|x|x|||
| Regressão||||||x|x|x||
| Elaboração do relatório final e Apresentação||||||||x|x|x

# Referências Bibliográficas
-  [Framingham contribution to cardiovascular disease](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4966216/ "Framingham contribution to cardiovascular disease")
- [Immunity, atherosclerosis and cardiovascular disease](https://bmcmedicine.biomedcentral.com/articles/10.1186/1741-7015-11-117 "Immunity, atherosclerosis and cardiovascular disease")
- [An overview of microRNAs: biology, functions, therapeutics, and analysis methods](https://onlinelibrary.wiley.com/doi/abs/10.1002/jcp.27486 "An overview of microRNAs: biology, functions, therapeutics, and analysis methods")
- [Association of Circulating miR-145-5p and miR-let7c and Atherosclerotic Plaques in Hypertensive Patients](https://www.mdpi.com/2218-273X/11/12/1840/htm "Association of Circulating miR-145-5p and miR-let7c and Atherosclerotic Plaques in Hypertensive Patients")
- [Generative adversarial networks: An overview](https://arxiv.org/pdf/1710.07035.pdf "Generative adversarial networks: An overview")
- [Generative Adversarial Networks](https://arxiv.org/abs/1406.2661 "Generative Adversarial Networks")
- [ Spectral Normalization for Generative Adversarial Networks](https://arxiv.org/abs/1802.05957 " Spectral Normalization for Generative Adversarial Networks")
- [Self-Attention Generative Adversarial Networks](https://arxiv.org/abs/1805.08318 "Self-Attention Generative Adversarial Networks")
- [Tabular GANs for uneven distribution](https://arxiv.org/pdf/2010.00638.pdf "Tabular GANs for uneven distribution")
- [Modeling Tabular Data using Conditional GAN](https://proceedings.neurips.cc/paper/2019/file/254ed7d2de3b23ab10936522dd547b78-Paper.pdf "Modeling Tabular Data using Conditional GAN")
- [Conditional Wasserstein GAN-based Oversampling of Tabular Data for Imbalanced Learning](https://arxiv.org/pdf/2008.09202.pdf "Conditional Wasserstein GAN-based Oversampling of Tabular Data for Imbalanced Learning")
- [Synthesizing Tabular Data using Conditional GAN](https://dspace.mit.edu/bitstream/handle/1721.1/128349/1202001437-MIT.pdf?sequence=1&isAllowed=y "Synthesizing Tabular Data using Conditional GAN")
- [OCT-GAN: Neural ODE-based Conditional Tabular GANs](https://arxiv.org/pdf/2105.14969.pdf "OCT-GAN: Neural ODE-based Conditional Tabular GANs")
