## Identit & Federation

- Ch3.IAM 最小限度允许访问，有各种策略。作用域单个账户内的用户或角色
IAM用户就像正式员工，有长期工牌（长期凭证）。IAM角色像临时通行证，比如访客证或某个会议室的临时权限（临时凭证）。
策略policy就是写在工牌或通行证上的权限说明，比如“可进入A栋，但不可进入机房”。权限边界SCP就像给你的权限加了个“天花板”，你再怎么申请，也不能超过这个范围。
@245(多) 财务希望跟踪所有账单。创建新账户做管理账户，管理账户创建IAM角色，附权限。允许财务team假设该角色
-- AdministratorAccess
"Effect": "Allow",
"Action": "*",是允许所有操作

-- PowerUserAccess
"Effect": "Allow","NotAction"里面星号是不允许
"Action"下面会写明确的细小iam设置，才是allo-- ConditionsIAM Policies 键值对，带条件的允许操作

-- IAM Policies Variables and Tags @103(多)

-- IAM Roles vs Resource Based Policies 一个账户扮演多个角色
@103(多)注意生产账户还是开发账户的设置内容
开发团队想访问生产账户。生产账户创建IAM policy以允许访问S3，创建role，赋予刚才的policy，让生产账户信任开发账户。
@309(多)allow an IAM user in Account A to assume a role in Account B即，
需要允A中的IAM用户来假设B中的角色。
就要在B配置信任策略允许A操作，A配身份策略sts:AssumeRole
@343
@351 不在Organizations，就无法使用SCP策略。此时需权限控制的话，IAM可以精确控制特定用户/角色在特定区域的资源操作。
@112 需要限制开发者只能启动的实例类型，应创建一个新的IAM策略。指定允许的实例类型，将策略附加到包含开发者IAM账户的IAM组。



-- IAM Permission Boundaries设权限边界

- Ch4.IAM Access Analyzer 自动检测潜在的资源暴露风险 @129 173
-- 策略验证（Policy Validation）可以对IAM策略验证，发现权限问题
-- 基于 CloudTrail 自动生成最小权限策略IAM策略
作用是识别权限，风险，暴露情况。题干会用干扰需求匹配这个工具。

- Ch5.STS(Security Token Service) 在不同账户间设角色,IAM角色是跨账户访问的推荐方式，符合AWS安全最佳实践。 有一些API
-- 核心API.AssumeRole 即create an IAM role
@101在Sales账户中创建一个IAM角色，市场团队的账户中更新QuickSight
@103(多)开发团队需要用临时凭证sts:AssumeRole访问生产账户。
@126@149@480 Amazon Resource Name (ARN)+external ID ; external ID在信任策略中可以防止“混淆代理问题”
@399 IAM roles for tasks,security groups to the tasks(best practice)
@185@454

-- API2.AssumeRoleWithSAML API
SAML = 企业内部员工访问 AWS
@200 正确选项要包含信任策略trust policy；SAML断言(assertions)必须映射用户组或到IAM角色;
AWS STS的AssumeRoleWithSAML API是实现SAML联合身份认证的关键步骤，必须正确调用此API并传递必要参数。
-- API3.AssumeRoleWithWebIdentity
login with Amazon Cognito,Amazon, Facebook, Google, or any OpenID
-- API4.GetSessionToken forMFA

- Ch6.Identity Federation & Cognit
Federation集中管理企业身份,不创建用户。允许“不在 AWS 账户内创建 IAM 用户”的情况下，让外部用户访问 AWS 资源。
目前推荐使用 SAML(企业AD集成) 或 Amazon Cognito
可用方法：SAML企业内部、Amazon Cognito(内部调用 STS,用token，移动App，匿名用户)+
Custom Identity Broker、OpenID Connect即 OIDC(Google 登录)
@385 一个开发单元的成员终止了属于另一个开发单元的EC2实例。用SAML的Session Tag标签管理身份认证，再更新IAM角色和策略
@411 外部用户从互联网访问程序。要通过MFA访问部署在容器的docker中。要用Amazon Cognito建用户池。
@517 用token保护API Gateway和后面的AWS Lambda
@418 用OIDC(IdP)提供用户管理，JWT Token保护ALB和API Gateway HTTP API。但发现ALB接受来自未经身份验证的用户请求。
应该配置即存的ALB，integrating the ALB with the IdP集成IdP和ALB。
@261 创建Organization和即存的Azure AD配合使用
@459 GitHub Actions to run a CI/CD pipeline，需求是构建短期密钥管理。用Create IAM OpenID Connect(OIDC)身份提供(IdP) in AWS IAM.因为是外部调用WebIdentity，所以要sts:AssumeRoleWithWebIdentity API call

- Ch7.AWS Directory Services(AD)
Microsoft Active Directory（AD） 和 AWS Directory Service 的考察;与本地 AD 的集成
Active Directory(AD)作用：集中式身份认证和权限管理。

-- AWS Managed Microsoft AD:托管完整 Microsoft AD。与 SQL Server 集成。与本地AD建立Forest Trust
@319 为托管在VPC的EC2 Windows实例提供安全的远程桌面连接。选项的方案都能实现，但都需要额外的产品配置成本高。用AWS Systems Manager Fleet Manager是个无代理的解决方案，减少了额外的基础设施成本。
@326 要求所有 Windows EC2 实例加入到 AWS 上的 Active Directory，加MFA，想尽可能用AWS托管服务。用AWS Directory Services for Microsoft,Ec2.用Ec2做安全配置

-- AD Connecter:轻量级代理。仅转发身份验证请求到本地AD。不在云中维护AD。只使用现有AD身份验证
@70 要用现有Active Directory凭据访问控制台。正在用AWS IAM Identity Center (AWS Single Sign-On)。要低成本。用创建组织，启用所有功能+AD Connector
@ 402 备灾，以便意外时将员工转移到远程环境。Win和Linux环境。要用本地Active Directory现有身份和MFA，复制现有桌面体验。用Amazon Workspace和AD Connector。配置RADIUS用于MFA
@476 在Ec2上运行Active Directory Domain Service(AD DS).要通过VPN访问VPC,VPN要用MFA。
可用AWS Client VPN endpoint和AD Connector

-- Simple AD:是托管。小规模，成本优先，或测试用途。不缓存目录数据

-- 网络可能中断:不选 AD Connector

- Ch8.AWS Organizations
Question #29, #31, #34, #38, #64, #79, #88
-- OrganizationAccountAccessRole IAM role@246 @367@391@480 跨账户管理assume the IAM role。成员账户中创建，授予管理账户权限以承担IAM角色
--  AWS CloudFormation StackSets:是AWS CloudFormation的一项功能，扩展了标准模板的能力，允许您在多个AWS账户和区域中，通过单次操作来部署和管理一组一致的CloudFormation堆栈。即跨区跨户，组织加集（Organizations+StackSets）
@38 要使用 AWS CloudFormation StackSets将资源（SNS主题）部署到 AWS 组织下的所有成员账户。
要管理账户车创建StackSets，CloudFormationStackSets 自动部署。
@84 跨多个AWS账户和多个区域的基础设施即代码部署。用Organizations 和 AWS CloudFormation StackSets。从一个拥有必要IAM权限的账户部署CloudFormation模板。
--  账单预算相关
@29 托管VPC，Ec2。已使用Key“costCenter”，Value“compliance”标记相关资源，希望识别Ec2上的安全工具成本。应在管理账户中激活costCenter用户定义标签，将数据保存S3
@34 企业有一个为每个team都建了一个OU，每个OU下都有数个AWS账户。应从管理账户创建CUR(Cost and Usage Report)
@79 整合多个账户(来自收购公司，不同计费的账户)的成本数据。用AWS Cost and Usage Report生成数据，自定义tag和成本类别。用Athena DB和QuickSight dataset（报告工具）做分析和可视化。
@88 Organizations架构下，为各业务部门提供独立的月度报告和预算超支通知。应在管理账户用AWS Budgets，Cost Explorer，SNS。
@108 每个部门都有自己的OU，合并计费。HR要加新系统放在新Reserviced Instance，希望确保其他部门不能共享RI折扣。
应在AWS Billing and Cost Management consol，用组织的管理账户关闭人力资源部门生产AWS账户的RI共享。
@225 有Organizations Amazon EC2、AWS Fargate、AWS Lambda，设计优化成本方案。
要通过管理账户在组织级别购买 Compute Savings Plan，使用管理账户级别的推荐。
@226 Organizations，分配每日预算，跟踪成本并提供通知。应在管理账户中用Budget，SNS
@不明 AWS Organizations标记策略。用于标准化跨组织账户的资源标记。它可以强制要求资源必须包含特定标签（如CostCenter， Project），并可以定义标签的允许值，实现标记合规性的集中管理
公司希望将特定成员账户限制在某些AWS区域。应将特定成员账户与一个新OU关联。应用标记策略和使用条件限制区域的SCP。
@334 不同AWS账户内不同的合规要求，与现有的本地 Active Directory Federation Services (AD FS) 服务器集成。
用Control Tower，加OU，连接IAM与本地AD FS。
@377 公司希望提高对各个部门账单的可见性。公司在选择计算资源时不想失去操作灵活性。这里不能选带SCP的。要选合并账单，Compute Savings Plans支持“保持操作灵活性”的需求。
@265 将所有开发者账户迁移到新的开发者组织。
先通过管理账户将开发者移除。创建新组织，将现有账户加入新组织。
从新组管理账户中调InviteAccountToOrganization，向开发账户发送邀请。
让开发人员登账户，确认加入新组。
@373 限制一些DevOps团队不使用的AWS服务的访问权限- 用Access Advisor确定最近使用的服务
拒绝特定的服务- 用Deny
所有账户都在Organizations内，将多个账户作为单一单元- 定义organizational units(OUs)，并将成员账户放OUs中
@379 IT将需要新账户。财务要集中的支付方法，但每个组支出的可见。安全团队要集中控制所有IAM使用。用Organizations，启用所有功能，Service Control来filter IAM权限
@441 所有业务线(LOB)生成单一AWS发票，每个业务线的费用独立计费，可以限制每个业务线的权限，每个业务线自己有完整的管理权限。
用Organization创建单一组织。SCP，允许仅使用批准的服务和功能，然后将策略应用到业务线账户。


- Ch9. Service Control Policies (SCP) 用于权限限制，作用于组织 Question #3, #32, #44, #57, #66
-- 将特定账户放入一个独立的组织单位（OU），然后在该OU上附加SCP和标记策略，是实现精细化、集中化、最小化配置管理的最佳实践。
@309(多) @464 @44 @286 @328(三个选项都是Control Tower但不能选) @410 @66 @245
-- SCP的作用是限制权限,只能“拒绝”操作。把希望允许的用户排除在SCP之外就可以。显式拒绝@282
-- SCP的继承性：需要逐层允许 Root→OU→Account
@3 @364 继承规则：SCP 是从 Root → OU → 子OU → Account
创建临时环境来授予新Config，移动根目录的SCP到目标OU（如Production OU），而不是仅仅创建临时OU。记住“移策”这个动作。
收购新账号，先进 Onboarding
→ 调整Config或其他
→ 再进 Production
@31 共享账户
@32 默认 Organization会默认账户继承FullAWSAccess的SCP，即默认允许所有 AWS 服务。
@57 @294 优先级：SCP > IAM权限 > IAM角色 > IAM用户　SCP设对了但还有问题，可能是IAM设置问题
@270 Control Tower提供了集中化管理的能力，可以创建组织单元（OUs）并附加SCP（Service Control Policies），从而实现跨账户的区域限制。但他无法完全自定义规则

-- Tag Policy:AWS Organizations Tag Policies  用于治理和审计 @282 @113(多)
-- Backup Policy:通过 Organizations 统一管理备份策略。
-- AI Opt-Out Policy:禁止客户数据用于AI模型训练
-- AWS Config持续监控AWS资源配置，并与定义配置规则进行比较要。“检查”才用 Config @113(多)


- Ch10. AWS IAM Identity Center(多个AWS账户和app提供统一登录入口)
-- 用户实际上是在“假设角色（Assume Role）”进入成员账户。

-- Permission Set（权限集）

Question #21, #70
@261
@319



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
Athena是连接原始数据（CUR）和可视化工具（如QuickSight）的桥梁。
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
