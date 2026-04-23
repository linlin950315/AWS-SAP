## Identit & Federation

- Ch3.IAM 最小限度允许访问，有各种策略
IAM用户就像正式员工，有长期工牌（长期凭证）。IAM角色像临时通行证，比如访客证或某个会议室的临时权限（临时凭证）。
策略policy就是写在工牌或通行证上的权限说明，比如“可进入A栋，但不可进入机房”。权限边界SCP就像给你的权限加了个“天花板”，你再怎么申请，也不能超过这个范围。

-- AdministratorAccess
"Effect": "Allow",
"Action": "*",是允许所有操作

-- PowerUserAccess
"Effect": "Allow","NotAction"里面星号是不允许
"Action"下面会写明确的细小iam设置，才是allo-- ConditionsIAM Policies 键值对，带条件的允许操作

-- IAM Policies Variables and Tags @103(多)

-- IAM Roles vs Resource Based Policies 一个账户扮演多个角色
@103(多)注意生产账户还是开发账户的设置内容
开发团队想访问生产账户。生产账户创建IAM policy以允许访问S3。生产账户创建role，赋予刚才的policy，让生产账户信任开发账户。
@309(多)allow an IAM user in Account A to assume a role in Account B即，
需要允A中的IAM用户来假设B中的角色。
就要在B配置信任策略允许A操作，A配身份策略sts:AssumeRole
@343
@351 IAM police还可以限制EC2的启动类型(t3.small) EC2区域(us-east-2 Region)

-- IAM Permission Boundaries设权限边界

- Ch4.IAM Access Analyzer 自动检测潜在的资源暴露风险 @129 173
-- 策略验证（Policy Validation）可以对IAM策略验证，发现权限问题
-- 基于 CloudTrail 自动生成最小权限策略IAM策略
作用是识别权限，风险，暴露情况。题干会用干扰需求匹配这个工具。

- STS(Security Token Service) 在不同账户间设角色 有一些API
Question #101, #103
-- API1.AssumeRole
@103(多)开发团队需要用临时凭证sts:AssumeRole访问生产账户。
@399 IAM roles for tasks,security groups to the tasks(best practice)
-- API2.AssumeRoleWithSAML API@200
-- API3.AssumeRoleWithWebIdentity
login with Amazon Cognito,Amazon, Facebook, Google, or any OpenID
-- API4.GetSessionToken forMFA

- Identity Federation & Cognito
  
- AWS Directory Services
  
- AWS Organizations
Question #29, #31, #34, #38, #64, #79, #88
--OrganizationAccountAccessRole @246 @367@391@480管理账户assume the IAM role，成员账户OrganizationAccountAccessRole IAM role

- Service Control Policies (SCP)
@309(多)@464
SCP的作用是限制权限,只能“拒绝”操作。把希望允许的用户排除在SCP之外就可以。
Question #3, #32, #44, #57, #66

- AWS IAM Identity Center
Question #21, #70

- AWS Control Tower
Question #64

- AWS Resource Access Manager
Question #11, #30, #53, #98

- RAM


## Security

- CloudTrail
Question #90
- CloudTrail - EventBridge Integration
- CloudTrail - SA Pro
- KMS(Key Management Service)
Question #51, #96, #101
- Parameter Store
Question #26
- Secrets Manager
Question #26
- RDS Security
- SSL Encryption, SNI & MITM
- AWS Certificate Manager - ACM
- CloudHSM
- Solution Architecture - SSL on ELB

- S3 Security
- S3 Access Points
- S3 Multi-Region Access Points
- S3 Multi-Region Access Points - Hands On
- S3 Object Lambda

- DDoS and AWS Shield
- AWS WAF - Web Application Firewall
- AWS Firewall Manager
- Blocking an IP Address

- Amazon Inspector
- AWS Config
Question #44, #53
- AWS Managed Logs
- Amazon GuardDuty
- IAM Advanced Policies(Least Privilege, Permissions Boundaries, Service Roles)
Question #74
- EC2 Instance Connect
- AWS Security Hub
- Amazon Detective

- Summary of Security & Encryption
- Security & Encryption Quiz

## Compute & Load Balancing

- Solution Architecture on AWS

### Compute

- EC2
- High Performance Computing (HPC)
- Auto Scaling
- Auto Scaling Update Strategies
- Spot Instances & Spot Fleet


### Containers

- Amazon ECS (Elastic Container Service)
- Amazon ECR (Elastic Container Registry)
- Amazon EKS (Elastic Kubernetes Service)
- AWS App Runner
- ECS Anywhere & EKS Anywhere


### Serverless

- AWS Lambda - Part 1
- AWS Lambda - Part 2

### Load Balancing & API

- Elastic Load Balancers - Part 1
- Elastic Load Balancers - Part 2
- API Gateway
- API Gateway - Part 2
- AWS AppSync

### Networking & DNS

- Route 53 - Part 1
- Route 53 - Part 2
- Route 53 - Resolvers & Hybrid DNS
- AWS Global Accelerator

### Architecture & Edge

- Comparison of Solutions Architecture
- AWS Outposts
- AWS WaveLength
- AWS Local Zones


## Storage

### Block & File Storage

- EBS & Local Instance Store
- Amazon EFS

### Object Storage

- Amazon S3
- Amazon S3 - Storage Class Analysis
- Amazon S3 - Storage Lens
- S3 Solution Architecture

### File Systems (Advanced)

- Amazon FSx
- Amazon FSx - Solution Architectures

### Data Transfer & Migration

- AWS DataSync
- AWS DataSync - Solution Architecture
- AWS Data Exchange
- AWS Transfer Family

### Cost & Comparison

- AWS Storage Services Price Comparison

## Caching

### CloudFront

- CloudFront - Part 1
- CloudFront - Part 2

### Edge Computing

- Lambda@Edge and CloudFront Functions
- Lambda@Edge Reduce Latency

### Caching

- Amazon ElastiCache

### Performance & Scaling

- Handling Extreme Rates

## DB

### NoSQL

- DynamoDB

### Search & Analytics

- Amazon OpenSearch

### Relational Databases

- RDS
- Aurora - Part 1
- Aurora - Part 2

## Service & Communication

### Orchestration

- Step Functions

### Queueing

- SQS

### Messaging

- Amazon SNS
- Amazon SNS - SQS Fan Out Pattern
- Amazon SNS - Message Delivery Retries

### Managed Brokers

- Amazon MQ

## Data Engineering

### Streaming

- Amazon Kinesis Data Streams
- Amazon Data Firehose
- Amazon Managed Service for Apache Flink
- Streaming Architectures

### Messaging & Streaming Platforms

- Amazon MSK

### Batch Processing

- AWS Batch
- Amazon EMR
- Running Jobs on AWS

### Data Integration

- AWS Glue

### Data Warehousing

- Redshift

### Specialized Databases

- Amazon DocumentDB
- Amazon Timestream

### Data Query & Visualization

- Amazon Athena
- Amazon QuickSight

### Architecture

- Big Data Architecture

## Monitoring

### Metrics & Monitoring

- CloudWatch

### Logging

- CloudWatch Logs

### Eventing

- Amazon EventBridge

### Tracing

- X-Ray

### Health

- AWS Personal Health Dashboard

## Developing & Instance Management

### Platform as a Service

- Elastic Beanstalk

### Deployment

- CodeDeploy

### Infrastructure as Code

- CloudFormation
- AWS CDK - Cloud Development Kit
- SAM - Serverless Application Model

### Governance

- Service Catalog

### Operations & Management

- AWS Systems Manager - SSM

### Service Discovery

- AWS Cloud Map

## Cost Control

### Cost Allocation

- Cost Allocation Tags
- AWS Tag Editor

### Advisory & Limits

- Trusted Advisor
- AWS Service Quotas

### Compute Cost Optimization

- EC2 Launch Types & Savings Plan
- EC2 Reserved Instance
- AWS Compute Optimizer

### Storage Cost Optimization

- S3 Cost Savings
- S3 Storage Classes - Reminder

### Monitoring & Budgeting

- AWS Budgets & Cost Explorer

## Migration

### Migration Strategies

- Cloud Migration Strategies - The 7Rs

### Hybrid Storage

- Storage Gateway
- Storage Gateway - Advanced Concepts

### Data Transfer Appliances

- Snow Family
- Snow Family - Improving Performance

### Database Migration

- AWS DMS - Database Migration Services

### Migration Planning

- AWS CART - Cloud Adoption Readiness Tool
- AWS Migration Evaluator

### VM Migration

- VM Migrations Services

### Disaster Recovery

- Disaster Recovery

### Resilience Testing

- AWS FIS - Fault Injection Simulator

### Backup

- AWS Backup

## VPC

### VPC Fundamentals

- VPC - Basics

### Connectivity (VPC to VPC)

- VPC Peering
- Transit Gateway

### Private Access

- VPC Endpoints
- VPC Endpoint Policies
- PrivateLink

### Hybrid Connectivity

- AWS S2S VPN
- AWS Client VPN
- Direct Connect
- On-Premise Redundant Connections

### Monitoring

- VPC Flow Logs

### Security

- AWS Network Firewall

## Maching Learning

## Other Service
