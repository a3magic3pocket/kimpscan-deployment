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
    ```