# Virtual-SDV-Lab

# Virtual SDV Lab – Compose Stack


## Prerequisties
- Docker, Docker Compose v2
- (Option) CARLA: GPU Driver and X11 setting


## get Starting
```bash
cp .env.example .env
# basic (only KUKSA)
docker compose up -d


# + Zenoh
docker compose --profile comm up -d


# + CARLA
xhost +local:root # Linux X 권한
docker compose --profile sim up -d