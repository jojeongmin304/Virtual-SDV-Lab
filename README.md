# Virtual-SDV-Lab

# Virtual SDV Lab – Compose Stack


## Prerequisties
- Docker, Docker Compose v2
- (Option) CARLA: GPU Driver and X11 setting


## get Starting
# .env.example 파일 기반으로 본인 환경 맞게 .env파일 생성하기
```bash
cp .env.example .env
# basic (only KUKSA)
docker compose up -d


# + Zenoh
docker compose --profile comm up -d


# + CARLA
xhost +local:root # Linux X 권한
docker compose --profile sim up -d


# All services
xhost +local:root # Linux
docker compose --profile comm --profile sim up -d



#mac 또는 Windows 사용 시
#docker-compose.yaml 파일에서 해당 서비스의 network_mode: "host" 설정 주석처리, 아래처럼 ports매핑 추가
# 예시: kuksa 서비스 수정
services:
  kuksa:
    image: ghcr.io/eclipse-kuksa/kuksa-databroker:0.5.0
    command: ["--insecure", "--port", "55555"]
    # network_mode: "host"  # <- 이 부분 주석
    ports:
      - "55555:55555"      # <- 포트 매핑 추가
    restart: unless-stopped
    # ... healthcheck 등 나머지 설정 ...
