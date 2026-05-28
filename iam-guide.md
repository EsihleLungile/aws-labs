# 🔐 IAM — Users, Roles, Policies & Best Practices

A practical guide to AWS Identity and Access Management (IAM) — controlling who can do what in your AWS account.

---

## What is IAM?

IAM (Identity and Access Management) is AWS's service for managing access to resources. It controls **authentication** (who you are) and **authorisation** (what you're allowed to do).

---

## Core IAM Components

| Component | Description |
|---|---|
| **User** | An individual identity (person or application) |
| **Group** | A collection of users sharing the same permissions |
| **Role** | An identity assumed temporarily (e.g. by an EC2 instance or Lambda) |
| **Policy** | A JSON document defining allowed/denied actions |

---

## Creating an IAM User (Console)

1. Go to **IAM** → **Users** → **"Add Users"**
2. Set a username (e.g. `esihle-admin`)
3. Choose **"Provide user access to the AWS Management Console"** if needed
4. Set a password and choose whether to require a password reset
5. Attach permissions — either directly or via a Group

---

## Creating a Group & Attaching a Policy

```
IAM → User Groups → Create Group
  Name: "CloudAdmins"
  Attach policy: "AdministratorAccess" (for admins)
                 "AmazonEC2FullAccess"  (for EC2 engineers)
                 "AmazonS3ReadOnlyAccess" (for read-only S3)
```

Then add your user to this group — they inherit all group permissions.

---

## Example: Custom IAM Policy (JSON)

This policy allows a user to list and read from a specific S3 bucket only:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-bucket-name",
        "arn:aws:s3:::my-bucket-name/*"
      ]
    }
  ]
}
```

> 💡 Always follow the **Principle of Least Privilege** — grant only the permissions needed, nothing more.

---

## IAM Roles — When and Why

Roles are used when **AWS services need to access other services on your behalf**.

**Example:** An EC2 instance that needs to write logs to S3 should use a Role, not hardcoded credentials.

```
IAM → Roles → Create Role
  Trusted entity: AWS Service → EC2
  Attach policy: AmazonS3FullAccess (or a custom policy)
  Name: "EC2-S3-WriteRole"
```

Then attach the role to your EC2 instance under **Actions → Security → Modify IAM Role**.

---

## IAM Best Practices

| Practice | Why It Matters |
|---|---|
| 🔒 Enable MFA on the root account | Root has unlimited access — protect it at all costs |
| 👤 Never use root for daily tasks | Create an admin IAM user instead |
| 📏 Least privilege principle | Reduce blast radius if credentials are compromised |
| 🔑 Rotate access keys regularly | Old keys are a security risk |
| 📋 Use IAM roles for services | Avoid hardcoding credentials in code |
| 🔍 Use IAM Access Analyzer | Identifies resources shared with external accounts |

---

## Key Takeaways

- IAM is global — it applies across all AWS regions
- Policies are written in JSON and define Allow/Deny rules
- Roles are preferable to users for service-to-service access
- Always audit IAM permissions regularly using the IAM Credentials Report
