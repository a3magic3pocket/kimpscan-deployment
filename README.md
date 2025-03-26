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
    GRANT ALL PRIVILEGES ON <db명>'.* TO 'your-id'@'%';

    # docker registry 컨테이너 생성
    # 내부 ip로 소통하기 위해 swarm과 관련 없이 단독으로 실행
    docker compose -f ./docker-compose-registry.yml up -d

    # kimpscan-backend 빌드 
    docker build -t kimpscan:0.0.1 -f ./dockerfile/Dockerfile-backend .

    ```