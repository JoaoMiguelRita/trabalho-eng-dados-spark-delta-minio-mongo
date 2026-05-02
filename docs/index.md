![Spark](https://img.shields.io/badge/Apache%20Spark-3.5.3-orange)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-3.2.0-blue)
![MongoDB](https://img.shields.io/badge/MongoDB-Latest-green)
![MinIO](https://img.shields.io/badge/MinIO-S3-red)
![Docker](https://img.shields.io/badge/Docker-Compose-blue)


# Data Lakehouse Pipeline — MongoDB → MinIO → Delta Lake

!!! info "Objetivo do Projeto"
    Este projeto foi desenvolvido como prática de **Engenharia de Dados**, focando em uma arquitetura moderna para extração, armazenamento e governança de dados. O fluxo principal realiza a ingestão de um banco NoSQL para um Object Storage, finalizando com a estruturação em tabelas Delta.

---

##  Visão Geral

A arquitetura utiliza o conceito de **Data Lakehouse**, permitindo transações ACID e versionamento de dados em um ambiente local escalável.

*   **Extração**: Dados originados de coleções do **MongoDB**.
*   **Storage**: Persistência inicial em **MinIO** (S3-Compatible).
*   **Processamento**: Transformação e conversão via **Apache Spark**.
*   **Governança**: Tabelas estruturadas com **Delta Lake**.
*   **Infraestrutura**: Orquestração via **Docker Compose**.

---

## Fluxo de Dados

O pipeline segue etapas rigorosas para garantir a integridade da informação:

1.  **Ingestão**: Conexão com o MongoDB e extração de 4 coleções de vendas.
2.  **Landing Zone**: Salvamento dos dados brutos em formato `JSON` no MinIO.
3.  **Bronze Layer**: Conversão dos arquivos JSON para tabelas `Delta`, garantindo tipagem correta.
4.  **DML Operations**: Realização de testes de manipulação com `INSERT`, `UPDATE` e `DELETE`.
5.  **Data Governance**: Auditoria de dados utilizando `HISTORY` e `TIME TRAVEL`.

---

## Arquitetura do Projeto

A arquitetura do projeto segue o modelo de **Landing Zone → Bronze Layer**, utilizando MongoDB como origem dos dados, MinIO como Data Lake e Delta Lake para persistência transacional.

| Etapa | Origem / Destino | Descrição |
|---|---|---|
| Notebook 00 | MongoDB Local | Criação da base `VendasArCondicionado` com 4 coleções e carga inicial de dados fictícios |
| Notebook 01 | MongoDB → MinIO | Extração das coleções e armazenamento em JSON no bucket `landing-zone` |
| Notebooks 02/03 | MinIO → Delta Lake | Conversão dos JSONs em tabelas Delta no bucket `bronze`, com operações DML e Time Travel |

---

### Fluxo da Pipeline

| Etapa | Origem / Destino | Detalhes |
|---|---|---|
| 1 | MongoDB Local | Base `VendasArCondicionado` com 4 coleções de dados |
| 2 | MinIO (S3) → `landing-zone/` | Armazenamento dos arquivos JSON extraídos do MongoDB |
| 3 | MinIO (S3) → `bronze/` | Conversão dos JSONs em tabelas Delta Lake |
| 4 | Delta Lake | Operações `INSERT`, `UPDATE`, `DELETE`, `HISTORY` e `TIME TRAVEL` |

---

### Responsabilidade dos Notebooks

| Notebook | Responsabilidade |
|---|---|
| Notebook 00 | Setup inicial e geração de dados fictícios |
| Notebook 01 | Extração MongoDB → MinIO (JSON) |
| Notebook 02 | Conversão JSON → Delta Lake |
| Notebook 03 | Operações DML, versionamento e Time Travel |


## Tecnologias Utilizadas

| Tecnologia | Versão | Função |
| :--- | :--- | :--- |
| **Apache Spark** | 3.5.3 | Motor de processamento distribuído. |
| **Delta Lake** | 3.2.0 | Armazenamento com transações ACID. |
| **MinIO** | Latest | Object Storage compatível com S3. |
| **MongoDB** | Latest | Banco de dados NoSQL. |
| **Docker Compose** | V2 | Orquestração da infraestrutura. |


## Resultados e Conceitos
!!! success "Resultados Alcançados"
- Pipeline Funcional: 4 coleções processadas com sucesso.
- Consistência: JSONs persistidos na Landing Zone.
- Evolução: Conversão para Delta Lake concluída.
- Versionamento: Histórico de dados validado.

## Conceitos Demonstrados

- **Arquitetura Medalhão**  
  Transição eficiente entre Landing Zone e Bronze Layer.

- **Tratamento de Dados**  
  Conversão de `ObjectId` para `String`.

- **Transações ACID**  
  Garantia de integridade nas escritas.

- **Time Travel**  
  Restauração de estados anteriores da base.

## Próximos Passos

-  **Camada Silver**  
  Implementar limpeza, padronização e regras de qualidade dos dados.

-  **Orquestração com Apache Airflow**  
  Automatizar o pipeline completo utilizando workflows agendados e monitorados.

---

## Autores

| Nome | Instituição | GitHub |
|---|---|---|
| Gustavo de Freitas Cardoso | Engenharia de Software — SATC | [perfil](https://github.com/GustavodeFreitasCardoso) |
| João Miguel Rita | Engenharia de Software — SATC | [perfil](https://github.com/JoaoMiguelRita) |



