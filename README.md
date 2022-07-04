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

Porém, a quantificação de microRNAs é um processo caro, desempenhado através de a tecnologia *open array* . Tornando, assim, custoso um grande número de pacientes para os experimentos. Como alternativa à ausência de pacientes reais, esse trabalho vem propor a síntese de novos pacientes utilizando uma variação de [CTGAN](https://proceedings.neurips.cc/paper/2019/file/254ed7d2de3b23ab10936522dd547b78-Paper.pdf"CTGAN"), a CopulaGAN, para dados tabulares e mantendo as características da população e suas correlações.

CTGAN, e subsequentemente a CopulaGAN, é uma subcategoria de GAN (*Generative Adversal Network*) porém voltada para dados tabulares. GANs são redes neurais compostas por dois componentes, o gerador e o descriminador que são treinados simultaneamente. Dados tabulares, diferentemente de imagens e áudios, que são contínuas, podem conter dados de tipos variados (inteiros, decimais, categóricos), diferentes formas de distribuição (multimodal, gaussiano, não-gaussiano, etc.) e podem ser muito desbalanceados. Nesse contexto, é foi necessário a utilização de uma CTGAN, criando um modo específico para normalização para superar as distribuições não-gaussianas e multidimensional. O treinamento também é feito exemplo por exemplo, para que a CTGAN explore todos os possíveis valores de cada categoria.

Uma CTGAN pode ser entendida por:
![alt text](https://github.com/ryamaguti07/E3-MicroRNA-Syntesis-in-Patients-with-Atherosclerosis./blob/main/reports/images/ctgan.png)

A Copula definida pela Figura abaixo, pode ajudar a CTGAN a sintentizar dados novos em situações nas quais as distribuições marginais são mais importantes.

### Objetivo
Modelos generativos tem sido muito popular nos últimos anos nas áreas de geração de imagens, texto, áudio, séries temporais e dados tabulares. Nesse projeto será abordado o desenvolvimento de uma [CTGAN](https://proceedings.neurips.cc/paper/2019/file/254ed7d2de3b23ab10936522dd547b78-Paper.pdf "CTGAN") para geração de novos pacientes com Ateroscleroses e seus miRNAs com a finalidade de aumentar o número de exemplos para o estudo.

A partir desses novos dados, será investigado também a qualidade dos dados gerados, e como as correlações já observadas se comportam com a adição de dados artificias.

Por fim, será desenvolvido um preditor para determinar o tamanho da placa de gordura na artéria de cada paciente a partir de suas expressões de miRNA. A hipótese é que a partir dos pacientes artificiais seja possível fazer um *data augmentation* para ter resultados mais precisos.

# Metodologia
A metodologia desse trabalho conta com uma parceria com o laboratório cardiovascular da FCM, e os dados são coletados a partir de pessoas reais.
### Banco de dados
Os dados foram coletados de 178 pacientes, e foram analisadas 84 características (genéticas, fisiológicas e comportamentais). Abaixo a tabela com as variáveis a suas descrições

| Característica | Tipo | Descrição|
|--|--|--|
| HC | inteiro | número de identificação do paciente
|Status| Categórico | Grupo a qual o paciente pertence (1-sem placa, 2- com placa)| 
|Idade|Inteiro|Idade do Paciente
|Sexo|Categórico| Sexo do paciente (0-M, 1-H)
|PAS|Ponto Flutuante| Pressão arterial sistólica
|PAD|Ponto Flutuante| Pressão arterial diastólica
|FC|Ponto Flutuante| Frequência cardíaca
|Septo|Ponto Flutuante| Medida do septo cardíaco feito pela ultrassonografia 
|PP|Ponto Flutuante| Medida da parede posterior do coração 
|DDFVE|Ponto Flutuante| Diâmetro diastólico do ventrículo esquerdo
|DSFVE|Ponto Flutuante| Diâmetro sistólico do ventrículo esquerdo 
|Peso|Ponto Flutuante| Peso do paciente
|Altura|Ponto Flutuante| Altura do paciente
|ASC|Ponto Flutuante| Área de superfície corpórea
|IMC|Ponto Flutuante| Índice de massa corpórea
|MVE|Ponto Flutuante| Massa ventricular esquerda
|FE|Ponto Flutuante| Fração de Ejeção
|MVE17|Ponto Flutuante| Massa ventricular esquerda
|MVEAltura|Ponto Flutuante| Massa ventricular esquerda/altura
|MVEASC|Ponto Flutuante| -
|RWT|Ponto Flutuante| -
|Geometria| Categórico| Formato do remodelamento cardíaco  
|DM|Categórico| Paciente com diabetes melitus (1- com diabetes, 0- saudável)
|Glicose|Ponto Flutuante|  Quantidade de glicose no sangue
|CT|Ponto Flutuante| Colesterol total no sangue
|TG|Ponto Flutuante| Concentração de triglicérides no sangue 
|HDL|Ponto Flutuante| Concentração de HDL no sangue
|LDL|Ponto Flutuante| Concentração de LDL no sangue
|Ureia|Ponto Flutuante| Taxa de uréia no sangue
|Creatinina|Ponto Flutuante| Dosagem de creatina no sanguue
|aciduric|Ponto Flutuante| Taxa de ácido úrico no sangue
|statusAU| Categórico | Classificação relacionado ao ácido úrico  
|LDLox|Ponto Flutuante| Quantificação de LDL oxidado no sangue 
|mir185|Ponto Flutuante| Expressão do microRNA mir-185
|logmir185|Ponto Flutuante| Expressão do logarítmo do microRNA mir-185
|mirlet7|Ponto Flutuante| Expressão do microRNA mir-let7
|logmirlet7|Ponto Flutuante| Expressão do logarítmo do microRNA mir-let7
|mir30a|Ponto Flutuante|  Expressão do microRNA mir-30a
|logmir30a|Ponto Flutuante| Expressão do logarítmo do microRNA mir-30a
|mir451|Ponto Flutuante| Expressão do microRNA mir-451
|logmir451|Ponto Flutuante| Expressão do logarítmo do microRNA mir-451
|mir92a|Ponto Flutuante| Expressão do microRNA mir-92a
|logmir92a|Ponto Flutuante| Expressão do logarítmo do microRNA mir-92a
|mir145|Ponto Flutuante| Expressão do microRNA mir-145
|logmir145|Ponto Flutuante|Expressão do logarítmo do microRNA mir-145
|dsveimt|Ponto Flutuante| Medida da pressão sistólica na camada íntima média  
|ddveimt|Ponto Flutuante| Medida da pressão diastólica na camada íntima média
|IMT|Ponto Flutuante| Espessura da placa de gordura na artéria (mm) 
|StatusIMT|Categórico| Classificação do paciente de acordo com a espessura da placa de gordura (1- placa < 0.8mm, 2- placa >= 0.8mm )
|vari|Ponto Flutuante| Identificação da ultrasonografia
|Presenplaca|Categórico| Presença de placa nas artérias (1- presente, 0- ausente)
|Placadir|Ponto Flutuante|Medida da placa de gordura à direita
|Intdir|Ponto Flutuante| Medida da placa de gordura na camada íntima-direita
|Meddir|Ponto Flutuante| Medida da placa de gordura na camada media-direita
|AdvDir|Ponto Flutuante| Medida da placa de gordura na adventícia-direita
|PlcaEsq|Ponto Flutuante| Medida da placa de gordura à esquerda
|Intesq|Ponto Flutuante| Medida da placa de gordura na camada íntima-esquerda
|MedEsq|Ponto Flutuante| Medida da placa de gordura na camada media-esquerda
|AdvEsq|Ponto Flutuante| Medida da placa de gordura na adventícia-esquerda
|Intima|Ponto Flutuante| Medida da camada íntima (esquerda e direita)
|media|Ponto Flutuante| Medida da camada média (esquerda e direita)
|adventicia|Ponto Flutuante| Medida da camada adventícia (esquerda e direita)
|VAR00001|Ponto Flutuante| -
|Tabagismo|Categórico| Fumante (0-não, 1-fumante) 
|DAC|Categórico|Doença arterial coronariana(0-não possui, 1-possui) 
|IAM|Categórico| Paciente já sofreu um infarto agudo do miocárdio (0-não, 1-sim)
|AVC|Categórico| Paciente já sofreu um AVC (0-não, 1-sim)
|IF|Categórico| Paciente sofre de insuficiência cardíaca (0-não, 1-sim)
|Diuretico|Categórico|Paciente faz uso de remédios diuréticos  (0-não, 1-sim)
|IECA|Categórico| Paciente faz uso de remédios inibidores da enzima conversora de angiotensina (0-não, 1-sim)
|BRA|Categórico|Paciente faz uso de remédios bloqueadores dos receptores de angiotensina (0-não, 1-sim)
|IEBRA|Categórico|Paciente faz uso de remédios IECA OU BRA  (0-não, 1-sim)
|betabloq|Categórico|Paciente faz uso de remédios beta bloqueadores (anti-hipertensivo) (0-não, 1-sim)
|BCC|Categórico|Paciente faz uso de remédios bloqueadores de canal de cálcio (0-não, 1-sim)
|Medicação|Categórico|Paciente faz uso de remédios que não se enquadram  (0-não, 1-sim)
|Antaaldost|Categórico| Paciente faz uso de remédios antagonista de aldosterona (0-não, 1-sim)
|agoalfacentral|Categórico|Paciente faz uso de remédios alfa agonistas de ação central (0-não, 1-sim)
|agoalfaper|Categórico|Paciente faz uso de remédios agonistas alfa periféricos (0-não, 1-sim)
|vasodilat|Categórico|Paciente faz uso de remédios vasodilatadores (0-não, 1-sim)
|statinas|Categórico| Paciente faz uso de estatininas (0-não, 1-sim)
|AAS|Categórico| Paciente faz uso de anticoagulante (0-não, 1-sim)
|obesidade|Categórico| Obesidade (1-não, 2-sim)

A distribuição desses dados pode ser vista na Figura abaixo:
![alt text](https://github.com/ryamaguti07/E3-MicroRNA-Syntesis-in-Patients-with-Atherosclerosis./blob/main/reports/images/variables.png)
Como notado, nenhuma das variáveis tem distribuições guassiana. Com isso em vista, a utilização da CopulaGAN é adequada uma vez que a CTGAN tem dificuldade de aprender padrões e interdependência, como a medida da camada íntima-média e o Status da doença.


### Abordagem
Esse trabalho será abordado em quatro fases:

1. Coleta e ocultação/deleção dos dados sensíveis: todo dado que possa identificar uma pessoa é retirado deste trabalho;
2. Síntese de dados: Utilizar a abordagem de CopulaTGAN para gerar um conjunto de pacientes artificiais com as 84 características;
3. Validação: Utilizar cálculos já estabelecidos (IMC, por exemplo) e profissionais do laboratório cardiovascular da FCM para validar os dados gerados;
4. Classificação: Criação e treino de um classificador, uma com a adição dos dados sintéticos e outra sem, com o objetivo de obter uma acurácia maior com a adição de novos pacientes.

### Avaliação
Para a avaliar os dados gerados, vai ser utilizado além da validação descrita na subseção acima (cálculos já conhecidos, e validação de profissionais da área da saúde), vão ser comparados as métricas de classificação clássicas, como a Acurrácia e o F1-Score. A hipótese é que com a adição dos pacientes sintéticos provenientes da CopulaGAN, o classificador tenha um desempenho melhor.

### Ferramentas
As ferramentas que são utilizadas nesse trabalho são:

- [Google Colab](https://colab.research.google.com/ "Google Colab")
- [Pytorch](https://pytorch.org/ "Pytorch")
- [Scikit-learn] (https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression "Scikit-learn")
- [CopulaGAN](https://sdv.dev/SDV/user_guides/single_table/copulagan.html) (SDV/Syntentic Data Vault)
- [SPSS](https://www.ibm.com/br-pt/analytics/spss-statistics-software "SPSS")

# Resultados
O primeiro passo da metodologia, remover os dados sensíveis, foi feita utilizando o SPSS e não será demonstrado. Foi removido os nomes, datas de nascimento e foi gerado um novo identificador aleatório para esse trabalho.

Ao adaptar a CopulaGAN para a aplicação no contexto de miRNA, a pasta "reports" contém os primeiros pacientes gerados.

Foram gerados 50 novos pacientes, que possuem características reais, que poderiam fazer parte do grupo original (no caso o grupo de hipertensos) acertando, inclusive a variável obesidade sendo dependente do IMC. Apesar dos dados se parecerem com os reais, e o o resultado do teste Kolmogorov-Smirnov entre os dados reais e os dados sintéticos ser de 0.8579, alguns dados são estranhos, como pacientes com medida de placa íntima-média de 1cm classificado como sem placa nessa camada.

## Classificadores
Foram desenvolvidos dois tipos de classficadores: Uma rede neural densa e uma regressão logísticia. 
Para ambos os casos foi utilizado a base de dados para o treinamento e a adição de dados nunca vistos pela rede para os testes



| Tipo de rede | Adição de Dados Sintéticos | Acurácia | F1-Score |
|-|-|-|-|
| Rede Neural Densa | - | 60.0 % | 0.67 | 
| Rede Neural Densa | x | 63.04% | 0.75 |
| Regressão Logística | - | 56.66% | 0.72 |
| Regressão Logística | x | 80.43% | 0.89 |

Através dessa avaliação dos classificadores, pode-se dizer que a síntese de dados ajudou a melhorar o desempenho dos classificadores, aumentando significativamente a acurácia da Regressão logística. 

# Conclusão
A síntese de dados tem ganho muita popularidade devido a seus desempenhos nos campos de geração de imagem, texto e dados tabulares. Esse trabalho vem apresentar a utilização de uma CopulaGAN para gerar dados na área da saúde, em específico para a geração de pacientes com Aterosclerose no contexto de estudo da relação dos miRNA com a placa de gordura característica da doença.

O desenvolvimento desse projeto já conta com pacientes sintéticos gerados, porém os pacientes gerados apresentam problemas em sua geração (pacientes que não poderiam existir).

Notou-se que a natureza esparsa dos dados, bem como a falta de alguns dados tornou o trabalho desafiador. Porém ao validar os dados com profissinais da saúde e verificando a validade dos dados com o auxílio do teste KS, do aumento da acurrácia e do F-1 Score dos classificadores destinados a prever o status de aterosclerose nos pacientes, podemos afirmar que a síntese de Dados através de GANs tem potencial de ajudar a área da saúde em casos com poucos dados e que dependem de métodos caros.

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
- [Copula Flows for Synthetic Data Generation](https://arxiv.org/abs/2101.00598 "Copula Flows for Synthetic Data Generation")
- 
