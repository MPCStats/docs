---
title: "Using Docker"
sidebar_position: 1
---

# Docker Deployment

1. Install `Docker` by following the instructions [here](https://docs.docker.com/engine/install/).

1. Install `Docker Compose` by following the instructions [here](https://docs.docker.com/compose/install/).

1. Build and start all servers with:
   ```bash
   ./configure.py
   docker-compose up --build
   ```

## Troubleshooting
If you encounter any issues, try running the following commands to clean up your Docker environment:

```bash
docker system prune -a --volumes
docker rm -f $(docker ps -aq)
docker volume prune -a -f
docker image prune -f
```
