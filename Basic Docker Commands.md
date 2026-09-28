# Basic Docker Commands

The Docker commands from the Docker I and Docker II slides, with the names we use in the course. See the slides for the details.

## Install Docker

| Command | What it does |
|---|---|
| `curl -fsSL https://get.docker.com \| sh` | Installs Docker on the droplet |
| `docker run hello-world` | Checks the installation: prints "Hello from Docker!" |

## Containers

| Command | What it does |
|---|---|
| `docker run -dit --name python -v data:/data astral/uv:python3.14-trixie-slim bash` | Runs the python container in the background, with the volume `data` at `/data` |
| `docker run -d --name dash -p 8050:8050 -e HOST=0.0.0.0 -v app_data:/app astral/uv:python3.14-trixie-slim uv run --with dash --with pandas /app/app.py` | Runs the Dash app on port 8050 of the droplet |
| `docker run -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=ThisIs4ThePassword -e POSTGRES_USER=postgres -d -v postgres_data:/var/lib/postgresql postgres` | Runs the PostgreSQL database on port 5432 (choose your own password) |
| `docker ps` | Lists the running containers |
| `docker ps -a` | Lists all containers, also the stopped ones |
| `docker exec -it python bash` | Opens a terminal in the container (leave it with `exit`) |
| `docker exec python cat /data/volume.txt` | Runs one command in the container without entering it |
| `docker logs dash` | Shows the output of the container |
| `docker stop python` | Stops the container |
| `docker start python` | Starts a stopped container |
| `docker restart python` | Stops and starts the container |
| `docker pause python` | Freezes the container |
| `docker unpause python` | Unfreezes the container |
| `docker rm python` | Removes a stopped container (`docker remove` does the same) |
| `docker rm -f python` | Stops and removes the container |
| `docker help ps` | Shows the help for a command (`docker help` lists all commands) |

## Images

| Command | What it does |
|---|---|
| `docker pull astral/uv:python3.14-trixie-slim` | Downloads an image without creating a container |
| `docker image ls` | Lists the images on the droplet |
| `docker rmi hello-world:latest` | Removes an image: give the tag, and remove its containers first |
| `docker image build --tag python:1.0.0 -f dockerfile_python .` | Builds the image `python:1.0.0` from the Dockerfile `dockerfile_python` in the current folder |
| `docker run -dit --name python --network dvbi_network -v data:/data python:1.0.0 bash` | Runs the python container from your own image, connected to `dvbi_network` |
| `docker image prune` | Removes dangling images (images without a tag) |

## Volumes

| Command | What it does |
|---|---|
| `docker volume create data` | Creates the volume `data` |
| `docker volume ls` | Lists the volumes |
| `chmod o+rw /var/lib/docker/volumes/data/_data` | Lets other users than root read and write in the folder of the volume |

## Networks

| Command | What it does |
|---|---|
| `docker network ls` | Lists the networks |
| `docker network create --driver bridge --attachable --scope local --subnet 10.0.42.0/24 --ip-range 10.0.42.128/25 dvbi_network` | Creates the network `dvbi_network` |
| `docker network connect dvbi_network python` | Connects the container `python` to the network |
| `docker network inspect dvbi_network` | Shows the network, with the IP address of each container |

## Docker Compose

Run these in the folder with `compose.yaml` (`cd ~/docker_learn`).

| Command | What it does |
|---|---|
| `docker compose up -d` | Creates and starts all the containers in `compose.yaml` |
| `docker compose ps` | Lists the containers of the compose file |
| `docker compose logs postgres` | Shows the output of a service |
| `docker compose down` | Stops and removes the containers of the compose file |
