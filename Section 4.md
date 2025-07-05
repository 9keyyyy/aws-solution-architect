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
