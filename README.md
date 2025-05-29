import { CodeBlock } from '@site/src/components/CodeBlock';

# AWS IAM User Management – Role-Based Access Control (RBAC)

This project demonstrates how to implement a **Role-Based Access Control (RBAC)** system in AWS using **IAM (Identity and Access Management)**. It includes secure creation and management of users, groups, roles, and policies, along with monitoring and auditing configurations.

---

## 📌 Features

- ✅ RBAC model with IAM users, groups, and roles  
- ✅ Fine-grained access control using IAM policies  
- ✅ Enforced security best practices (MFA, access key rotation)  
- ✅ Logging and auditing with AWS CloudTrail and AWS Config  

---

## 🔧 Prerequisites

- AWS account with administrator access  
- AWS CLI installed and configured  
- IAM permissions to create users, roles, and policies  
- Basic knowledge of AWS IAM and security  

---

## 🚀 Project Structure

<CodeBlock language="bash">{`
aws-iam-rbac/
├── policies/
│   ├── admin-policy.json
│   ├── dev-policy.json
│   └── read-only-policy.json
├── setup-iam.sh         # Bash script to automate setup (optional)
└── README.mdx
`}</CodeBlock>

---

## 📁 Step-by-Step Guide

### Step 1: 🛠️ Create IAM Groups

<CodeBlock language="bash">{`
aws iam create-group --group-name Admins
aws iam create-group --group-name Developers
aws iam create-group --group-name ReadOnlyUsers
`}</CodeBlock>

---

### Step 2: 📝 Create IAM Policies

**Example**: `admin-policy.json`

<CodeBlock language="json">{`
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "*",
    "Resource": "*"
  }]
}
`}</CodeBlock>

**Create the policy using CLI:**

<CodeBlock language="bash">{`
aws iam create-policy \\
  --policy-name AdminPolicy \\
  --policy-document file://policies/admin-policy.json
`}</CodeBlock>

Repeat this step for `dev-policy.json` and `read-only-policy.json`.

---

### Step 3: 🔗 Attach Policies to Groups

<CodeBlock language="bash">{`
aws iam attach-group-policy \\
  --group-name Admins \\
  --policy-arn arn:aws:iam::<YOUR_ACCOUNT_ID>:policy/AdminPolicy

aws iam attach-group-policy \\
  --group-name Developers \\
  --policy-arn arn:aws:iam::<YOUR_ACCOUNT_ID>:policy/DevPolicy
`}</CodeBlock>

---

### Step 4: 👥 Create IAM Users and Add to Groups

<CodeBlock language="bash">{`
aws iam create-user --user-name JohnAdmin
aws iam add-user-to-group --user-name JohnAdmin --group-name Admins

aws iam create-user --user-name JaneDev
aws iam add-user-to-group --user-name JaneDev --group-name Developers
`}</CodeBlock>

---

### Step 5: 🔐 Enforce MFA and Security

Enable MFA on each user via AWS Console or CLI.

**Policy to enforce MFA:**

<CodeBlock language="json">{`
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {
      "BoolIfExists": {
        "aws:MultiFactorAuthPresent": "false"
      }
    }
  }]
}
`}</CodeBlock>

---

### Step 6: 🔍 Monitor IAM Activity with CloudTrail and AWS Config

#### Enable CloudTrail

- Open AWS Console → CloudTrail → Create Trail  
- Enable logging for all regions  

#### Enable AWS Config

- Select all resource types to record  
- Set up SNS notifications and S3 bucket for delivery (optional)  

---

## 📜 Best Practices Followed

- Principle of least privilege  
- Regular access key rotation  
- Use of roles over static credentials  
- MFA enforcement  
- Continuous monitoring and auditing  

---

## 🧰 Optional Automation

You can automate the setup using a bash script `setup-iam.sh` using the AWS CLI commands listed above.

---

## 📚 Resources

- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  
- [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)  
- [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/)  

---

## 🤝 Contributing

Fork the repo and submit a PR to improve automation, structure, or security.

---

## 📄 License

This project is licensed under the MIT License.
