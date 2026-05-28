# 🖥️ EC2 Instance Setup & SSH Access

A step-by-step walkthrough of launching and connecting to an EC2 instance on AWS.

---

## What is EC2?

Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable virtual servers in the cloud. It allows you to run applications without investing in physical hardware.

---

## Steps to Launch an EC2 Instance

### 1. Log into AWS Console
- Navigate to **EC2** under the Services menu
- Click **"Launch Instance"**

### 2. Choose an AMI (Amazon Machine Image)
- Select **Amazon Linux 2** or **Ubuntu 22.04 LTS** for general use
- AMIs are pre-configured OS templates

### 3. Choose Instance Type
| Type | vCPUs | RAM | Use Case |
|---|---|---|---|
| t2.micro | 1 | 1 GB | Free tier, testing |
| t2.small | 1 | 2 GB | Light workloads |
| t3.medium | 2 | 4 GB | Dev environments |

> 💡 `t2.micro` is free tier eligible — ideal for learning.

### 4. Configure Key Pair
- Create a new key pair (`.pem` file) — **download and save it securely**
- This is your only way to SSH into the instance

### 5. Configure Security Group
Allow the following inbound rules for a basic setup:

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | Your IP |
| HTTP | TCP | 80 | 0.0.0.0/0 |
| HTTPS | TCP | 443 | 0.0.0.0/0 |

> ⚠️ Never set SSH source to `0.0.0.0/0` in production — restrict it to your IP.

### 6. Launch the Instance
- Review your settings and click **"Launch Instance"**
- Wait for the instance state to show **"Running"**

---

## Connecting via SSH (Linux/Mac)

```bash
# Step 1 — Set correct permissions on your key file
chmod 400 your-key.pem

# Step 2 — Connect to your instance
ssh -i "your-key.pem" ec2-user@<your-public-ip>

# For Ubuntu AMIs, use 'ubuntu' instead of 'ec2-user'
ssh -i "your-key.pem" ubuntu@<your-public-ip>
```

---

## Useful Commands Once Connected

```bash
# Check OS info
uname -a

# Update packages
sudo yum update -y         # Amazon Linux
sudo apt update && sudo apt upgrade -y   # Ubuntu

# Check disk space
df -h

# Check memory usage
free -h

# Check running processes
top
```

---

## Key Takeaways

- Always restrict SSH access to your own IP in the Security Group
- Stop instances when not in use to avoid unnecessary charges
- Key pairs cannot be recovered — store your `.pem` file safely
- Elastic IP addresses allow a fixed public IP for your instance
