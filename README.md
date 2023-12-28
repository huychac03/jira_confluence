# jira_confluence
Deploy Jira and Confluence Server using Docker compose

- We need to modify the server.xml file to fit with the Protocol we use. There I'm using https
- After running the docker compose file, we have to exec into Postgres container to create database for Jira (database for Confluence is already created in Docker compose)
    - We have to exec into this container with root privileges by running this command:
        ` docker exec -it -u 0 <container ID> bash `
- To connect Confluence with Postgres, we use connection string option: 
    - JDBC URL:  jdbc:postgresql://postgres:5432/confluence
    - db: login: confluence / password: confluence
- To connect Jira with Postgres, we use 