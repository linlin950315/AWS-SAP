## Identity&Federation

- IAM 最小限度允许访问，有各种策略
-- AdministratorAccess
"Effect": "Allow",
"Action": "*",是允许所有操作
-- PowerUserAccess
"Effect": "Allow","NotAction"里面星号是不允许
"Action"下面会写明确的细小iam设置，才是allow的操作
-- ConditionsIAM Policies 键值对，带条件的允许操作
-- IAM Policies Variables and Tags @103(多)
-- IAM Roles vs Resource Based Policies 一个账户扮演多个角色
@103(多)注意生产账户还是开发账户的设置内容
开发团队想访问生产账户。生产账户创建IAM policy以允许访问S3。生产账户创建role，赋予刚才的policy，让生产账户信任开发账户。
@309(多)allow an IAM user in Account A to assume a role in Account B即，
需要允A中的IAM用户来假设B中的角色。
就要在B配置信任策略允许A操作，A配身份策略sts:AssumeRole
@343
@351 IAM polic还可以限制EC2的启动类型(t3.small) EC2区域(us-east-2 Region)
-- IAM Permission Boundaries设权限边界
- IAM Access Analyzer
- STS(Security Token Service) 在不同账户间设角色 有一些API
--API1.AssumeRole
@103(多)开发团队需要用临时凭证sts:AssumeRole访问生产账户。
@399 IAM roles for tasks,security groups to the tasks(best practice)
----API2.AssumeRoleWithSAML API@200
----API3.AssumeRoleWithWebIdentity
login with Amazon Cognito,Amazon, Facebook, Google, or any OpenID
--API4.GetSessionToken
- Identity Federation & Cognito
- AWS Directory Services
- AWS Organizations
- --OrganizationAccountAccessRole @246 @367@391@480管理账户assume the IAM role，成员账户OrganizationAccountAccessRole IAM role
--SCP（Service Control Policies）@309(多)@464SCP的作用是限制权限,只能“拒绝”操作。把希望允许的用户排除在SCP之外就可以。
- AWS Organizations Policies
- AWS IAM Identity Center
- AWS Control Tower
- AWS Resource Access Manager
- RAM
- Summary of Identity & Federation

## Security

- CloudTrail
- CloudTrail - EventBridge Integration
- CloudTrail - SA Pro
- KMS
- Parameter Store
- Secrets Manager
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
- AWS Managed Logs
- Amazon GuardDuty
- IAM Advanced Policies
- EC2 Instance Connect
- AWS Security Hub
- Amazon Detective

- Summary of Security & Encryption
- Security & Encryption Quiz



