# Deploying n8n with Docker and PostgreSQL

This guide provides a comprehensive walkthrough for deploying [n8n](https://n8n.io/), a powerful, source-available workflow automation tool, using Docker and Docker Compose. This setup uses a PostgreSQL database for a robust and persistent data backend, which is recommended for production or heavy use.

## Prerequisites

Before you begin, you must have Docker and Docker Compose installed on your system. If you haven't installed them yet, please follow the official documentation:

*   **Install Docker Engine:** [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
*   **Install Docker Compose:** [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

> **Note:** Newer versions of Docker Desktop and Docker Engine include `docker compose` as a native command (without the hyphen). This guide uses the classic `docker-compose` syntax, but you can use `docker compose` if that's what your system supports.

## Installation Steps

### Step 1: Create a Project Directory

It's good practice to keep your configuration files organized. Create a dedicated directory for your n8n setup and navigate into it.

```bash
mkdir n8n-docker
cd n8n-docker
```

### Step 2: Create the `docker-compose.yml` File

Create a file named `docker-compose.yml` in your new directory.

```bash
touch docker-compose.yml
```

Now, copy and paste the following content into the `docker-compose.yml` file.

```yaml
version: '3.8'

services:
  n8n_postgres:
    image: postgres:15
    container_name: n8n_postgres
    restart: always
    environment:
      POSTGRES_USER: replacewith_your_db_user
      POSTGRES_PASSWORD: replacewith_your_db_password
      POSTGRES_DB: n8n
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $$POSTGRES_USER -d $$POSTGRES_DB"]
      interval: 5s
      timeout: 5s
      retries: 10

  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: always
    ports:
      - "127.0.0.1:5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=n8n_postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=replacewith_your_db_user
      - DB_POSTGRESDB_PASSWORD=replacewith_your_db_password
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=replacewith_your_n8n_user
      - N8N_BASIC_AUTH_PASSWORD=replacewith_your_n8n_password
      - N8N_ENCRYPTION_KEY=replace_this_with_a_very_strong_key
      - GENERIC_TIMEZONE=UTC # Replace with your timezone, e.g., 'America/New_York'
    depends_on:
      n8n_postgres:
        condition: service_healthy
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  postgres_data:
  n8n_data:
```

### Step 3: Configure Your Environment Variables

Before launching the services, you **must** replace the placeholder values in your `docker-compose.yml` file.

1.  **Database Credentials:**
    *   Replace `replacewith_your_db_user` and `replacewith_your_db_password` with a secure username and password for your PostgreSQL database.
    *   **IMPORTANT:** You must use the same values in both the `n8n_postgres` and `n8n` service definitions.

2.  **n8n Web UI Credentials:**
    *   Replace `replacewith_your_n8n_user` and `replacewith_your_n8n_password` with the username and password you want to use to log in to the n8n web interface.

3.  **n8n Encryption Key:**
    *   The `N8N_ENCRYPTION_KEY` is critical for securing your workflow credentials. Replace `replace_this_with_a_very_strong_key` with a long, random, and unique string.
    *   You can generate a strong key with the following command:
        ```bash
        # This command generates a 32-byte random hexadecimal string
        openssl rand -hex 32
        ```
    *   **IMPORTANT:** Save this key in a secure location. If you lose it, you will lose access to all credentials stored in your n8n instance.

4.  **Timezone (Optional but Recommended):**
    *   Change `GENERIC_TIMEZONE` from `UTC` to your local timezone (e.g., `Europe/Berlin`, `America/Los_Angeles`). This ensures cron-based workflows trigger at the correct local time. A list of valid timezones can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

### Step 4: Launch the n8n Stack

Once your `docker-compose.yml` is configured, open your terminal in the same directory and run the following command:

```bash
docker-compose up -d
```

*   **`docker-compose up`**: This command reads the `docker-compose.yml` file and starts the services defined within it.
*   **`-d` (Detached Mode)**: This flag runs the containers in the background, so you can close your terminal without stopping them.

> Depending on your Docker setup, you may need to run this command with `sudo`: `sudo docker-compose up -d`.

Docker will now pull the `postgres` and `n8n` images (if they don't exist locally) and then create and start the containers.

## Post-Installation

### Access Your n8n Instance

Wait a minute for the services to initialize. Then, open your web browser and navigate to:

**[http://localhost:5678](http://localhost:5678)**

You will be prompted to log in. Use the `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD` you set in the `docker-compose.yml` file.

### Managing Your Deployment

Here are some useful commands to manage your n8n stack:

*   **Check the status of your containers:**
    ```bash
    docker-compose ps
    ```

*   **View the logs for the n8n container (useful for debugging):**
    ```bash
    docker-compose logs -f n8n
    ```

*   **Stop and remove the containers:**
    ```bash
    docker-compose down
    ```
    *(This will not delete your data, as it is stored in Docker volumes.)*

*   **Update your n8n instance:**
    ```bash
    # Pull the latest images for n8n and postgres
    docker-compose pull

    # Recreate the containers with the new images
    docker-compose up -d
    ```

## Understanding the `docker-compose.yml` Configuration

*   **`services`**: Defines the individual containers that make up your application.
    *   `n8n_postgres`: The PostgreSQL database container.
    *   `n8n`: The main n8n application container.
*   **`image`**: Specifies the Docker image to use for the container (e.g., `postgres:15`).
*   **`restart: always`**: Ensures that Docker automatically restarts the container if it crashes or if the system reboots.
*   **`environment`**: Sets environment variables inside the container, which are used to configure the application (e.g., database credentials, encryption key).
*   **`volumes`**: Persists data outside the container's lifecycle.
    *   `postgres_data:/var/lib/postgresql/data`: Maps the `postgres_data` Docker volume to the directory where PostgreSQL stores its data.
    *   `n8n_data:/home/node/.n8n`: Maps the `n8n_data` Docker volume to the directory where n8n stores its data, including workflows and credentials.
    *   This ensures your database and workflows are safe even if you stop or recreate the containers.
*   **`ports`**: Exposes container ports to the host machine.
    *   `"127.0.0.1:5678:5678"`: Maps port `5678` inside the container to port `5678` on your host machine's `localhost` interface. This means n8n is only accessible from your local machine. If you want to access it from other devices on your network, change it to `"5678:5678"`.
*   **`depends_on`**: Controls the startup order. `n8n` will wait for `n8n_postgres` to be healthy before it starts, preventing connection errors.

## Security Best Practices

*   **Strong Credentials**: Always use strong, unique passwords for both your database and the n8n basic authentication.
*   **Encryption Key**: The `N8N_ENCRYPTION_KEY` is extremely important. **Back it up in a password manager or secure location.** Without it, you cannot recover the credentials stored in n8n.
*   **Firewall**: The provided configuration only exposes n8n to `localhost`. If you expose it to your network (by changing `127.0.0.1:5678:5678` to `5678:5678`), ensure your server is protected by a firewall. For public-facing deployments, it is highly recommended to use a reverse proxy like Nginx or Caddy with SSL/TLS encryption.
