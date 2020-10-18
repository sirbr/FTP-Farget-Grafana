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
for non-Linux OS you can find a binary download here:

https://github.com/weaveworks/eksctl/releases

on Linux or Mac, you can just execute:

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp  

sudo mv /tmp/eksctl /usr/local/bin
```

This utility will use the same _credentials_ file as we explored for the AWS cli, located under '~/.aws/credentials'

#### Test
```eksctl version```
