# IA.md

## 1. Como fui direcionada

Ao longo do desenvolvimento do case, fui dando contexto conforme as etapas avançavam e conforme surgiam dúvidas no processo.

No início, meu foco principal foi entender como estruturar o pipeline dos quatro indicadores pedidos no case: CDI, IPCA, dólar e Ibovespa. A partir disso, fui pedindo ajuda para definir como tratar cada série, principalmente porque elas possuem frequências e naturezas diferentes.

As minhas instruções foram ficando mais específicas conforme eu avançava. Em vários momentos, eu não pedi apenas um código pronto, mas questionei como os dados deveriam ser tratados economicamente antes de decidir como implementá-los. Por exemplo, perguntei sobre o uso de Base 100, tratamento de fins de semana e feriados, diferença entre médias e fechamentos mensais e como comparar indicadores que possuem unidades diferentes.

Também fui direcionando bastante a forma das respostas. Quando alguma explicação estava complexa demais, pedi uma versão mais simples ou um passo a passo. Na parte do Power BI e do GitHub, por exemplo, precisei de orientações bastante práticas, com indicação do que clicar e em que ordem fazer cada etapa.

Em alguns momentos, pedi uma estrutura antes de implementar. Isso aconteceu principalmente na organização do dashboard, quando discutimos quais páginas deveriam existir, quais perguntas cada gráfico deveria responder e quais indicadores deveriam aparecer juntos.

Na parte do código, houve um processo mais iterativo. Eu executava o que era sugerido, verificava o resultado e voltava com erros, prints ou questionamentos. Com isso, o código e as decisões foram sendo ajustados ao longo do processo.

---

## 2. Pedidos principais

Os cinco pedidos mais importantes que fiz durante o desenvolvimento foram:

### 1. Estruturar o pipeline dos indicadores

Pedi ajuda para entender como organizar a extração, transformação e preparação dos dados de CDI, IPCA, dólar e Ibovespa.

O principal desafio era preservar a natureza de cada série e, ao mesmo tempo, criar uma base que pudesse ser utilizada no Power BI.

### 2. Definir o tratamento correto de cada indicador

Questionei como transformar as séries para análise mensal e quais métricas fariam sentido.

Entre as decisões discutidas estavam:

- média mensal do CDI anualizado;
- composição do CDI para calcular retorno acumulado;
- IPCA mensal e acumulado em 12 meses;
- média mensal e fechamento do dólar;
- média mensal e fechamento do Ibovespa;
- cálculo das variações mensais.

### 3. Resolver problemas de extração e tratamento dos dados

Durante o desenvolvimento, encontrei erros na API do Banco Central e dificuldades para obter algumas séries.

Também tive problemas na importação dos CSVs para o Power BI, principalmente relacionados à interpretação dos separadores decimais.

Pedi ajuda para entender os erros e encontrar alternativas sem alterar de forma incorreta os dados.

### 4. Estruturar e montar o dashboard no Power BI

Pedi ajuda para decidir como apresentar os indicadores de forma clara para uma equipe comercial.

Discutimos:

- cartões com valores atuais;
- comparação entre CDI e IPCA;
- gráficos de dólar e Ibovespa;
- comparação entre os indicadores;
- organização das páginas;
- cores;
- navegação;
- quais informações eram realmente úteis para o público do dashboard.

### 5. Organizar a entrega no GitHub

Na etapa final, pedi ajuda para entender o que era um repositório, como funcionavam commits e como organizar os arquivos no GitHub.

Também pedi ajuda para estruturar o README e documentar as principais decisões tomadas durante o projeto.

---

## 3. Correções e questionamentos

Durante a conversa, houve vários momentos em que questionei ou rejeitei sugestões.

Um dos principais pontos foi a comparação dos indicadores. Quando foi sugerida a criação de cartões como “maior desempenho” ou “maior movimento”, questionei porque essas informações não estavam diretamente disponíveis nas bases e exigiriam novas medidas. Preferi utilizar métricas que já existiam e eram objetivamente mensuráveis.

Também questionei a comparação entre CDI, dólar e Ibovespa em Base 100. Antes de utilizar esse tipo de comparação, quis entender por que ela faria sentido e quais limitações existiam, principalmente porque os indicadores não possuem a mesma natureza econômica.

Outro ponto importante foi o tratamento de fins de semana e feriados. Em vez de simplesmente preencher os dias sem observação, questionei qual seria o significado econômico dessa decisão. A conclusão foi não criar observações artificiais para dias em que não existe negociação ou divulgação.

Na construção do dashboard, rejeitei algumas estruturas que estavam mais complexas do que eu precisava. Em alguns momentos, pedi para reduzir a quantidade de gráficos ou tornar as análises mais simples e diretamente ligadas às perguntas que a equipe comercial poderia fazer.

Também corrigi respostas relacionadas ao Power BI quando as opções indicadas não apareciam na minha versão do programa. Nesses casos, enviei prints da tela e pedi novas instruções com base no que realmente estava aparecendo.

Durante o processo de tratamento dos dados, também questionei sugestões quando elas exigiriam refazer partes do pipeline em um momento em que a API do Banco Central estava indisponível. Nesses casos, procurei alternativas que permitissem continuar o trabalho com as bases já obtidas.

---

## 4. Divisão das decisões

### Decisões tomadas por mim

Algumas decisões foram definidas diretamente por mim ou escolhidas após comparar alternativas apresentadas:

- utilização do R como linguagem principal do pipeline;
- utilização do Power BI para o dashboard;
- manutenção de um dashboard mais simples e comercial;
- divisão final do dashboard em duas páginas;
- preferência por métricas objetivas e mensuráveis;
- não preencher artificialmente fins de semana e feriados;
- utilização dos dados disponíveis mesmo diante das limitações temporárias da API;
- escolha final dos gráficos e da organização visual;
- decisão de manter o projeto com foco na interpretação dos indicadores, sem tentar transformar o dashboard em um relatório de research completo.

Também fui responsável por executar os códigos, verificar os resultados, identificar erros e decidir quais sugestões seriam ou não incorporadas ao projeto.

### Sugestões da IA que foram aceitas

Algumas sugestões foram apresentadas pela IA e aceitas após análise:

- tratar cada indicador de acordo com sua frequência e natureza;
- utilizar média mensal do CDI anualizado para acompanhar sua trajetória;
- converter o CDI anualizado em fator diário para calcular retornos acumulados;
- calcular o IPCA de 12 meses por composição e não por soma simples;
- utilizar fechamento mensal para calcular variações de dólar e Ibovespa;
- manter valores ausentes quando não existe histórico suficiente para o cálculo;
- tratar o último mês como MTD quando ele ainda estiver incompleto;
- separar uma base histórica mensal de uma base com valores atuais;
- documentar no README as decisões metodológicas e limitações;
- organizar os commits do GitHub de forma incremental;
- utilizar o README para explicar tanto a execução quanto as decisões do projeto.

Nem todas as sugestões foram utilizadas exatamente como apresentadas. Algumas foram simplificadas ou adaptadas ao que já estava implementado no projeto.

---

## 5. Pontos de atenção

Existem alguns pontos em que as sugestões da IA precisam de verificação humana.

### Série de dólar utilizada

O case descreve o indicador como Dólar PTAX venda. Durante o desenvolvimento, foi utilizada a série SGS de código 1.

Essa correspondência deve ser validada diretamente na fonte oficial antes de assumir que a série utilizada corresponde exatamente à definição pedida no case.

### Disponibilidade da API do Banco Central

Durante o projeto, a API do Banco Central apresentou erros temporários, incluindo respostas 502.

Por isso, alguns erros encontrados durante a execução podem estar relacionados à disponibilidade da fonte e não necessariamente ao código.

### Comparação entre indicadores

CDI, IPCA, dólar e Ibovespa possuem naturezas muito diferentes.

Comparações visuais entre eles devem ser interpretadas com cuidado. Mesmo quando transformados para facilitar a comparação, isso não significa que possuam o mesmo risco, significado econômico ou comportamento.

### Relações entre as séries

Movimentos simultâneos entre dólar, Ibovespa, inflação e juros não devem ser interpretados automaticamente como relações causais.

O dashboard apresenta principalmente relações descritivas.

### Power BI

Algumas orientações sobre Power BI precisaram ser corrigidas ao longo da conversa porque menus e opções podem variar dependendo da versão e do tipo de visual utilizado.

Por isso, as configurações finais do dashboard devem ser verificadas diretamente no arquivo entregue.

### Reprodução do pipeline

O README explica como executar o projeto do zero, mas é importante verificar a execução completa em um ambiente limpo antes da entrega.

Isso inclui confirmar:

- instalação dos pacotes;
- funcionamento do `reticulate`;
- disponibilidade do `yfinance`;
- disponibilidade das APIs;
- caminhos utilizados para salvar e carregar os arquivos.

### Documentação

A documentação foi construída durante o desenvolvimento e revisada com apoio de IA.

Mesmo assim, os nomes dos arquivos, pastas, gráficos e variáveis descritos no README devem ser conferidos com a versão final do repositório para evitar diferenças entre a documentação e os arquivos efetivamente entregues.
