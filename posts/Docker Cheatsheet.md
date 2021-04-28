# Docker Cheatsheet

## List

### Display all running containers with their ID, name, image, status...

```sh
docker container ls -a
```

### Display all running container with their ID only

```sh
docker container ls -aq
```

## Stop containers

### Stop a chosen container

```sh
docker stop <CONTAINER_ID>
```

### Stop all containers

```sh
docker stop $(docker container ls -aq)
```

## Removing containers

### Removing all stopped containers

```sh
docker container rm $(docker container ls -aq)
```

### Removing all containers

```sh
docker container stop $(docker container ls -aq) && docker system prune -af --volumes
```
