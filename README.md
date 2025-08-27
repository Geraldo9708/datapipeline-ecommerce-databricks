# Pipeline E-commerce (Databricks | Spark SQL)

**Objetivo:** Demonstrar um pipeline **ELT** em camadas **Bronze → Silver → Gold** a partir de um arquivo JSON de eventos de e-commerce, com parsing, normalização (epoch→timestamp), `explode` de itens e modelagem de dados para consulta (dimensões e fato).

> Este repositório contém o notebook exportado do Databricks e a estrutura para documentação/portfólio. Próximos passos incluem orquestração (Airflow/Dagster), testes de dados (dbt tests) e exposição via API (FastAPI).

## Arquitetura (Medallion)
```mermaid
flowchart LR
    A[Arquivo JSON (eventos)] -->|Ingestão| B[Bronze (raw)]
    B -->|Transformação| C[Silver (curado/normalizado)]
    C -->|Modelagem| D[Gold (dimensões + fato)]
    D -->|Consumo| E[Consultas analíticas / BI / API]
```

## Estrutura
```
datapipeline-ecommerce-databricks/
├─ notebooks/
│  └─ 01-pipeline-ecommerce-databricks.py   # Notebook exportado do Databricks
├─ data/
│  └─ ecommerce_sample.json                 # Amostra mínima do dataset
├─ docs/
│  ├─ diagram.md                            # Diagrama e anotações
│  └─ screenshots/                          # Prints do pipeline/consultas
├─ tests/
│  └─ README_tests.md                       # Diretrizes de testes (futuros)
├─ .gitignore
├─ LICENSE
└─ README.md
```

## Como usar (Databricks)
1. **Importe** `notebooks/01-pipeline-ecommerce-databricks.py` para o seu workspace no Databricks.
2. **Anexe** a um cluster com **Spark SQL** habilitado.
3. **Carregue** a amostra `data/ecommerce_sample.json` em um local acessível (DBFS, upload, ou ajuste o caminho no notebook).
4. **Execute** as células na ordem:
   - Bronze: leitura e persistência do bruto.
   - Silver: parsing/normalização (`from_json`, epoch→timestamp, `explode` de itens).
   - Gold: tabelas de dimensões e fato, filtros por `event_name='finalize'`, métricas derivadas.
5. **Consultas**: rode as queries de top produtos, cupons, vendas por UF/data, etc.

## Próximos passos sugeridos
- **Orquestração**: DAG no Airflow ou Job no Dagster com retries/backfill.
- **Testes de dados**: adicionar `dbt` com testes (`not_null`, `unique`, `relationships`).
- **API**: FastAPI expondo o `gold` com paginação/filtros e OpenAPI.
- **Observabilidade**: logging estruturado e validações de contagem por camada.
- **SCD2 (dim_produtos)**: controlar histórico quando nome/preço mudarem.

## Portfólio (como descrever no CV/LinkedIn)
- Pipeline **ELT** (Databricks/Spark SQL) com **Bronze → Silver → Gold**; parsing JSON, epoch→timestamp, `explode` de itens.
- **Modelagem** de **dimensões (cidades/produtos)** e **fato de vendas** (cálculo de total, filtro por evento 'finalize').
- **Consultas analíticas**: ranking de produtos, uso de cupons, vendas por período/UF.

---

**Licença:** MIT
