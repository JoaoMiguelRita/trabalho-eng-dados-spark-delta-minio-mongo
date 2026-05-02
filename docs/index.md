# Projeto Apache Spark + Delta Lake + MinIO + MongoDB

## Visão Geral

Este projeto foi desenvolvido para prática de **Engenharia de Dados**, utilizando uma arquitetura moderna com:

- Apache Spark
- Delta Lake
- MinIO
- MongoDB
- Docker Compose

O objetivo principal é simular um pipeline real de dados, realizando a extração de informações de um banco NoSQL, persistência em Object Storage e transformação para tabelas Delta Lake na camada Bronze.

---

## Fluxo do Projeto

### Etapas principais

1. Extração de coleções do MongoDB
2. Persistência dos dados em JSON no MinIO (landing-zone)
3. Conversão dos arquivos JSON para tabelas Delta Lake (bronze)
4. Manipulação com comandos DML:
   - INSERT
   - UPDATE
   - DELETE
5. Exploração de recursos como:
   - HISTORY
   - TIME TRAVEL

---

## Arquitetura

```text
┌─────────────────┐     ┌──────────────────┐     ┌───────────────────┐
│     MongoDB     │────▶│   MinIO (S3)     │────▶│   MinIO (S3)      │
│      Local      │     │   landing-zone/  │     │   bronze/         │
│                 │     │   (JSONs)        │     │   (Delta Tables)  │
│ VendasArCondic. │     │                  │     │                   │
│   4 Coleções    │     │  1 JSON/coleção  │     │ INSERT/UPDATE     │
│                 │     │                  │     │ DELETE/HISTORY    │
└─────────────────┘     └──────────────────┘     └───────────────────┘


Tecnologias Utilizadas
Apache Spark 3.5.3

Motor de processamento distribuído para transformação e manipulação de dados.

Delta Lake 3.2.0

Formato de armazenamento com suporte a transações ACID e versionamento.

MinIO

Object Storage local compatível com Amazon S3.

MongoDB

Banco de dados NoSQL orientado a documentos.

Docker Compose

Responsável pela orquestração da infraestrutura local.

Resultados Obtidos
4 coleções extraídas com sucesso do MongoDB
JSONs persistidos no bucket landing-zone
Conversão concluída para Delta Lake
Operações INSERT, UPDATE e DELETE executadas com sucesso
Histórico de versões validado com Delta HISTORY
Recuperação de versões anteriores com TIME TRAVEL
Conceitos Demonstrados
Engenharia de Dados aplicada
Arquitetura Medalhão (Landing → Bronze)
Object Storage como Data Lake
Conversão de tipos (ObjectId → String)
Processamento distribuído com Spark
Versionamento e governança com Delta Lake
Transações ACID em Data Lakes
Próximos Passos
Camada Silver

Adicionar uma nova camada com:

limpeza de dados
padronização
joins
regras de qualidade
Orquestração com Airflow

Automatizar o pipeline com Apache Airflow para elevar o projeto a um nível mais próximo do mercado.

Autores

Gustavo de Freitas Cardoso
Engenharia da Computação — SATC
GitHub: https://github.com/GustavodeFreitasCardoso

João Miguel Rita
Engenharia da Computação — SATC
GitHub: https://github.com/JoaoMiguelRita