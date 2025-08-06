# Apache Airflow 3.0 com Docker Compose em 3 passos

Este guia rápido e prático demonstra como configurar e executar o Apache Airflow 3.0 localmente usando Docker Compose. Ele inclui os arquivos `docker-compose.yaml` e `.env` necessários para um ambiente completo com PostgreSQL como banco de dados e Redis como broker de mensagens para o CeleryExecutor.

## Pré-requisitos

Antes de começar, certifique-se de ter o Docker e o Docker Compose instalados em sua máquina. Você pode baixá-los e instalá-los a partir do site oficial do Docker:

*   [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Passos para Executar o Airflow

Siga os passos abaixo para subir o ambiente do Airflow:

### 1. Faça um clone deste repositório

Primeiro preciso subir os arquivos deste repositório para sua máquina ou a máquina que deseja instalar o airflow 3.0.
Crie a pasta do projeto airflow na sua máquina, execute no bash:

```bash
mkdir projeto_airflow && cd projeto_airflow/
```

Agora, considerando que você tenha o git instalado em sua máquina, execute o seguinte comando:

```bash
git clone https://github.com/Rafasansouza/airflow3.0-server-docker
```

Assim, terá os arquivos necessarios do projeto.

### 2. Inicializar o Ambiente do Airflow

Antes de iniciar todos os serviços, é necessário inicializar o ambiente do Airflow, o que inclui a criação do banco de dados e do usuário administrador. Execute o comando abaixo no terminal, na mesma pasta onde estão seus arquivos:

```bash
docker compose up airflow-init
```

Este comando pode levar alguns minutos para ser concluído, pois ele baixa as imagens necessárias e configura o banco de dados.

### 3. Iniciar os Serviços do Airflow

Após a inicialização bem-sucedida, você pode iniciar todos os serviços do Airflow em segundo plano com o seguinte comando:

```bash
docker compose up -d
```

Este comando irá subir os containers para o servidor web do Airflow, scheduler, worker, PostgreSQL e Redis.

## Acessando a Interface Web do Airflow

Uma vez que todos os serviços estejam rodando, você pode acessar a interface web do Apache Airflow em seu navegador:

[http://localhost:8080](http://localhost:8080)

As credenciais de login padrão são:

*   **Usuário:** `airflow`
*   **Senha:** `airflow`

## Solução de Problemas Comuns

### `dependency failed to start: container airflow-server-docker-redis-1 is unhealthy`

Este erro geralmente indica que o container do Redis não está iniciando corretamente, muitas vezes devido a dados persistidos corrompidos ou incompatíveis. Para resolver, você pode limpar os volumes do Docker e tentar novamente. **ATENÇÃO: Isso apagará todos os dados do Airflow e do Redis.**

1.  **Parar e remover os containers e volumes existentes:**
    ```bash
    docker compose down -v
    ```

2.  **Tentar inicializar e subir o Airflow novamente:**
    ```bash
    docker compose up airflow-init
    docker compose up -d
    ```

Se o problema persistir, verifique os logs do container do Redis para obter mais informações:

```bash
docker logs airflow-server-docker-redis-1
```

## Parando o Airflow

Para parar todos os serviços do Airflow e remover os containers, execute:

```bash
docker compose down
```

Para parar os serviços e remover também os volumes de dados (limpando completamente o ambiente), use:

```bash
docker compose down -v
```

Espero que este guia seja útil para você começar a usar o Apache Airflow 3.0 com Docker Compose!
