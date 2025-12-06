# 🚴 Estudo de Caso Cyclistic: Estratégia de Conversão de Ciclistas Casuais

## 1. Introdução e Tarefa de Negócios 

Este projeto é um estudo de caso prático para a empresa fictícia de bicicletas compartilhadas Cyclistic, com o objetivo de **desenvolver uma nova estratégia de marketing para converter ciclistas casuais em membros anuais**.

A diretora de marketing, Lily Moreno, identificou que o sucesso futuro da empresa depende de maximizar o número de assinaturas anuais, pois os **membros anuais são significativamente mais lucrativos** que os ciclistas casuais.

---

## 2. Perguntas Chave de Negócios

Para guiar a estratégia, a análise focou em responder as seguintes perguntas:

1.  **Como membros anuais e ciclistas casuais usam as bicicletas Cyclistic de maneiras diferentes?** *(Foco desta análise)*
2.  Por que os ciclistas casuais comprariam assinaturas anuais da Cyclistic?
3.  Como a Cyclistic pode usar a mídia digital para influenciar os ciclistas casuais a se tornarem membros?

---

## 3. Preparação dos Dados 

A análise utilizou dados históricos de viagens de bicicleta da Cyclistic, disponibilizados sob licença pela Motivate International Inc. Os dados utilizados são de **Janeiro a Outubro de 2025** (os mais atuais) e representam viagens no sistema de bicicletas compartilhadas de Chicago.

### Fontes de Dados e Estrutura

* **Fonte:** Dados mensais de viagens da Divvy Tripdata (disponíveis em [https://divvy-tripdata.s3.amazonaws.com/index.html](https://divvy-tripdata.s3.amazonaws.com/index.html)).
* **Estrutura de Colunas:** Cada arquivo CSV contém os seguintes 13 campos:
    * `ride_id` (Identificador único da viagem)
    * `rideable_type` (Tipo de bicicleta, ex: clássica, elétrica)
    * `started_at` (Data e hora de início da viagem)
    * `ended_at` (Data e hora de fim da viagem)
    * `start_station_name` (Nome da estação de início)
    * `start_station_id` (ID da estação de início)
    * `end_station_name` (Nome da estação de fim)
    * `end_station_id` (ID da estação de fim)
    * `start_lat`, `start_lng` (Latitude e Longitude de início)
    * `end_lat`, `end_lng` (Latitude e Longitude de fim)
    * `member_casual` (Tipo de cliente: "member" ou "casual")
* **Credibilidade (Critério ROCCC):** Os dados são considerados **Confidáveis** e **Completos** (rastreando fielmente as viagens). São **Originais** (diretamente da fonte), **Atuais** (2025) e **Abrangentes** (cobrem quase um ano).
* **Considerações de Privacidade:** O uso de informações de identificação pessoal (PII) é estritamente proibido, limitando a análise a padrões de uso (como as colunas acima) e não a dados demográficos pessoais.

### Entregável

O resultado desta fase é o arquivo **`cyclistic_viagens_unificadas_bruto.csv`**.

([01_cyclistic_preprocessing.ipynb](https://github.com/RaulHamad/Cyclistic_Case_Study_Coursera/blob/main/01_cyclistic_preprocessing.ipynb))

---

## 4. Limpeza e Criação de Dados Analíticos

Esta fase teve como objetivo transformar o conjunto de dados bruto (12 meses de dados unificados) em um conjunto limpo, consistente e enriquecido com métricas cruciais, preparando-o para a análise.

### Ferramentas e Metodologia

| Tarefa Principal | Ferramenta Escolhida | Justificativa |
| :--- | :--- | :--- |
| **Limpeza e Transformação** | **Python (Pandas)** | Escolhido por ser o padrão da indústria para manipulação de DataFrames. O Pandas oferece eficiência (principalmente com a sintaxe `.dt` e `groupby`) para lidar com grande quantidade de dados. |
| **Ambiente** | **Google Colab** | Utilizado para garantir reprodutibilidade, acesso em nuvem aos dados do Google Drive e escalabilidade do processamento. |

### Medidas de Integridade e Limpeza (Data Cleaning)

A integridade dos dados foi garantida pela aplicação rigorosa das seguintes regras de negócio no notebook `02_data_cleaning_and_feature_engineering.ipynb`:

 * **Tratamento de Nulos:** Todas as linhas onde faltavam informações geográficas críticas (`start_station_name`, `end_station_name`, `start_lat`, `end_lat`) foram **removidas** utilizando o método `df.dropna()`. Esta medida, embora tenha descartado aproximadamente 2 milhões de registros, foi crucial para garantir que a análise de estações e o mapeamento de *hotspots* fossem feitos apenas com dados completos e válidos.
 * **Remoção de Duplicatas:** Foi verificado e garantido que o campo `ride_id` (ID da viagem) era **único**, removendo quaisquer linhas duplicadas para evitar vieses nas contagens e médias.
 * **Filtragem de Anomalias de Tempo:** Foram removidas viagens que representavam erros de registro ou não eram representativas do uso normal do serviço:
    * Viagens com **duração inferior a 1 minuto** (anomalias de tempo curto).
    * Viagens com **duração superior a 24 horas** (indicadores de roubo ou falha grave de sistema).
*  **Verificação Categórica:** Foi assegurado que a coluna `member_casual` continha **somente** os valores válidos (`'member'` e `'casual'`).

### 3.3. Engenharia de Recursos (Feature Engineering)

Para responder à pergunta de negócio, foram criados novos campos analíticos a partir dos campos de data e hora:

| Coluna Criada | Cálculo | Relevância para a Análise |
| :--- | :--- | :--- |
| **`ride_length(min)`** | `(ended_at - started_at)` / 60 | **Métrica Chave** para calcular a duração média de viagem e diferenciar o uso casual (viagens mais longas) do membro (viagens de deslocamento mais curtas). |
| **`ride_length(hours)`** | `(ended_at - started_at)` / 3600 | **Métrica Chave** Controle e Filtragem: Usado para aplicar as regras de negócio de tempo limite (ex: remover viagens > 24 horas) de forma legível. |
| **`day_of_week`** | Extraído de `started_at` | Essencial para agrupar e comparar a frequência de uso em **dias úteis (0-4)** vs. **fins de semana (5-6)**. |
| **`month`** | Extraído de `started_at` | Utilizado para identificar padrões de uso **sazonal** (ex: casuais dominam no verão). |

### Entregável

O resultado desta fase é o arquivo **`cyclistic_dados_limpos_analise.csv`**.

([02_data_cleaning_and_feature_engineering.ipynb](https://github.com/RaulHamad/Cyclistic_Case_Study_Coursera/blob/main/02_data_cleaning_and_feature_engineering.ipynb))

---
