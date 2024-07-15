<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Custom ELT Project</title>
</head>
<body>
    <h1>Custom ELT Project</h1>
    <p>This repository showcases a custom Extract, Load, Transform (ELT) project that leverages Docker, Airbyte, Airflow, dbt, and PostgreSQL to illustrate a straightforward ELT process.</p>
    <h2>Repository Layout</h2>
    <ol>
        <li>
            <strong>docker-compose.yaml</strong>: This file configures Docker Compose to manage multiple Docker containers, defining several services:
            <ul>
                <li><code>source_postgres</code>: The source PostgreSQL database operating on port 5433.</li>
                <li><code>destination_postgres</code>: The destination PostgreSQL database operating on port 5434.</li>
                <li><code>postgres</code>: The PostgreSQL database for storing Airflow metadata.</li>
                <li><code>webserver</code>: The Airflow Web UI.</li>
                <li><code>scheduler</code>: Airflow's task scheduler.</li>
            </ul>
        </li>
        <li>
            <strong>airflow</strong>: This directory contains the Airflow project, including DAGs to orchestrate the ELT workflow with Airbyte and dbt.
        </li>
        <li>
            <strong>postgres_transformations</strong>: This directory includes the dbt project with custom models to be written into the destination database.
        </li>
        <li>
            <strong>source_db_init/init.sql</strong>: This SQL script initializes the source database with sample data, creating tables for users, films, film categories, actors, and film actors, and populating them with sample data.
        </li>
    </ol>
    <h2>How It Operates</h2>
    <ol>
        <li>
            <strong>Docker Compose</strong>: Using the <code>docker-compose.yaml</code> file, several Docker containers are launched:
            <ul>
                <li>A source PostgreSQL database populated with sample data.</li>
                <li>A destination PostgreSQL database.</li>
                <li>A PostgreSQL database for storing Airflow metadata.</li>
                <li>The Airflow webserver for UI access.</li>
                <li>The Airflow scheduler.</li>
            </ul>
        </li>
        <li>
            <strong>ELT Process</strong>: In Airbyte, configure the source and destination database details to establish a connection. Obtain the connection ID from the Airbyte UI, insert it into the <code>elt_dag.py</code> file, and run Airflow to synchronize the databases. Once synchronization is complete, Airflow will trigger dbt to perform transformations on the destination database.
        </li>
        <li>
            <strong>Database Initialization</strong>: The <code>init.sql</code> script sets up the source database with sample data by creating several tables and inserting sample records.
        </li>
    </ol>
    <h2>Getting Started</h2>
    <ol>
        <li>Ensure Docker and Docker Compose are installed on your machine.</li>
        <li>Clone this repository.</li>
        <li>Navigate to the repository directory and execute <code>./start.sh</code>.</li>
        <li>Once all containers are running, access the Airbyte UI at <a href="http://localhost:8000">http://localhost:8000</a> and the Airflow UI at <a href="http://localhost:8080">http://localhost:8080</a>.</li>
        <li>In Airbyte, input the details for both the source and destination databases (details can be found in the <code>docker-compose.yaml</code> file in the root directory).</li>
        <li>Copy the Connection ID after creating the connection and paste it into the <code>CONN_ID</code> variable in the <code>elt_dag.py</code> file located in the <code>airflow</code> directory.
            <ul>
                <li><strong>Note</strong>: You may need to restart the Docker containers running Airflow and dbt to propagate the new Connection ID.</li>
            </ul>
        </li>
        <li>Once configured, navigate to Airflow, run the DAG, and observe the orchestration of the ELT process!</li>
    </ol>
    <h2>Setting Up Airbyte</h2>
    <ol>
        <li>Clone the Airbyte repository:</li>
        <pre><code>git clone https://github.com/airbytehq/airbyte.git
cd airbyte</code></pre>
        <li>If starting Airbyte for the first time, run this command:</li>
        <pre><code>./run-ab-platform.sh</code></pre>
        <li>Start Airbyte using Docker:</li>
        <pre><code>docker-compose up</code></pre>
        <li>Access the Airbyte dashboard at <a href="http://localhost:8000">http://localhost:8000</a>.</li>
    </ol>
</body>
</html>
