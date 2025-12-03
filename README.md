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

---
