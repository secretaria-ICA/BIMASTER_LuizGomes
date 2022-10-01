# Scraping e previsão de preços de passagem Rio - Berlim

#### Aluno: [Luiz Gomes Ribeiro Neto](https://github.com/luizgrneto)
#### Orientador: [Felipe Borges](https://github.com/FelipeBorgesC)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Scrap_Precos_2022.ipynb](Scrap_Precos_2022.ipynb)
- [Scrap_Precos_2023.ipynb](Scrap_Precos_2023.ipynb)
- [Modelos.ipynb](Modelos.ipynb)
- [Modelos_Normalizado.ipynb](Modelos_Normalizado.ipynb)
- [LSTM.ipynb](LSTM.ipynb)

---

### Resumo

Este trabalho tem como ideia unir um dos hobbys preferidos do aluno, que é viajar, com as disciplinas que o mesmo melhor se identificou: Web Scrapping e Machine Learning. O intuito é criar um protótipo de ferramenta que coletará dados de passagens e estabelecerá uma previsão de preço dos próximos meses.

Como as bases de dados de preços de passagens encontradas da internet são particulares/pagas, foi desenvolvido um código que faz a leitura e armazenamento dos preços encontrados no site Skyscanner. Com esses dados em mãos (cerca de um ano de preços de passagens, coletados de 3 em 3 dias com duração de 7 dias de viagem), faz-se um estudo dos principais modelos de Machine Learning e uma rede neural Long Short-Term Memory (LSTM) para previsão de preços para o futuro. 

### 1. Introdução

Todo turista ou viajante que começa a preparar alguma viagem se depara com o mesmo problema: quando comprar sua passagem para obter o preço mais barato. Através da raspagem de dados e com algumas análises de regressão em Machine Learning, tentaremos prever o comportamento dos preços ao longo de um período e descobrir qual o melhor período para fazer a compra de passagem de ida e volta. 

Para simplificar, utilizaremos a cidade do Rio de Janeiro como origem, Berlim como destino, e um período de viagem de 7 dias. É possível alterar esses valores no código, se assim desejar. 

### 2. Modelagem

De início, viu-se que era necessário a criação de um dataset próprio para treino dos modelos, pois os datasets presentes na internet são escassos e quando existem, são particulares e/ou pagos. 

Portanto, decidiu-se pela raspagem de dados do site Skyscanner, por já ser conhecido e considerado confiável. A ferramenta utilizada foi a biblioteca Selenium - ferramenta já amplamente conhecida e utilizada para automatizar operações de browsers - na linguagem Python. 

Alguns parâmetros e técnicas utilizadas para este processo foram: 
- Criação de um profile próprio para o Chrome utilizado pelo Selenium (arquivos estão dentro da pasta selenium).
- Definição de parâmetros do webdriver do Chrome, como versão do navegador, flag de automação desativada, tamanho da janela e o acréscimo de tempos de espera ao longo do código, para deixar o processo mais "humano", e menos "robótico". 
- A url utilizada pelo Chromedriver já foi com as querys da busca pré-definidas, pois na página inicial do Skyscanner existem ferramentas de calendário para definir data de ida e chegada que utilizam Javascript e outras ferramentas, dificultando o processo de automatização. 

A raspagem é feita em cima do código HTML aberto da própria página do Skyscanner e busca unicamente o preço "mais barato", definido na parte de cima da página. Para armazenar o valor do preço, é guardada a data de ida, de volta, a cidade e o valor do preço em si, já incluso a ida e a volta definidas pelo Skyscanner. Para isso, é utilizada a biblioteca Beautiful Soup, para Python. 

O dataset final ficou da seguinte forma: 

![Dataset Completo](https://github.com/luizgrneto/BIMASTER_LuizGomes/blob/main/Imagens/dataset_completo.PNG)

Com os valores dos preços em mãos, foram feitos estudos de diversos tipos de modelos de regressão de Machine Learning, como Random Forest Regressor, Decision Tree Regressor, KNN Regressor (biblioteca sklearn) e XGBoost (biblioteca xgboost), todos com suas respectivas funções de hypertuning para encontro do melhor resultado. Também foi utilizada uma rede LSTM, da biblioteca Keras.  

A métrica utilizada para comparação foi o MAE (Mean Absolute Error). 

### 3. Resultados

Os melhores resultados ficaram igualmente com a rede LSTM e com o modelo Random Forest Regressor, ambos com MAE em torno de 410 reais:

![Predição em cima do teste](https://github.com/luizgrneto/BIMASTER_LuizGomes/blob/main/Imagens/trecho_predicao.PNG)

### 4. Conclusões

Conseguimos um modelo de predição com erro de cerca de 410 reais, para trechos de passagem que variam facilmente em torno de R$10.000,00 e R$3.000,00. Em termos de custo, a variação do erro é aceitável. Além disso, pela previsão do modelo acima, o período ideal de compra de passagem nesse período seria entre o sexto e o vigésimo quarto dia a partir da consulta.

Há algum espaço para melhora que inclui acrescentar mais variáveis aos modelos, como escalas, tempo de viagem, e mais pontos de predição. 

---

Matrícula: 202190324

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
