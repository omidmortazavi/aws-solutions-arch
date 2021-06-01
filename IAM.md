# Identity and Access Mamangemr (IAM)

### Root Account
### Users
- Can belong to 0 or many group
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
- Policies define `permissions` of group or `inline` directly to User. 
- Least Privilege Principle