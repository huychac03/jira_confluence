# jira_confluence
Deploy Jira and Confluence Server using Docker compose
In this example, I using Nginx as a Proxy Server 

# Checklist:
1. Modify the server.xml file 
    - We need to modify the server.xml file to fit with the Protocol we use. There I'm using https (port 443)
    - Note: Only open the part that match with ur protocol. And change the `proxyName` to ur domain.

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

6. Connect Confuence to database:
    - To connect Confluence with Postgres, we use connection string option: 
        - JDBC URL:  `jdbc:postgresql://postgres:5432/confluence`
        - db: user: `confluence` / password: `confluence`

7. Connect Jira to database:
    - Hostname: `IP address` (Here, we have to use the `ens5`. We can find it by running command: `ip a` )
    - Port: `5432` (default)
    - Database: `jira`
    - username: `confluence`
    - password: `confluence`
    - schema: `public`


# Note:
1. Fix bug can not login by root user after import data:
    - https://confluence.atlassian.com/doc/restore-passwords-to-recover-admin-user-rights-158390.html
    - https://confluence.atlassian.com/doc/configuring-system-properties-168002854.html
