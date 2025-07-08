### Section 4: IAM 및 AWS CLI

**AWS Identity and Access Management (AWS IAM)**

- Global service
- Root account created by default, shouldn’t be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups
- Users don’t have to belong to a group, and user can belong to multiple groups




**IAM – Password Policy**

- Strong passwords = higher security for your account
- In AWS, you can setup a password policy:<br>
• Set a minimum password length<br>
• Require specific character types<br>
• Allow all IAM users to change their own passwords<br>
• Require users to change their password after some time (password expiration)<br>
• Prevent password re-use<br>

**Multi Factor Authentication - MFA**

- MFA = password you know + security device you own
- Main benefit?
    - if a password is stolen or hacked, the account is not compromised

**MFA devices options in AWS**

- Virtual MFA device
- Universal 2nd Factor (U2F) Security Key
- Hardware Key Fob MFA Device
- Hardware Key Fob MFA Device for AWS GovCloud (US)

**How can users access AWS**

- To access AWS, you have three options:<br>
• AWS Management Console (protected by password + MFA)<br>
• AWS Command Line Interface (CLI): protected by access keys<br>
• AWS Software Developer Kit (SDK) - for code: protected by access keys<br>
- Access Keys are generated through the AWS Console<br>
- Users manage their own access keys
- Access Keys are secret, just like a password. Don’t share them<br>
• Access Key ID ~= username<br>
• Secret Access Key ~= password<br>

**What’s the AWS CLI**

- A tool that enables you to interact with AWS services using commands in your command-line shell
- Direct access to the public APIs of AWS services
- You can develop scripts to manage your resources

**What’s the AWS SDK**

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enables you to access and manage AWS services programmatically


**IAM Roles for Services**

- Some AWS service will need to perform actions on your behalf
- To do so, we will assign permissions to AWS services with IAM Roles
- Common roles:
    - EC2 Instance Roles
    - Lambda Function Roles
    - Roles for CloudFormation

**IAM Security Tools**

- IAM Credentials Report (account-level)
• a report that lists all your account's users and the status of their various credentials
- IAM Access Advisor (user-level)
• Access advisor shows the service permissions granted to a user and when those services were last accessed.
• You can use this information to revise your policies

**IAM Guidelines & Best Practices**

- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
- Never share IAM users & Access Keys

**IAM Section – Summary** 

- Users: mapped to a physical user, has a password for AWS Console
- Groups: contains users only
- Policies: JSON document that outlines permissions for users or groups
- Roles: for EC2 instances or AWS services
- Security: MFA + Password Policy
- AWS CLI: manage your AWS services using the command-line
- AWS SDK: manage your AWS services using a programming language
- Access Keys: access AWS using the CLI or SDK
- Audit: IAM Credential Reports & IAM Access Advisor
