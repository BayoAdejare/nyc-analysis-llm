# DataInsight: NYC Taxi Data Analysis with LLM Integration

## Overview

DataInsight is a two-part data engineering and analysis project that processes NYC Taxi trip data and provides an intelligent interface for data exploration. It demonstrates the use of modern data engineering tools and natural language processing techniques.

### Part 1: Data Ingestion and Storage
- Fetches real-time NYC Taxi trip data from an API
- Processes and stores the data in a PostgreSQL database
- Uses Kafka for data streaming and Spark for data processing

### Part 2: LLM-Powered Data Exploration
- Develops an application that uses a Language Model (LLM) to interact with the stored data
- Allows users to query and analyze the data using natural language

### Tech Stack

- Apache Kafka: For real-time data ingestion
- Apache Spark: For data processing
- Prefect: For workflow orchestration
- PostgreSQL: For data storage
- Docker: For containerization and easy deployment
- Python: Primary programming language
- Hugging Face Transformers: For the language model integration

### Data Source

This project uses the [NYC Taxi & Limousine Commission Trip Record Data API](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). We'll be working with the Yellow Taxi Trip Records, which provide information about pick-up and drop-off dates/times, locations, trip distances, fares, rate types, payment types, and driver-reported passenger counts.

## Project Structure

```
datainsight/
│
├── data_ingestion/
│   ├── src/
│   │   ├── producers/
│   │   │   └── taxi_trip_producer.py
│   │   ├── consumers/
│   │   │   └── taxi_trip_consumer.py
│   │   ├── transformers/
│   │   │   └── trip_transformer.py
│   │   └── loaders/
│   │       └── postgres_loader.py
│   ├── workflows/
│   │   └── etl_flow.py
│   └── docker/
│       ├── Dockerfile.kafka
│       ├── Dockerfile.spark
│       ├── Dockerfile.prefect
│       └── Dockerfile.postgres
│
├── llm_interface/
│   ├── src/
│   │   ├── app.py
│   │   ├── database.py
│   │   └── llm_query_engine.py
│   ├── models/
│   │   └── fine_tuned_model/
│   └── docker/
│       └── Dockerfile.llm_app
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Setup and Installation

1. Clone the repository:
   ```
   git clone https://github.com/yourusername/datainsight.git
   cd datainsight
   ```

2. Build and start the Docker containers:
   ```
   docker-compose up -d
   ```

3. Initialize the Prefect server (for Part 1):
   ```
   docker-compose run prefect server create-tenant --name default
   docker-compose run prefect server start
   ```

## Part 1: Running the Data Ingestion Pipeline

1. Register the Prefect flow:
   ```
   docker-compose run prefect flow register /app/part1_data_ingestion/workflows/etl_flow.py
   ```

2. Start the Prefect agent to run the flow:
   ```
   docker-compose run prefect agent start
   ```

3. Trigger the Prefect flow:
   ```
   docker-compose run prefect flow run datainsight-etl
   ```

## Part 2: Using the LLM-Powered Data Exploration App

1. Ensure the Docker container for the LLM app is running:
   ```
   docker-compose up -d llm_app
   ```

2. Access the application at `http://localhost:8501` in your web browser.

3. Use natural language to query and analyze the NYC Taxi data. For example:
   - "Show me the average fare amount for trips in Manhattan last month."
   - "What's the busiest time for taxi pickups on weekends?"
   - "Compare the trip distances between weekdays and weekends."

## Data Flow

### Part 1: Data Ingestion and Storage
1. The Kafka producer fetches NYC Taxi trip data from the API and publishes it to a Kafka topic.
2. The Spark consumer subscribes to the Kafka topic and processes the incoming data in real-time.
3. The trip transformer applies necessary transformations and aggregations to the data.
4. The PostgreSQL loader stores the processed data in the database.
5. Prefect orchestrates the entire workflow, ensuring proper execution and error handling.

### Part 2: LLM-Powered Data Exploration
1. The user interacts with the web application, entering natural language queries.
2. The LLM query engine interprets the user's query and translates it into SQL.
3. The application executes the SQL query against the PostgreSQL database.
4. The results are processed and presented to the user in a readable format.

## Monitoring and Visualization

- Access the Prefect UI at `http://localhost:8080` to monitor the data ingestion workflow.
- Use the web application at `http://localhost:8501` for data exploration and visualization.

## Future Enhancements

- Implement more advanced natural language understanding capabilities.
- Add support for generating visualizations based on user queries.
- Extend the system to handle multiple data sources and domains.
- Implement user authentication and query history tracking.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
