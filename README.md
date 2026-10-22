# Pipeline de Datos Financieros en Tiempo Real

Proyecto personal de aprendizaje en ingeniería de datos. El objetivo es practicar la obtención, transmisión, almacenamiento y transformación de cotizaciones financieras utilizando diferentes herramientas del ecosistema de datos.

Para desarrollarlo, tomé como referencia el tutorial [real-time-stocks-mds](https://github.com/Jay61616/real-time-stocks-mds), adaptándolo a mi entorno de trabajo y explorando las herramientas utilizadas en el proceso.

## Descripción del proyecto

El proyecto integra Python, Apache Kafka, MinIO, Apache Airflow, Snowflake y dbt para construir un flujo de datos financieros.

El proceso contempla las siguientes etapas:

1. **Obtención:** consulta de cotizaciones financieras mediante la API de Finnhub.
2. **Transmisión:** envío de los datos a Apache Kafka mediante un productor desarrollado en Python.
3. **Almacenamiento:** consumo de los mensajes y almacenamiento de archivos JSON en MinIO.
4. **Orquestación y carga:** utilización de un DAG de Airflow para transferir datos desde MinIO hacia Snowflake.
5. **Transformación:** organización de modelos SQL en dbt mediante las capas Bronze, Silver y Gold.

## Arquitectura

```text
Finnhub API
     |
     v
Python Producer
     |
     v
Apache Kafka
     |
     v
Python Consumer
     |
     v
MinIO (archivos JSON)
     |
     v
Apache Airflow
     |
     v
Snowflake
     |
     v
dbt
Bronze -> Silver -> Gold
```

*Nota: el diagrama representa el flujo previsto del proyecto. La ejecución completa de cada etapa depende de la configuración de los servicios y sus conexiones.*

## Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Python | Scripts para obtener y consumir datos. |
| Finnhub API | Fuente de cotizaciones financieras. |
| Apache Kafka | Transmisión de mensajes. |
| MinIO | Almacenamiento de archivos JSON. |
| Apache Airflow | Orquestación de tareas mediante DAGs. |
| Snowflake | Almacenamiento y consulta de datos. |
| dbt | Organización y ejecución de transformaciones SQL. |
| Docker Compose | Configuración y ejecución de los servicios. |

## Estructura del proyecto

```text
.
├── capturas/
├── dbt_stocks/
│   └── dbt_stocks/
│       ├── dbt_project.yml
│       └── models/
│           ├── bronze/
│           │   ├── bronze_stg_stock_quotes.sql
│           │   └── sources.yml
│           ├── silver/
│           │   └── silver_clean_stock_quotes.sql
│           ├── gold/
│           │   ├── gold_candlestick.sql
│           │   ├── gold_kpi.sql
│           │   └── gold_treechart.sql
│           └── example/
├── infra/
│   ├── docker-compose.yml
│   ├── producer/
│   │   └── producer.py
│   ├── consumer/
│   │   └── consumer.py
│   └── dags/
│       └── minio_to_snowflake.py
├── .gitignore
└── requisitos.txt
```

### Organización de los modelos dbt

- **Bronze:** contiene el modelo `bronze_stg_stock_quotes.sql`, relacionado con la preparación inicial de las cotizaciones.
- **Silver:** contiene `silver_clean_stock_quotes.sql`, destinado a la limpieza de los datos.
- **Gold:** incluye modelos SQL para preparar datos orientados a velas financieras (`gold_candlestick.sql`), indicadores (`gold_kpi.sql`) y una visualización tipo *tree chart* (`gold_treechart.sql`).

## Aprendizajes

Con este proyecto estoy practicando:

- Consumo de datos desde una API.
- Envío y recepción de mensajes mediante Kafka.
- Almacenamiento de datos en formato JSON.
- Configuración de servicios con Docker Compose.
- Orquestación de tareas con Airflow.
- Conexión y carga de datos hacia Snowflake.
- Organización de transformaciones SQL con dbt.
- Estructuración de un pipeline de datos con diferentes herramientas.

## Ejecución

Para ejecutar el proyecto se requiere Docker Desktop, las dependencias de Python y las credenciales de los servicios externos.

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/CrhistianZahir/Ingenier-a-de-Datos-Pipeline-rea-financiera.git
   ```

2. Entrar en el directorio:

   ```bash
   cd Ingenier-a-de-Datos-Pipeline-rea-financiera
   ```

3. Revisar `infra/docker-compose.yml` y configurar las variables y credenciales necesarias sin incluir secretos en el repositorio.

4. Iniciar los servicios:

   ```bash
   docker compose -f infra/docker-compose.yml up -d
   ```

5. Configurar las conexiones necesarias para la API, Kafka, MinIO y Snowflake.

6. Ejecutar el productor y verificar la recepción de mensajes por el consumidor.

7. Configurar y ejecutar el DAG de Airflow y, posteriormente, ejecutar los modelos dbt según la configuración del proyecto.

*Los pasos pueden necesitar ajustes según la configuración local. Esta documentación no garantiza que el proyecto pueda ejecutarse directamente sin configuración adicional.*

## Capturas

La carpeta `capturas/` está destinada a documentar visualmente el funcionamiento del proyecto, por ejemplo, los servicios en Docker, los mensajes de Kafka, los archivos almacenados en MinIO o las ejecuciones de Airflow.

## Referencia

Proyecto utilizado como guía de aprendizaje:

[Jay61616/real-time-stocks-mds](https://github.com/Jay61616/real-time-stocks-mds)

Este repositorio tiene fines educativos y forma parte de mi proceso de aprendizaje y práctica en ingeniería de datos.

