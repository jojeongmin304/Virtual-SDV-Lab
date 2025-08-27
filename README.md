# Virtual-SDV-Lab

# Virtual SDV Lab – Compose Stack
```
#docker compose 사용으로 Kuksa,Zenoh, CARLA를 각각 컨테이너로 관리 -> 문제확인, 버전교체 용이 / 원하는 개발하려는 툴만 선택적으로 빌드 가능 / QEMU로 에뮬레이션 필요X
# 개발은 docekr compose로 하고, 최적화와 배포는 Leda + Kanto로

#파일 설명: .yaml파일: 여러 서비스(컨테이너) 정의서 -> 어떤이미지, 포트 등 기술. Docker Compose가 이 파일을 읽고 서비스 한번에 실행/중지 한다.
          .env파일: 환경마다 달라지는 값을 적어두는 파일 -> 그래서 커밋하지 않는다. 포트값이 다를 수 있다.

#OS별 네트워킹: 리눅스는 network_mode: "host"가 편리 / macOS,Windows는 ports: 매핑으로 해야한다.

## Prerequisties
- Docker, Docker Compose v2
- (Option) CARLA: GPU Driver and X11 setting


## get Started
```bash
# .env.example 파일 기반으로 본인 환경 맞게 .env파일 생성하기
cp .env.example .env

# basic (KUKSA + zenoh)
docker compose up -d

# + MQTT
docker compose --profile comm up -d


# + CARLA
xhost +local:root # Linux X 권한
docker compose --profile sim up -d


# All services
xhost +local:root # Linux
docker compose --profile comm --profile sim up -d
```

## 네트워크 설정 가이드

#mac 또는 Windows 사용 시
#docker-compose.yaml 파일에서 해당 서비스의 network_mode: "host" 설정 주석처리, 아래처럼 ports매핑 추가

```yaml
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
