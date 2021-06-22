# Desafio Final - Trilha Engenheiro IA Microsoft
Repo contendo relatório para os problemas apresentados no Desafio Final - Trilha Engenheiro IA Microsoft, que envolvem Machine Learning e Inteligência Artificial aplicados ao contexto da educação. O Desafio Final, faz parte da iniciativa de aprendizagem de IA, ofertada pelo grupo Ãnima em parceria com a Microsoft, para os estudantes das universidades do grupo, no primeiro semestre de 2021.

Utilizei o Microsoft Learn p/ apoio ao desenvolvimento deste estudo.
Link p/ orientações: https://docs.microsoft.com/pt-br/learn/modules/create-regression-model-azure-machine-learning-designer/

###  1. Autor
Raphael Feldberg

##  2. Problemas propostos

Os problemas propostos no Desafio Final se encontram descritos no github: https://github.com/flaviocalado-anima/ms-challenge-2021

Para este Desafio Final, ou Hackathon, escolhi realizar o Desafio 1.

## 3. Desafio 1 (3 pontos) Existe alguma correlação entre as notas das provas objetivas? Comente.

### Tarefa
Quem é bom em matemática, também vai bem em ciências da natureza? Nesse problema, você selecionará os campos da prova objetiva:

NU_NOTA_CN Nota da prova de Ciências da Natureza NU_NOTA_CH Nota da prova de Ciências Humanas NU_NOTA_LC Nota da prova de Linguagens e Códigos NU_NOTA_MT Nota da prova de Matemática

Você deverá realizar uma análise de regressão para descobrir se é possível prever a nota da prova de Ciências da Natureza caso se saiba a nota de outra.

- Resultado
Você deverá mostrar a previsão de uma nota de Ciências da Natureza com base em uma nota da prova de Matemática

## 4. Dataset utilizado:
Conjunto de dados contendo campos referentes ao Enem 2019. Utilizaremos apenas os dados referentes as nota das provas citadas acima.
Link para download do dataset completo: https://download.inep.gov.br/microdados/microdados_enem_2019.zip

### 5. PASSO 1: Criar um Workspace do Azure Machine Learning
- Nome do workspace escolhido: desafios-ml-sj
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/1-grupo-de-recrusos.png)

Após criado o Workspace, clique em Iniciar o estudio. Utilizaremos o Estúdio do Azure Machine Learning p/ compilar, treinar, avaliar e implantar modelos de machine learning.

### 6. PASSO 2: Criar recursos de computação
Para treinar e implantar modelos usando o designer do Azure Machine Learning, é necessário usar a computação para executar o processo de treinamento e testar o modelo treinado após a implantação.

####  6.1. Criando uma instancia de computação
Instância de computação: estações de trabalho de desenvolvimento que os cientistas de dados podem usar para trabalhar com modelos e dados.
Foi criada a instância de computação com as seguntes configurações:
- Tipo de máquina virtual: CPU
- Tamanho da máquina virtual: Standard_DS11_v2 (escolha Selecionar entre todas as opções para pesquisar por este tamanho de máquina e escolhê-lo)
- Nome de computação: insira um nome exclusivo
- Habilitar o acesso SSH: não selecionado
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/2-instancia-computacao.png)

####  6.2. Criar um cluster de computação
Você o usará para treinar um modelo de machine learning.
Foi criado um cluster de computação com as seguntes configurações:

- Prioridade da máquina virtual: dedicada
- Tipo de máquina virtual: CPU
- Tamanho da máquina virtual: Standard_DS11_v2 (escolha Selecionar entre todas as opções para pesquisar por este tamanho de máquina e escolhê-lo)
- Nome de computação: insira um nome exclusivo
- Número mínimo de nós: 0
- Número máximo de nós: 2
- Segundos de espera antes de reduzir verticalmente: 120
- Habilitar o acesso SSH: não selecionado
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/3-cluster-computacao.png)

### 7. PASSO 3: Explorar dados
#### 7.1. Criar um pipeline
Um pipeline será usado para treinar um modelo de machine learning.

#### 7.2. Adicionar e explorar um conjunto de dados
Devido a quantidade de dados que o Dataset original contém, cerca de 2.5Gb de dados, sendo 10k linhas e 136 colunas, ao executar meu pipeline, obtive falha, pela quantidade de dados.
Decidi criar um novo Dataset, utilizando apenas 3 colunas, limitando assim as variáveis, sendo: 
NU_INSCRICAO - Nº de ordem, para contagem das provas.
NU_NOTA_CN - Notas das provas de Ciências Naturais
NU_NOTA_MT - Notas das provas de Matemática
Após criado, salvei o arquivo.csv

Para criar um conjunto de dados no Azure ML, utilizei esse tutorial, como exemplo: https://docs.microsoft.com/pt-br/learn/modules/use-automated-machine-learning/data
Depois de criado, segue imagem da Visualização dos "dados brutos". Repare que contém alguns registros faltantes.
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/4-dataset-view.png)

No painel à esquerda, do Estudio, expanda a seção Datasets e arraste o módulo "Micro Dados Enem 19" para a tela.

#### 7.3. Adicionar transformações de dados
Normalmente, você aplica transformações de dados para preparar os dados para modelagem.

1 - No painel à esquerda, expanda a seção "Transformação de Dados" e arraste um módulo **Selecionar Colunas no Conjunto de Dados** para a tela, abaixo do módulo "Micro Dados Enem 19" e conecte a saída deste último na entrada do módulo de seleção de colunas.
2 - Neste módulo de seleção, edite as colunas para selecionar as colunas com nomes NU_NOTA_CN e NU_NOTA_MT.
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/5-select-cols.png)

Agora é a hora de limpar os dados, pois temos registros faltantes, conforte citado acima.
1 - Arraste um módulo **Limpar Dados Ausentes** da seção "Transformações de Dados" e coloque-o no módulo "Selecionar Colunas no Conjunto de Dados".
2 - Selecione este módulo e edite as colunas. 
3 - Na janela de colunas, selecione colunas com regras pelos nomes, NU_NOTA_MT.

Defina o painel de configurações, conforme a imagem:
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/6-clean-data.png)

Não realizei a normalização dos dados, pois ambos estão na mesma grandeza, no caso notas de prova de 0 a 100. Porém, eu poderia realizar melhorias, como adicionar novas colunas, como colunas calculadas, para enriquecer o meu conjunto de dados.

### 8. PASSO 4: Criar e executar um pipeline de treinamento
Após a transformação dos dados, vamos treinar um modelo de machine learning.

#### 8.1. Adicionar módulos de treinamento

1 - No painel à esquerda, na seção "Transformações de Dados", arraste um módulo **Dividir Dados** na tela, p/ baixo do módulo "Limpar Dados Ausentes". Em seguida, conecte a saída que contém *Conjunto de dados transformados* (à esquerda) do módulo "Limpar Dados Ausentes" para a entrada do módulo "Dividir dados".
2 - Selecione o módulo Dividir dados e defina as configurações dele da seguinte maneira:

- Modo de divisão: dividir linhas
- Fração das linhas no primeiro conjunto de dados de saída: 0,7
- Semente aleatória: 123
- Divisão estratificada: Falso

3 - Expanda a seção "Treinamento de Modelo" no painel à esquerda e arraste um módulo **Treinar Modelo** na tela, p/ baixo do módulo "Dividir Dados". Em seguida, conecte a saída (à esquerda) deste último na entrada (à direita) do módulo de treino.
4 - O modelo que estamos treinando preverá o valor de NU_NOTA_CN. Portanto, selecione o módulo "Treinar Modelo" e modifique as configurações dele para definir a Label Column como NU_NOTA_CN. (variável alvo)

Como o rótulo NU_NOTA_CN que o modelo irá prever é um valor numérico, então precisamos treinar o modelo usando um algoritmo de *regressão*.
5 - Expanda a seção "Algoritmos de Machine Learning" e, sob "Regressão", arraste um módulo **Regressão Linear** para a tela, à esquerda do módulo "Dividir Dados" e acima do módulo "Treinar Modelo".  Em seguida, conecte a saída deste novo módulo à entrada do *Modelo não treinado* (à esquerda) do módulo "Treinar Modelo".

Para testar o modelo treinado, precisamos usá-lo para pontuar o conjunto de dados de validação que retivemos quando dividimos os dados originais.
6 - Expanda a seção "Pontuação e Avaliação de Modelo" e arraste um módulo **Pontuar Modelo** para a tela, abaixo do módulo "Treinar Modelo". Em seguida, conecte a saída deste, à entrada *Modelo treinado* (à esquerda) do módulo "Pontuar Modelo" e arraste a saída (à direita) do módulo "Dividir dados" para a entrada Conjunto de dados (à direita) do módulo "Pontuar Modelo".

#### 8.2. Resultados do treinamento
Após feito e executado o experimento do pipeline de treinamento, seguem os resultados parciais:
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/8-resultado-score-model-treino.png)
Observe que ao lado da coluna NU_NOTA_CN (que contém os valores verdadeiros), há uma nova coluna denominada **Rótulos pontuados**, que contém os valores de rótulo previstos.
Pode-se ver também, que o resultado não foi bom...rs

### 9. PASSO 5: Avaliar um modelo de regressão
O modelo está prevendo valores para o rótulo preço, mas o quão confiáveis são as previsões dele?

1- Na seção "Pontuação e avaliação do modelo", arraste um módulo **Avaliar Modelo** para a tela sob o módulo "Pontuar Modelo" e conecte a saída deste ultimo à entrada do *Conjunto de dados pontuado* (à esquerda) do módulo "Avaliar Modelo".

Confira como fica o pipeline de treino executado:
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/7-pipeline-treinamento.png)

#### 9.2. Avaliação do modelo
Selecione o módulo "Avaliar Modelo" e, no painel de configurações, na guia Saídas + logs, em Saídas de dados na seção Resultados da avaliação, use o ícone **Visualizar** para ver os resultados.
![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/9-avaliacao-do-modelo-pipeline-treino.png)
Os resultados incluem as métricas:

- MAE (erro médio absoluto): a diferença média entre os valores previstos e os valores reais. Quanto menor esse valor, melhor a qualidade da previsão do modelo;
- REQM (raiz do erro quadrático médio): A raiz quadrada da diferença quadrática média entre os valores previstos e verdadeiros. Quando comparado ao MAE (acima), uma diferença maior indica maior variância nos erros individuais;
- RSE (erro ao quadrado relativo): uma métrica relativa entre 0 e 1 com base no quadrado das diferenças entre os valores previstos e os reais. Quanto mais próximo de 0 essa métrica for, melhor será o desempenho do modelo;
- RAE (erro absoluto relativo): uma métrica relativa entre 0 e 1 com base nas diferenças absolutas entre os valores previstos e os reais. Quanto mais próximo de 0 essa métrica for, melhor será o desempenho do modelo;
Coeficiente de determinação (R 2): essa métrica é mais comumente conhecida como R-quadrado e resume a quantidade de variância entre os valores previstos e verdadeiros que é explicada pelo modelo. Quanto mais próximo de 1 for esse valor, melhor será o desempenho do modelo.

