 ## EFETUAR OS COMANDOS OBRIGATÓRIOS:

 - Sincronizar o uv local com as dependencia que estão nesse projeto
 ```bash
  uv sync
```

- Subir o container que já está configurado no `docker-compose`
 ```bash
  docker compose up -d
```

Continuação do trabalho

após rodar o notebook '01_mongodb_to_minio_json.ipynb': http://localhost:9021

no arquivo '02_json_to_delta.ipynb' o comando 2. Criar SparkSession com Delta Lake e MinIO pode ser que leve uns 2 minutos para baixar as dependencias de conexao da aws
