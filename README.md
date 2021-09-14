# Docker 环境管理

### 容器编译

```shell
docker-compose -f docker-compose.developer.yml -p developer build --no-cache
```

### 容器启动

```shell
docker-compose -f docker-compose.developer.yml -p developer  up -d
```