# Identity and Access Management (IAM)
## Summary

| Term          | Definition          |
| ------------- |:-------------------:|
| Users         | Physical User       |
| Groups | Contains Only Users  |
- Policies : JSON document that outlines permissions for users and groups  
- Roles : for EC2 Instance or AWS Service  
- Security : MFA + Password Policy  
- Access Keys : Programmatic Access (CLI/SDK)  
- Audit : IAM Cred Report & IAM Access Advisor  

---
## Notes

### Root Account
### Users
- Can belong to 0 or many group
- Password Policy
- MFA
  - Virtual MFA (e.g. Google auth)
  - Universal 2nd Factor Key (e.g YubiKey)
  - Hardware Key Fob (e.g RSA)
- Access
  - Management Console
  - CLI (Access Key ID / Secret Access Key)
  - SDK (Access Key ID / Secret Access Key)
- AWS Cloud Shell

### Groups
### Policies
- JSON Document
  - Version
  - ID
  - Statement
    - SID
    - Effect : allows/denies access
    - Principle : account/user/role
    - Action : list of actions
    - Resource : resources to which actions apply
- Policies define `permissions` for a group or user (`inline`). 
- Least privilege principle

### Roles
- Assign permissions, with permission policies, to a `service`
- Common Roles (EC2 Instance Roles, Lambda Function, CloudFormation)

### Security Tools
- IAM Credential Report (account-level)
  - Lists all users & permissions
- IAM Access Advisor (user-level)
  - Service Permissions per user and when they were last used