# jira_confluence
Deploy Jira and Confluence Server using Docker compose

# Checklist:
1. Modify the server.xml file 
    - We need to modify the server.xml file to fit with the Protocol we use. There I'm using https (port 443)
2. Create network:
    - `docker network create jira-confluence-network`
3. Create 3 volumes:
    - ` docker volume create confluence_home_data`
    - ` docker volume create jira_home_data `
    - ` docker volume create postgresql`
4. Running the compose file:
    - ` docker compose up -d `
5. Create database:
    - After running the docker compose file, we have to exec into Postgres container to create database for Jira (database for Confluence is already created in Docker compose)
    - We have to exec into this container with root privileges by running this command:
        - ` docker exec -it -u 0 <container ID> bash `
    - Login using 'confluence' user:
        - `psql -U confluence`
    - Create Jira db:
        - ` CREATE DATABASE jira; `

- To connect Confluence with Postgres, we use connection string option: 
    - JDBC URL:  jdbc:postgresql://postgres:5432/confluence
    - db: login: confluence / password: confluence
- To connect Jira with Postgres, we use 