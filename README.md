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
    image: bitnami/grafana:6
    ports:
      - '3000:3000'
    environment:
      GF_SECURITY_ADMIN_PASSWORD: "bitnami"
      GF_RENDERING_SERVER_URL: "http://grafana-image-renderer:8080/render"
      GF_RENDERING_CALLBACK_URL: "http://grafana:3000/"
  grafana-image-renderer:
    image: bitnami/grafana-image-renderer:1
    ports:
      - '8080:8080'
    environment:
      HTTP_HOST: "0.0.0.0"
      HTTP_PORT: "8080"
      ENABLE_METRICS: 'true'
```








