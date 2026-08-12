# Pipeline de Indicadores de Mercado — Vêneto Family Office

## Sobre o projeto

Esse projeto foi desenvolvido para o case técnico do processo seletivo da Vêneto Family Office.
A proposta era construir um pipeline de dados de mercado, desde a extração até a visualização final em um dashboard, pensando principalmente no uso por uma equipe comercial de Family Office.
Os quatro indicadores analisados foram:
- CDI
- IPCA
- Dólar
- Ibovespa

Para desenvolver o projeto, usei R para a parte de extração e tratamento dos dados, CSV para organizar as bases finais e Power BI para o dashboard.

## Fontes dos dados

As séries foram obtidas das seguintes fontes:

### CDI
Fonte: Banco Central do Brasil — SGS  
Código utilizado: 4389

### IPCA
Fonte: Banco Central do Brasil — SGS  
Código utilizado: 433

### Dólar
Fonte: Banco Central do Brasil — SGS  
Código utilizado: 1

### Ibovespa
Fonte: Yahoo Finance, utilizando o `yfinance`  
Ticker: `^BVSP`


## Como funciona o pipeline

Eu organizei o projeto em quatro etapas principais:

**Extração → Transformação → Carga → Dashboard**

### Extração

Na extração, busquei as séries do CDI, IPCA e dólar pela API do Banco Central. Para o Ibovespa, usei o `yfinance` pelo `reticulate`, dentro do R. O período utilizado foi dos últimos 5 anos a partir da data de execução.

### Transformação

Depois da extração, fiz o tratamento das bases para deixar as séries em um formato mais simples de usar no Power BI. Entre os tratamentos realizados estão:
- padronização das datas;
- conversão dos valores para formato numérico;
- retirada de valores ausentes;
- retirada de datas duplicadas;
- organização cronológica;
- criação de métricas mensais.

Como os quatro indicadores têm frequências e naturezas diferentes, eu não quis tratar todos do mesmo jeito.

## Tratamento de cada indicador

### CDI

O CDI vem como uma taxa anualizada em base 252. Para analisar a trajetória, usei a média mensal da taxa. Para calcular o retorno acumulado, converti a taxa anualizada em uma taxa diária equivalente e depois fiz a composição das taxas dentro do mês. As principais métricas utilizadas foram:
- `cdi_medio_aa`
- `cdi_acumulado_mes`

### IPCA

O IPCA já é uma série mensal, então não fez sentido calcular média mensal. Mantive o valor mensal oficial e também calculei o IPCA acumulado em 12 meses por composição. As principais métricas foram:

- `ipca_mes`
- `ipca_12m`
- `indice_precos`

Os primeiros valores do IPCA 12 meses ficam vazios porque ainda não existem 12 meses anteriores suficientes para fazer o cálculo. Preferi manter esses valores vazios em vez de preencher artificialmente.

### Dólar

Para acompanhar a trajetória do dólar, usei a média das cotações disponíveis dentro de cada mês. Também mantive a última cotação do mês para calcular a variação mensal de fechamento contra fechamento. As principais métricas foram:
- `dolar_medio`
- `dolar_fechamento`
- `dolar_variacao_mes`

### Ibovespa

Para o Ibovespa, usei a média mensal dos fechamentos para acompanhar a trajetória. Para calcular o retorno mensal, utilizei o último fechamento de cada mês em relação ao fechamento do mês anterior. As principais métricas foram:
- `ibov_medio`
- `ibov_fechamento`
- `ibov_retorno_mes`


## Fins de semana, feriados e dados faltantes

Não preenchi fins de semana e feriados artificialmente. Como nesses dias não existe observação de mercado, preferi trabalhar apenas com os dados que realmente estavam disponíveis. Caso exista alguma ausência em um dia útil, esse valor deve ser investigado antes de ser preenchido.


## Bases finais

No final do tratamento, gerei duas bases principais.

### `base_mensal.csv`

Essa base contém o histórico mensal dos indicadores e as métricas criadas para análise.

### `dados_atuais.csv`

Essa base contém o último valor disponível de cada indicador e é utilizada principalmente nos cartões do Power BI.


## Dashboard

O dashboard foi desenvolvido no Power BI pensando em uma equipe comercial que precisa ter uma leitura rápida dos principais indicadores de mercado.

Optei por dividir o painel em duas páginas: uma voltada para a visão geral do mercado e outra para comparação do desempenho dos indicadores.

### Visão de Mercado

A primeira página apresenta os últimos valores disponíveis de:

- CDI
- IPCA 12 meses
- Dólar
- Ibovespa

Também foi incluído um gráfico comparando a trajetória do CDI e do IPCA, permitindo acompanhar o comportamento dos juros em relação à inflação ao longo do período.

A ideia dessa página é funcionar como uma leitura inicial do cenário, reunindo os principais indicadores de forma simples e rápida.

### Desempenho

A segunda página foi criada para aprofundar a comparação entre os indicadores.

Ela contém três análises principais:

- trajetória do Ibovespa em relação ao câmbio;
- comparação entre IPCA e CDI;
- comparação conjunta dos indicadores.

Como os indicadores possuem unidades e naturezas econômicas diferentes, as comparações devem ser interpretadas principalmente pela trajetória e pelo movimento relativo das séries, e não como uma equivalência entre elas. O objetivo dessa página é facilitar a identificação de movimentos conjuntos ou divergentes entre os principais indicadores acompanhados.


## Meses incompletos

Como a data final do pipeline depende do dia em que ele é executado, o último mês pode estar incompleto. Por isso, quando necessário, as métricas do mês corrente devem ser interpretadas como MTD — Month to Date — e não como um mês fechado.


## Como rodar o projeto

Para rodar o projeto do zero é necessário ter:

- R
- RStudio
- Python
- Power BI Desktop

Pacotes utilizados no R:

- rbcb
- dplyr
- lubridate
- reticulate

Para o Ibovespa também é necessário ter o yfinance instalado no ambiente Python utilizado pelo reticulate.

A instalação pode ser feita com:

    install.packages(c("rbcb", "dplyr", "lubridate", "reticulate"))
    reticulate::py_install("yfinance")

Depois disso:

1. abrir o arquivo `tratamento de dados.Rmd` no RStudio;
2. executar o código desde o início;
3. gerar as bases tratadas;
4. abrir o arquivo do Power BI;
5. atualizar as fontes de dados, caso seja necessário.



## Estrutura do projeto

    src/
      tratamento de dados.Rmd

    data/
      processed/
        base_mensal.csv
        dados_atuais.csv

    dashboard/
      arquivo_power_bi.pbix

    README.md
    IA.md

Os nomes acima devem acompanhar os arquivos disponíveis no repositório.



## Limitações

Algumas limitações que considerei durante o desenvolvimento do projeto:

- os indicadores possuem frequências e naturezas econômicas diferentes;
- o IPCA é divulgado com uma frequência diferente das séries financeiras diárias;
- o último mês da base pode estar incompleto;
- o cálculo do IPCA acumulado em 12 meses exige histórico anterior suficiente;
- a API do Banco Central pode apresentar instabilidades temporárias;
- comparações entre os indicadores mostram movimentos e trajetórias, mas não significam que eles sejam equivalentes;
- relações observadas entre as séries são descritivas e não representam necessariamente causalidade.

Além disso, como o pipeline utiliza um período de cinco anos a partir da data de execução, os meses localizados nas extremidades da amostra podem conter menos observações do que um mês completo.



## Uso de Inteligência Artificial

Utilizei inteligência artificial como apoio durante o desenvolvimento principalmente para:

- organizar o pipeline;
- discutir decisões de tratamento;
- revisar partes do código;
- entender erros durante o processo;
- estruturar o dashboard;
- revisar a documentação.

As sugestões foram avaliadas e ajustadas ao longo do desenvolvimento, e as decisões finais foram tomadas de acordo com o objetivo do case e com o comportamento dos dados.

O detalhamento do uso de IA está disponível no arquivo `IA.md`.



## Tecnologias utilizadas

- R
- RStudio
- Banco Central do Brasil — SGS
- Python
- yfinance
- CSV
- Power BI
- GitHub



## Resultado final

O resultado do projeto é um pipeline que conecta fontes públicas de dados de mercado a bases tratadas e a um dashboard desenvolvido no Power BI. A ideia foi organizar os dados de forma simples e reproduzível, respeitando as diferenças entre cada indicador e transformando as séries em informações que possam apoiar uma leitura mais rápida do cenário de mercado.
