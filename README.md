# install docker and docker compose if not installed
#create a docker-compose.yml 
replace the default user and password credentials

'''
version: '3.8'

services:
  n8n_postgres:
    image: postgres:15
    container_name: n8n_postgres
    restart: always
    environment:
      POSTGRES_USER: replacewith_your_username
      POSTGRES_PASSWORD:replacewith_your_password
      POSTGRES_DB: n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data

  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=n8n_postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=replacewith_your_username
      - DB_POSTGRESDB_PASSWORD=replacewith_your_password
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=replacewith_your_username
      - N8N_BASIC_AUTH_PASSWORD=replacewith_your_password
      - N8N_ENCRYPTION_KEY=replace_this_with_a_very_strong_key
      - GENERIC_TIMEZONE=UTC
    depends_on:
      - n8n_postgres
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  postgres_data:
  n8n_data:

'''
#start docker
sudo docker-compose up -d
