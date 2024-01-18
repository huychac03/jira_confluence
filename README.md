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


# Note & How to fix Bugs:
1. Fix bug can not login by root user after import data:
    Troubleshooting:
    - https://confluence.atlassian.com/doc/restore-passwords-to-recover-admin-user-rights-158390.html
    - https://confluence.atlassian.com/doc/configuring-system-properties-168002854.html

2. Fix bug about "Collaborative":
    Troubleshooting: 
    - Turn on Collaborative editing:  https://confluence.atlassian.com/doc/troubleshooting-collaborative-editing-858772087.html
    - If Collaborative editing is already enabled, but the Synchrony system has problems: 
        - Restart the Synchrony system
        - Check port 8091 is available
        - Reverse proxy issues
        - And some other issues....
        All using this documentation to fix: https://confluence.atlassian.com/doc/troubleshooting-collaborative-editing-858772087.html
        If you do everything but still got the error: Consider recreating the Confluence Container!!!

3. Fix bug Unable to Export in Excel:
    Troubleshooting: 
    - https://confluence.atlassian.com/jirakb/unable-to-export-in-excel-missing-excel-all-fields-excel-current-fields-options-1141973487.html
    OR:
    - Find jira-config.properties file and then add "jira.export.excel.enabled" in to this file.
    - Note:
        - From JIRA 7.2 export in EXCEL has been removed but, if you want you can unlock that feature as follow :
            Stop JIRA.
            Locate the jira-config.properties file in the $JIRA_HOME directory. If the file does not exist, please proceed to create it.
            Open the file and add the below on a separate line:
            jira.export.excel.enabled=true
            Save this file.
            Restart JIRA.

4. Cann't find jira-config.properties file:
    Troubleshooting: 
    - This file is located in the Jira application home directory. 
    - In new Jira installations, this file may not initially exist and if so, it needs to be created manually in that directory.

