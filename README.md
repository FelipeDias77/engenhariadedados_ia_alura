# VoeBem Analytics: Engenharia de Dados & IA

Projeto prático desenvolvido durante a Imersão em Engenharia de Dados e IA da Alura. O objetivo deste projeto é construir um pipeline de dados completo para processar, tratar e modelar dados públicos de voos da ANAC (VRA), permitindo a análise detalhada sobre pontualidade, atrasos e cancelamentos da malha aérea brasileira.

## Arquitetura Medalhão

O projeto foi estruturado no **Databricks** utilizando a Arquitetura Medalhão para garantir governança, qualidade e performance:

*   🥉 **Camada Bronze:** Ingestão dos dados brutos e históricos provenientes da ANAC (aeródromos, companhias aéreas e histórico de voos).
*   🥈 **Camada Silver:** Limpeza, transformação e validação de qualidade utilizando **Delta Live Tables (DLT)**. Foram aplicadas regras de *Data Quality (Expectations)* para garantir a integridade dos dados, isolando registros inconsistentes em uma tabela de quarentena (`vra_quarentena`).
*   🥇 **Camada Gold:** Modelagem dimensional (`dim_aeroporto`, `fato_voos`) e criação de uma One Big Table (`obt_voos`) totalmente desnormalizada e otimizada para consumo focado em métricas de negócio.

## Inteligência Artificial (Databricks Genie Agent)

Além do processamento tradicional de dados, o projeto conta com uma camada de Inteligência Artificial para autoatendimento (*Self-Service Analytics*). 

Foi configurado o **Databricks Genie Agent**, permitindo que usuários de negócio façam perguntas em linguagem natural (ex: *"Quais rotas domésticas têm o maior atraso médio?"*) e a IA gere automaticamente as consultas SQL corretas baseadas na tabela OBT da camada Gold, respeitando as regras e premissas de negócio documentadas.

## Estrutura do Repositório

*   `/notebooks`: Notebooks Databricks utilizados para a ingestão e exploração inicial dos dados.
*   `/pipelines/qualidade`: Scripts SQL do pipeline do Delta Live Tables, responsáveis pelas transformações e testes de qualidade da camada Silver.
*   `/sql/gold`: Códigos DDL e DML para a criação das tabelas dimensionais e da `obt_voos` na camada Gold.
*   `/genie_agent`: Arquivos de documentação contendo as diretrizes de negócio, *prompts* de sistema e *sample queries* utilizadas para treinar o agente de IA generativa.

## Tecnologias e Ferramentas Utilizadas

*   **Ambiente Cloud:** Databricks
*   **Linguagens:** SQL, Python (PySpark)
*   **Processamento & Armazenamento:** Delta Lake, Delta Live Tables (DLT)
*   **GenAI:** Databricks Genie
*   **Versionamento:** Git / GitHub

---
## Autor

**Felipe Dias Santana**  
*Estudante de Sistemas para Internet (IFB) e Estagiário de Dados.*
