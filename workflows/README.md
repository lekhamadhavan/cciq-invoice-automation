## Linux setup
Our working directory:
cd /home/Anjali/Desktop/CCIQ/invoice/cciq_n8n/

### setting script
open vim or nano and name the file docker-compose.yaml

press i to go into insert mode
---------
paste the following code:
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    container_name: n8n
    restart: always

    ports:
      - "5678:5678"

    environment:
      GENERIC_TIMEZONE: Asia/Riyadh
      TZ: Asia/Riyadh
      N8N_SECURE_COOKIE: false

      # Optional MySQL configuration for n8n
      DB_TYPE: mysqldb
      DB_MYSQLDB_HOST: mysql-n8n
      DB_MYSQLDB_PORT: 3306
      DB_MYSQLDB_DATABASE: invoice
      DB_MYSQLDB_USER: admin
      DB_MYSQLDB_PASSWORD: admin

    volumes:
      - .n8n:/home/node/.n8n
      - /home/anjali/Desktop/CCIQ/invoice/cciq_n8n:/home/node/.n8n-files

    depends_on:
      - mysql-n8n

  mysql-n8n:
    image: mysql:8.0
    container_name: mysql-n8n
    restart: always

    ports:
      - "3307:3306"

    environment:
      MYSQL_ROOT_PASSWORD: root_anju
      MYSQL_DATABASE: invoice
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin

    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
---------

Press esc. Then press :wq to save and exit.

### Run the following commands to start the containers:

Start env (n8n & mysql):
docker compose up -d
 
Stop env:
docker compose down
 
Check status:
docker compose ps
 
Check logs:
docker compose logs

Add the name or Id of your container at the end and only that container will be affected by the command.
For example, instead of 'docker compose down', if we give 'docker compose down n8n', it will only stop n8n, Not the mysql container.

## Windows setup

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/) on your Windows machine.
2. Open Docker Desktop.
3. Click on the search bar at the top
4. Search for Type: n8nio/n8n
5. Click on the official n8nio/n8n image
6. Click Pull to download the latest image
7. Configure container settings:
    - Container Name: n8n-local
    - Port Mapping:
        Host Port: 5678
        Container Port: 5678
    - Volume Mapping:
        Volume: n8n-data
        Container Path: /home/node/.n8n-files
    - Click run to start the container
8. Open your browser and go to http://localhost:5678

Once n8n is up and running do the rest of the setup instructions

## Credentails for n8n flow

1. Quickbooks Online OAuth2 API
    - Client ID
    - Client Secret
Test connection

2. MySQL account
    - Host
    - Port
    - Database
    - Username
    - Password

3. JWT Auth
    - Secret
to generate secret use 
```bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

4. n8n API
    - API Key
We get n8n API key from n8n UI.
- Go to settings -> n8n API -> Create an API Key
- Copy and paste the created api key.

## Flow

- Change the company id in HTTP Request depending on the quickbooks account you are using.

