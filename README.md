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
This utility will use the same _credentials_ file as we explored for the AWS cli, located under '~/.aws/credentials'

#### Test
```eksctl version```

### kubectl - the commandline K8s tool

#### install _kubectl
```bash
brew install kubectl 
```
or 
```bash
brew install kubernetes-cli 
```




#### check kubectl

* Linux:  
```kubectl version --short --client```
* Windows:  
```kubectl.exe version --short --client```

