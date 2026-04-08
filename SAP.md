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
-- IAM Permission Boundaries设权限边界
- IAM Access Analyzer
- STS 在不同账户间设角色
@103(多)开发团队需要用临时凭证sts:AssumeRole访问生产账户。
@200(多)
- Identity Federation & Cognito
- AWS Directory Services
- AWS Organizations
--OrganizationAccountAccessRole @246
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



