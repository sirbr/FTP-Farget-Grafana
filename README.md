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
      GF_SECURITY_ADMIN_PASSWORD=<Password> ###not a secure option
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

Grafana needs permissions granted via IAM to be able to read CloudWatch metrics and EC2 tags/instances/regions. 
Here is a role example:
```bash
  ECSTaskExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Statement:
        - Effect: Allow
          Principal:
            Service: [ecs-tasks.amazonaws.com]
          Action: ['sts:AssumeRole']
      Path: /
      Policies:
        - PolicyName: AmazonECSTaskExecutionRolePolicy
          PolicyDocument:
            Statement:
            - Sid: AllowReadingMetricsFromCloudWatch
              Effect: Allow
              Action:
                -cloudwatch:DescribeAlarmsForMetric
                -cloudwatch:DescribeAlarmHistory
                -cloudwatch:DescribeAlarms
                -cloudwatch:ListMetrics
                -cloudwatch:GetMetricStatistics
                -cloudwatch:GetMetricData
              Resource: '*'
            - Sid: AllowReadingLogsFromCloudWatch
              Effect: Allow
              Action:
                -logs:DescribeLogGroups
                -logs:GetLogGroupFields
                -logs:StartQuery
                -logs:StopQuery
                -logs:GetQueryResults
                -logs:GetLogEvents
              Resource: '*'
            - Sid: AllowReadingTagsInstancesRegionsFromEC2
              Effect: Allow
              Action:
                -ec2:DescribeTags 
                -ec2:DescribeInstances
                -ec2:DescribeRegions
            - Sid: AllowReadingResourcesForTags
              Effect: Allow
              Action:
                -tag:GetResources
              Resource: '*'
```
### Allow inbound access to Grafana’s default port: 3000
Allow inbound requests to port 3000

### Grafana’s Public IP
ECS > Tasks 

Grafana default credentials are username: admin and password: admin
You can chosse the standard Grafana image to get password stored in AWS Systems Manager Parameter Store


## Usefill links 

* [ECS Cluster with a Fargate Task](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-cli-tutorial-fargate.html)
* [Using AWS CloudWatch in Grafana](https://grafana.com/docs/grafana/latest/datasources/cloudwatch/)




