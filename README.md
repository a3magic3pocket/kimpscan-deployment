# kimpscan-deployment
kimpscan-deployment

# 진행
- ```bash
    # kimpscan 스택 배포
    export MY_MARIADB_ROOT_PASSWORD=my_mariadb_root_password && docker stack deploy -c docker-compose.yml kimpscan

    # kimpscan 스택 제거
    docker stack rm kimpscan

    # mariadb
    mariadb -u root -p

    # kimpscan-backend의 sql 디렉토리에서 DB 생성 및 테이블 생성
    # DB 유저 생성
    CREATE USER 'your-id'@'%' IDENTIFIED BY 'your-password';

    # 당신의 유저에 dtt의 모든 권한
    GRANT ALL PRIVILEGES ON <db명>.* TO 'your-id'@'%';

    # docker registry 컨테이너 생성
    # 내부 ip로 소통하기 위해 swarm과 관련 없이 단독으로 실행
    docker compose -f ./docker-compose-registry.yml up -d

    # docker registry 컨테이너 서버 private ip HTTP로 push 하게 설정
    sudo vim /etc/docker/daemon.json
    """ # 추가
    {
        "insecure-registries" : ["10.0.1.251:5000"]
    }
    """
    sudo systemctl daemon-reload

    # kimpscan-backend 빌드 
    docker build -t kimpscan:0.0.1 -f ./dockerfile/Dockerfile-backend .

    # 이미지 태그를 registry 서버 url 포함하게 변경
    # 가정: docker tag kimpscan:0.0.1 90.0.0.123:5000/kimpscan:0.0.1
    docker tag <local-image-name>:<tag> <registry-private-ip>:5000/<image-name>:<tag>
    
    # docker registry로 kimpscan-backend 배포
    docker push 90.0.0.123:5000/kimpscan:0.0.1

    # registry 이미지 목록 조회
    curl http://90.0.0.123:5000/v2/_catalog

    # 특정 이미지의 태그 조회
    curl http://90.0.0.123:5000/v2/kimpscan/tags/list

    # kimpscan-web 빌드
    docker build -t kimpscan-web:0.0.1 -f ./dockerfile/Dockerfile-web .

    # 이미지 태그를 registry 서버 url 포함하게 변경
    docker tag kimpscan-web:0.0.1 90.0.0.123:5000/kimpscan-web:0.0.1
    
    # docker registry로 kimpscan-web 배포
    docker push 90.0.0.123:5000/kimpscan-web:0.0.1
    ```