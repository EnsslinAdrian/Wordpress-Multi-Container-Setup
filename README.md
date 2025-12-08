# Wordpress-Multi-Container-Setup
A simple Docker Compose project that runs WordPress and a MariaDB database as a multi-container setup.


## Table of contents
1. [Requirements](#requirements)
2. [Quickstart](#quickstart)
3. [Project Structure](#project-structure)
4. [Usage](#usage)
5. [Docker commands](#docker-commands)


## Requirements
Before running this project, ensure you have:

- **Docker** installed  
- **Docker Compose** installed 


## Quickstart
Clone the repository from GitHub
```bash
git clone git@github.com:EnsslinAdrian/Wordpress-Multi-Container-Setup.git
```

Navigate to the folder
```bash
cd Wordpress-Multi-Container-Setup
```

Create a `.env` file using this command and fill in the variables:

```bash
cp .env.template .env
```

Run the server directly:
```bash
docker compose up -d
```

Access this URL and create your account:
```
localhost:8000
```


## Project Structure
```
|-- .env
|-- .env.template
|-- gitignore
|-- docker-compose.yml
|-- README.md
```

## Usage
This project is designed to run multiple services in the same network.
For this purpose, the docker-compose.yml file was created and configured.

> The first service creates an image for WordPress that I install with the latest version.
```yml
wordpress:
    image: wordpress:latest # Use the latest WordPress image
    restart: on-failure:5 # Restart the container automatically on failure, up to 5 times
    depends_on: # Ensure the database starts before WordPress
      - db
    ports:
      - "8000:80"
    networks: # Connect to the custom network
      - wp-net
    environment:
      WORDPRESS_DB_HOST: ${WORDPRESS_DB_HOST}
      WORDPRESS_DB_USER: ${WORDPRESS_DB_USER}
      WORDPRESS_DB_PASSWORD: ${WORDPRESS_DB_PASSWORD}
      WORDPRESS_DB_NAME: ${WORDPRESS_DB_NAME}
    volumes:
      - wordpress_data:/var/www/html # Persist WordPress data
```
### Configuration 
- restart: restart on failure up to 5 times
- depends_on: wait for the database to start first
- ports: mapped to port 8000 from 80
- networks: connection to the custom network
- environment: login variables are loaded from the .env file
- volumes: persistently stored in the container at /var/www/html

<br>

> The second service uses the MySQL image that I install with version 8.0
```yml
  db:
    image: mysql:8.0 
    restart: on-failure:5 # Restart the container automatically on failure, up to 5 times
    networks: # Connect to the custom network
      - wp-net
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_RANDOM_ROOT_PASSWORD: ${MYSQL_RANDOM_ROOT_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql # Persist database data
```

### Configuration
- restart: restart on failure up to 5 times
- networks: connection to the custom network
- environment: login variables are loaded from the .env file
- volumes: persistently stored in the container at /var/lib/mysql


## Docker commands
Start project
```bash
docker compose up -d
```
Stop containers
```bash
docker compose down
```
Remove containers + volumes
```bash
docker compose down -v
```
Restart
```bash
docker compose restart
```
### Useful Docker commands
List running containers
```bash
docker ps
```
View container logs
```bash
docker logs -f <container-name>
```

## Author
Adrian Enßlin