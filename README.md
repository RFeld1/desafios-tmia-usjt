# Desafio Final - Trilha Engenheiro IA Microsoft
Repo contendo relatório para os problemas apresentados no Desafio Final - Trilha Engenheiro IA Microsoft, que envolvem Machine Learning e Inteligência Artificial aplicados ao contexto da educação. O Desafio Final, faz parte da iniciativa de aprendizagem de IA, ofertada pelo grupo Ãnima em parceria com a Microsoft, para os estudantes das universidades do grupo, no primeiro semestre de 2021.

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

Resultado
Você deverá mostrar a previsão de uma nota de Ciências da Natureza com base em uma nota da prova de Matemática

## 4. Dataset utilizado:
Conjunto de dados contendo campos referentes ao Enem 2019. Utilizaremos apenas os dados referentes as nota das provas citadas acima.
Link para download do dataset completo: https://download.inep.gov.br/microdados/microdados_enem_2019.zip

### 5. PASSO 1: Criar um Workspace do Azure Machine Learning
- Nome do workspace: desafios-ml-sj

![](https://github.com/RFeld1/desafios-tmia-usjt/blob/main/Images/1-grupo-de-recrusos.png)

### 6. PASSO 4: Criar recursos de computação
Para treinar e implantar modelos usando o designer do Azure Machine Learning, é necessário usar a computação para executar o processo de treinamento e testar o modelo treinado após a implantação.

####  6.1. Criando uma instancia de computação
Instãncia de computação: estações de trabalho de desenvolvimento que os cientistas de dados podem usar para trabalhar com modelos e dados.
Foi criada a instância de computação com as seguntes configurações:
- Tipo de máquina virtual: CPU
- Tamanho da máquina virtual: Standard_DS11_v2 (escolha Selecionar entre todas as opções para pesquisar por este tamanho de máquina e escolhê-lo)
- Nome de computação: insira um nome exclusivo
- Habilitar o acesso SSH: não selecionado


