# Projeto Apache Spark + Delta Lake + MinIO + MongoDB

Projeto desenvolvido para prática de Engenharia de Dados com Spark e Delta Lake. O fluxo consiste na leitura de dados de um banco de dados NoSQL (MongoDB) com a extração de coleções no formato JSON, ingestão em um Object Storage (MinIO) e a transformação desses dados em tabelas Delta Lake na camada Bronze.

* Extração de coleções de um banco MongoDB (Local/Docker)
* Carga no MinIO (Object Storage compatível com S3) no formato JSON
* Conversão para formato Delta Lake (landing-zone -> bronze)
* Manipulação dos dados com comandos DML (INSERT, UPDATE, DELETE) e exploração de Time Travel

## Arquitetura

    ┌─────────────────┐     ┌──────────────────┐     ┌───────────────────┐
    │     MongoDB     │────▶│   MinIO (S3)     │────▶│   MinIO (S3)      │
    │      Local      │     │   landing-zone/  │     │   bronze/         │
    │                 │     │   (JSONs)        │     │   (Delta Tables)  │
    │ VendasArCondic..│     │                  │     │                   │
    │   4 Coleções    │     │  1 JSON/coleção  │     │   INSERT/UPDATE   │
    │                 │     │                  │     │   DELETE/HISTORY  │
    └─────────────────┘     └──────────────────┘     └───────────────────┘
         Notebook 00             Notebook 01            Notebooks 02/03
        (Carga Inicial)          (Extração)              (Delta + DML)


## Pré-requisitos
* Linux (Ubuntu 24.04 ou WSL do Windows 11)
* Docker e Docker Compose v2+
* Python 3.11 (PySpark 3.5.3 requer Python <= 3.12)
* Java 11 (OpenJDK)
* [uv](https://docs.astral.sh/uv/) (Gerenciador de pacotes e ambientes Python)

## Setup do Ambiente

### 1. Subir os Containers (MongoDB + MinIO)

    docker compose up -d

**Containers criados:**

| Container | Imagem | Portas |
| :--- | :--- | :--- |
| mongodb-local | mongo:latest | 27017 |
| minio | minio/minio:RELEASE.2025-02-03... | 9020 (API), 9021 (Console) |

**Credenciais:**

| Serviço | Usuário | Senha |
| :--- | :--- | :--- |
| MongoDB | admin | 1234 |
| MinIO | minioadmin | minioadmin |

*Console MinIO:* http://localhost:9021

### 2. Configurar Variáveis de Ambiente
Crie o arquivo de configuração a partir do template fornecido:

    cp .env.example .env

*(Certifique-se de que os dados no arquivo .env correspondam às credenciais da tabela acima).*

### 3. Configurar o Ambiente Python
Crie o ambiente virtual e instale as dependências usando o uv:

    uv venv
    source .venv/bin/activate
    uv sync

## Executando o Projeto

Execute os notebooks na seguinte ordem. **Importante:** Certifique-se de selecionar o ambiente virtual (.venv) como Kernel do Jupyter antes de executar as células.

| # | Notebook | Descrição |
| :--- | :--- | :--- |
| 00 | 00_setup_mongo.ipynb | Cria o database VendasArCondicionado no MongoDB e gera a carga inicial de dados fictícios em 4 coleções. |
| 01 | 01_mongo_to_minio_json.ipynb | Extrai todas as coleções do MongoDB -> JSON no MinIO (bucket landing-zone). |
| 02 | 02_json_to_delta.ipynb | Lê os arquivos JSON do MinIO e converte para tabelas Delta Lake (bucket bronze). |
| 03 | 03_dml_delta.ipynb | (Próximo passo) Executa comandos DML (INSERT, UPDATE, DELETE), exibe HISTORY e TIME TRAVEL. |

## Estrutura do Projeto

    spark-delta-minio-mongodb/
    ├── docker-compose.yml                   # Infraestrutura: MongoDB + MinIO
    ├── .env.example                         # Template de variáveis de ambiente
    ├── pyproject.toml                       # Dependências Python gerenciadas pelo UV
    ├── .python-version                      # Python 3.11
    ├── notebook/                            # Notebooks Jupyter
    │   ├── 00_setup_mongo.ipynb             # Setup: Criação de base e coleções no MongoDB
    │   ├── 01_mongo_to_minio_json.ipynb     # Extração: MongoDB -> MinIO (JSON)
    │   ├── 02_json_to_delta.ipynb           # Conversão: JSON -> Delta Lake (Camada Bronze)
    │   └── 03_dml_delta.ipynb               # DML: Manipulação e Time Travel no Delta
    └── README.md

## Tecnologias Utilizadas
* **Apache Spark 3.5.3 (PySpark):** Motor de processamento distribuído.
* **Delta Lake 3.2.0:** Formato de armazenamento com suporte a transações ACID.
* **MinIO:** Object Storage local compatível com Amazon S3.
* **MongoDB:** Banco de dados NoSQL orientado a documentos.
* **Docker Compose:** Orquestração de containers locais.
* **Python 3.11 + uv:** Linguagem base e gerenciamento ultrarrápido de dependências.

## Conceitos Demonstrados
* Ingestão e extração de dados em bancos NoSQL.
* Tratamento de conversão de tipos (ex: ObjectIds do MongoDB para String).
* Object Storage como repositório principal de dados (Data Lake).
* Transformação de JSON multi-linha em tabelas estruturadas via PySpark.
* Arquitetura Medalhão (Camada Landing Zone -> Camada Bronze).
* Transações ACID e versionamento de dados em Data Lakes.

## Links e Referências
* [MongoDB - Documentação Oficial](https://www.mongodb.com/docs/)
* [MinIO - Documentação](https://min.io/docs/minio/linux/index.html)
* [Delta Lake - Releases](https://docs.delta.io/latest/index.html)
* [PyMongo - Documentação](https://pymongo.readthedocs.io/en/stable/)