# Docker Compose

---

Docker Compose is a tool for **defining and running multi-container Docker applications**.  
It allows you to define services, networks, and volumes in a single **`docker-compose.yml`** file.


## Why Use Docker Compose?

- **Manage multiple containers easily** – Define, start, and stop services in one command.
- **Avoid complex `docker run` commands** – Use a single YAML file instead.
- **Enable service orchestration** – Containers can communicate using defined networks.
- **Environment consistency** – Works the same across development and production.


## `docker-compose.yml` File Structure

A `docker-compose.yml` file defines services, networks, and volumes in YAML format.

Example:

```yaml
version: '3.8'  # Compose file format version  

services:  
  app:  
    image: myapp:v1  # Use an existing image  
    ports:  
      - "8080:80"  # Expose container port 80 on host port 8080  
    depends_on:  
      - db  # Ensure the database starts first  

  db:  
    image: mysql:latest  
    environment:  
      MYSQL_ROOT_PASSWORD: secret  
      MYSQL_DATABASE: mydb  
    volumes:  
      - db_data:/var/lib/mysql  # Persist database data  

volumes:  
  db_data:  # Define a named volume  
```  

---

## Docker Compose Commands

### Start and Stop Services

| Command | Description |
|---------|-------------|
| `docker-compose up` | Start all services |
| `docker-compose up -d` | Start in **detached mode** (background) |
| `docker-compose down` | Stop and remove containers, networks, and volumes |
| `docker-compose restart` | Restart all services |

### Managing Containers

| Command | Description |
|---------|-------------|
| `docker-compose ps` | List running services |
| `docker-compose logs` | Show logs for all services |
| `docker-compose logs -f {service}` | Tail logs for a specific service |
| `docker-compose exec {service} {command}` | Run a command inside a running container |
| `docker-compose stop {service}` | Stop a specific service |

### Building and Rebuilding

| Command | Description |
|---------|-------------|
| `docker-compose build` | Build images defined in `docker-compose.yml` |
| `docker-compose up --build` | Build and start services |
| `docker-compose pull` | Pull latest images from registry |

---

## Defining Services in `docker-compose.yml`

### Using Build Context
Instead of specifying an image, you can **build from a Dockerfile**:

```yaml
services:  
  app:  
    build: .  
    ports:  
      - "5000:5000"  
```

---

## Environment Variables
Define environment variables inline or from an **`.env` file**:

```yaml
services:  
  app:  
    environment:  
      - APP_ENV=production  
      - SECRET_KEY=mysecret  
```

Using an `.env` file:

```yaml
services:  
  app:  
    env_file:  
      - .env  
``` 

`.env` file:  
```ini
APP_ENV=production
SECRET_KEY=mysecret
```
