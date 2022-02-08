# airbnb_ai

![Code Size](https://img.shields.io/github/languages/code-size/pkoliveira/airbnb_ai)
![Contributors](https://img.shields.io/github/contributors/pkoliveira/airbnb_ai?color=dark-green) 
![Issues](https://img.shields.io/github/issues/pkoliveira/airbnb_ai) 


## Uso

Para executar o notebook do repositório, checar a instalação dos pacotes descritos em #Requisitos, e baixar (ou indicar a url do dado): http://data.insideairbnb.com/brazil/rj/rio-de-janeiro/2021-12-24/data/listings.csv.gz

## Requisitos
```
re >= 2.2.1
tensorflow >= 2.3.1
xgboost >= 1.5.2
sklearn >= 0.24.0
numpy >= 1.18.1
matplotlib >= 3.1.3
pandas >= 1.2.4
seaborn >= 0.10.0
```

## a. Como foi a definição da sua estratégia de modelagem?

Partiu-se de uma rede simples, e que foi sendo refinada para melhor se adequar à base de dados. Nesse caso, utilizando uma MLP com apenas uma camada oculta. Uma outra rede testada foi a XGB. Há espaço para melhoria nas redes, dada as métricas de avaliação, para que apresentem resultados aprimorados.

## b. Como foi definida a função de custo utilizada?

Para um teste simples, foi avaliada a ativação relu em camadas intermediárias e, por se tratar de classificação, dentre as possibilidades, foi utilizada a ativação softmax, retornando a probabilidade de pertencimento entre as classes. O otimizado escolhido foi o adam, bastante utilizado na literatura, e que apresenta otimização de taxa de aprendizado dinâmica. Como função de custo, a escolhida é a categorical_crossentropy, utilizada para classificação e que age em conjunto com a função de ativação da última camada.

## c. Qual foi o critério utilizado na seleção do modelo final?

Foi realizado um refinamento de hiperparâmetros para se chegar na configuração final, porém ainda carece de refinamento já que os modelos não classificaram bem o suficiente todas as classes. Pode-se utilizar técnicas de otimização de hiperparâmetros para isso, permitindo explorarmos os que se apresentarem mais promissores, partindo de técnicas como random search ou grid search em um escopo de busca. Existem inclusive, algumas abordagens que fazem essa busca de forma mais automatizada.

## d. Qual foi o critério utilizado para validação do modelo? Por que escolheu utilizar este método?

Para avaliar se o modelo está bom o suficiente, foi utilizada a técnica de folding, especificamente o K-Fold. A ideia é realizar o treinamento e teste do modelo em diferentes segmentos de amostras. A média dos folds representa como o modelo se comportaria em uma situação real (produção).

Então, a partir das métricas utilizadas, se em todos os folds elas tiverem um bom desempenho, então o modelo, de maneira geral, também refletirá o bom desempenho.

## e. Quais evidências você possui de que seu modelo é suficientemente bom?

A acurácia, por si só, é uma péssima métrica para avaliar o desempenho do modelo, pois avalia a qualidade de forma geral. Existem métricas que auxiliam na avaliação individual de cada classe, dando então um maior panorama para avaliação do modelo. 

O conjunto de métricas:
- matriz de confusão: apresenta a relação entre as amostras corretamente classificadas (verdadeiras) e as incorretamente classificadas (falsas), a partir dela, podemos realizar o cálculo de outras métricas, como precision e recall
- precision: calcula as classes corretamente classificadas como verdadeiras
- recall: calcula a classificação correta das amostras de acordo com todas as amostras que deveriam ser verdadeiras
- f1-score: cálculo a partir do precision e recall, basicamente resume essas duas métricas
- curva roc: não perfeitamente funcional neste caso, mas apresenta o desempenho para cada classe. Recomendável para bases balanceadas
- curva precision-recall: ideal para este tipo de problema, conseguimos ter uma noção melhor do desempenho em bases sem balanceamento

Fornecem uma gama de informações para avaliar a qualidade do modelo, a partir delas conseguimos induzir se o modelo está generalizando bem ou não. Partindo disto, o modelo apresentado apresenta problemas para classificação de classes menos representadas na base de dados (classes 1 e 3), apresentando métricas ruins de forma geral. Isso indica que o modelo precisa de uma maior exploração de hiperparâmetros, assim como abordagens para reduzir o desbalanceamento ou melhorar o aprendizado.

Um outro modelo sugerido, o XGB, por si só, apresenta métricas bem superiores em 3 das 4 classes, porém ainda tendo problemas com a classe 1, que é a menos representativa em toda a base de dados.
