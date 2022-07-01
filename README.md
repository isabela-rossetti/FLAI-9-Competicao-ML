# 9ª Competição de Machine Learning FLAI - Modelo Supervisionado de Regressão 

INTRODUÇÃO

O modelo supervisionado de regressão foi desenvolvido com o objetivo de prever a demanda de aluguéis de bicicletas a partir de variáveis do dia e do clima, proposto como um desafio pela comunidade FLAI em sua 9ª competição de Machine Learning.\
“Segundo dados da Associação Brasileira do Setor de Bicicletas, o primeiro semestre de 2021 registrou um aumento de 34,17% nas vendas, em pesquisa que ouviu 180 lojistas do ramo em 20 estados brasileiros. ” (https://www.oliberal.com/economia/mercado-de-bicicletas-vem-crescendo-em-belem-desde-o-inicio-da-pandemia-1.441376, acesso em 28/06/2022).\
Com a crescente demanda o modelo de previsão se torna um aliado das empresas que prestam esse serviço de aluguel para que elas possam se planejar em relação a quantidade de bicicletas que serão disponibilizadas bem como suas manutenções ou renovações.\
No modelo de regressão apresentado a linguagem de programação utilizada foi Python e as bibliotecas foram Pandas, para importação, visualização e tratamento de algumas variáveis e PyCaret (especificamente a pycaret.regression) para construção, avaliação e deploy do modelo. O código foi rodado na ferramenta Google Colab.


DESENVOLVIMENTO

Os dados fornecidos foram separados em treino e teste. Os de treino contam com 4.500 linhas e com as seguintes variáveis: hora, dia, estação, feriado, temperatura, chuva, umidade, sol, visibilidade e vento, além da variável resposta: aluguéis. Já os dados de teste possuem 3.000 linhas e as mesmas variáveis, exceto a de resposta (aluguéis).\
A partir da análise dos dados de treino e construção do modelo, o mesmo foi aplicado aos dados de teste.\
Após a importação dos dados a primeira ação foi checar o tipo das variáveis nas planilhas de treino e teste. As variáveis numéricas (int ou float) são: hora, temperatura, chuva, umidade, sol, vento e aluguéis e as categóricas são: dia, estação, feriado e visibilidade.\
Porém ao visualizar os dados podemos notar que a variável visibilidade é numérica (e não categórica), então é necessário realizar a conversão em string, para que seja possível remover o símbolo de %, e utilizar a função pd.to_numeric para, enfim, transformá-la em numérica (float).\ 
Após a conversão foi construído o modelo de regressão utilizando a biblioteca pycaret.regression.\
Para configurar a regressão utilizamos a função setup, que possui 57 parâmetros para escolha desde tratamento dos dados até qual método de validação vai ser utilizado.
Neste modelo apenas o parâmetro da normalização foi definido como verdadeiro. De acordo com a documentação da biblioteca o padrão de normalização quando não definido (default) é o método z-score. O parâmetro session_id também foi definido para que seja possível reproduzir os mesmos resultados todas as vezes que o código for rodado.\
Com os parâmetros do setup definidos, a função compare_models foi utilizada para comparar e selecionar os 5 melhores modelos a partir do RMSE mais baixo (métrica utilizada para seleção do melhor modelo na competição) e guardá-los na variável “melhores”.\
A função stack_models (melhores), que segundo a documentação oficial tem como saída “uma grande pontuação com pontuações de validação cruzada por dobra” dentre os melhores modelos foi construída e o resultado do RMSE foi o menor em comparação aos outros modelos vistos na função compare_models. Enquanto o RMSE nos dados de treino do modelo Light Gradient Boosting Machine (LGBM) foi de 290.831, o RMSE do stack_models foi de 283.978.\
Quando aplicado aos dados de teste separados pelo modelo (e não os dados da planilha de teste fornecida para a competição) o RMSE do LGBM foi de 281.0477 e do stack_models foi de 271.586.\
O modelo LGBM foi tunado na tentativa de melhorar seu desemprenho, mas o resultado foi inferior ao modelo sem tunagem.
Portanto, o modelo escolhido para deploy foi o stack_models por apresentar menor RMSE.


CONCLUSÃO

Pensando em aplicar o modelo em um negócio, com a previsão em mãos é possível planejar a disponibilidade de bicicletas nos locais tendo como base qual a estação do ano em que estamos e os horários de picos, por exemplo.\
Ter dados para basear o planejamento é de suma importância em um negócio porque ele está diretamente ligado ao ganho ou perda financeiras de uma empresa. 
Sabendo que no inverno a média de aluguéis é baixa, pode-se planejar a manutenção preventiva de uma grande quantidade de bicicletas, por exemplo, enquanto no verão elas serão mais solicitadas. Ou simplesmente aceitar o risco de não ter bicicletas disponíveis quando a demanda for alta, mas ter uma ideia de quanto se deixaria de ganhar nesse caso. \
Outro planejamento pode ser feito pensando nos horários. Os de pico de aluguéis giram em torno das 6h e 18h, provavelmente horário em que muitas pessoas estão se deslocando para o trabalho. Nesse caso, se for possível identificar a pessoa que está alugando e a frequência em que ocorre, pode-se oferecer um plano mensal para fidelizar esse cliente.\
Enfim, as possibilidades de planejamentos com essas informações são enormes, mas não serão tratadas neste momento por necessitarem de mais informações e discussões entre áreas, não somente a de dados. O objetivo deste texto é apenas apresentar o modelo, conceitos e ferramentas que foram utilizadas para construir o modelo de regressão supervisionado.\
O modelo atual passa por tentativa de melhorias e futuramente será publicado neste diretório. Além de uma visualização desses dados em um dashboard.
