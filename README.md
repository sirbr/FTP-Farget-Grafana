# FTP-Farget-Grafana

## Prepare local env

### Setup AWS CLI

```bash
$ aws configure --profile <your-profile-name>
AWS Access Key ID [None]: <your-aws-access-key-id>
AWS Secret Access Key [None]: <your-aws-secret-access-key>
Default region name [None]: <your-region>
Default output format [None]:
```

### Setup of eksctl

#### Installation
On macOs with Homebrew, you can just execute:

```bash
brew install weaveworks/tap/eksctl
```

#### Test
```eksctl version```

### Setup docker-compose.yml

```bash
version: '2'

services:
  grafana:
    image: grafana/grafana
    ports:
      - 3000:3000
    environment:
      GF_SERVER_ROOT_URL=http://localhost:3000
      GF_SECURITY_ADMIN_PASSWORD=<Password>
      GF_RENDERING_SERVER_URL=http://localhost:8081/render
      GF_RENDERING_CALLBACK_URL=http://localhost:3000/
      GF_LOG_FILTERS=rendering:debug
      GF_LOG_LEVEL=info
    logging:
      driver: awslogs
      options: 
      awslogs-create-group: true
      awslogs-group: "/ecs/grafana-fargate"
      awslogs-region: <region>
      awslogs-stream-prefix: grafana
  grafana-image-renderer:
    image: grafana/grafana-image-renderer
    expose:
        - 8081
    logging:
      driver: awslogs
      options: 
      awslogs-create-group: true
      awslogs-group: "/ecs/grafana-render-fargate"
      awslogs-region: <region>
      awslogs-stream-prefix: grafana
```





