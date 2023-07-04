# Introdução ao Airflow

Arquivos e materiais baseados no curso de Marc Lambert no Udemy: The Complete Hands-On Introduction to Apache Airflow

Vou registrar aqui alguns pontos importantes do curso para caso eu precise consultar depois.

A estrutura de pastas foi criada pelo próprio docker compose.

## Estrutura básica de uma DAG
Uma DAG é um grupo de tasks programadas para serem executas de formas sequenciais ou não. Por definição, elas não podem ser cíclicas.

Uma DAG possui algumas características básicas:
- id: identificador único daquela dag;
- start_date: data de início daquela dag, quando começará a ser executada;
- schedule_interval: com que frequência essa dag deve ser executada. Há marcadores especiais para facilitar a descrição da frequência.

Exemplo:
```
with DAG('user_processing', start_date = datetime(2023, 4, 1),
         schedule_interval='@daily', catchup = False) as dag:
    
    create_table = PostgresOperator(
        task_id='create_table',
        postgres_conn_id = 'postgres',
        sql = '''
            CREATE TABLE IF NOT EXISTS users (
                firstname TEXT NOT NULL,
                lastname TEXT NOT NULL,
                country TEXT NOT NULL,
                username TEXT NOT NULL,
                password TEXT NOT NULL,
                email TEXT NOT NULL
            );
        '''
    )
```

## Testando uma task criada
Para verificar se uma task está funcionando corretamente, podemos testá-la através do terminal  acessando o local onde o scheduler atua, dentro do container. Para isso, no terminal do VScode, digite o comando:

`docker exec -it {nome da sua pasta}_airflow_scheduler_1 /bin/bash`

Agora os comandos feitos no terminal serão executados pelo container. Para testar sua task:

`airflow tasks test user_processing create_table 2023-01-01`

Nesse caso `user_processing` é o nome da minha dag e `create_table` é o nome da minha task. A data deve ser uma data no passado.

### Extraindo arquivo config do airflow
Para extrair o arquivo do docker, você precisa extrair o arquivo do scheduler_1.
Para isso use o comando `docker ps` no terminal para saber qual container termina com o nome `scheduler_1`. Em seguida digite no terminal:

`docker cp hands-on-airflow-introduction_airflow-scheduler_1:/opt/airflow/airflow.cfg .`

Esse comando irá copiar o arquivo `airflow.cfg` para sua pasta raiz.


