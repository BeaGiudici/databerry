#software 
## Installation
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Check that the installation works with a simple hello-world image.
```
sudo docker run hello-world
```

## Image installed on databerry
- [[boinc]]
- [[pihole]]

## Basic commands:
- Run a docker compose container: `sudo docker compose up -d` (in the directory where the image is stored).
- Stop a running container: `docker compose stop <container-name>`
- Prune any stopped containers to avoid cached configs: `docker system prune -f`
- See running containers: `docker ps`
- Execute a command in a particular container: `docker exec -it <<container-name> <command>`.