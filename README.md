# AWS Hands-On Activities (Beginner-Friendly Guide)

A consolidated set of practical AWS exercises covering Databases, AI/ML, Compute, Storage, and Cost Management. Each activity includes clear steps, commands, and concepts—ideal for beginners exploring real cloud workflows.


---

# 1. Launching an Amazon RDS Database

Overview

Amazon RDS is a managed relational database service supporting MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server. It automates backups, patching, scaling, and monitoring.

Why It Matters

You get a production-ready database without managing servers.

Ideal for applications, microservices, analytics, and dashboards.



---

Steps

1. Open RDS Console

AWS Console → RDS → “Create database”

2. Select Engine

Choose:
```
MySQL 8.x or

PostgreSQL 15.x
```

3. Select Template

Free tier (if available).

4. Configuration
```
DB identifier: my-db-instance

Username: admin
```
Password: create your own


5. Instance size

Free tier: db.t3.micro

6. Storage

Allocated: 20GB

Autoscaling → Enabled


7. Connectivity

VPC: default
Public access: Yes (for testing)
Port:

MySQL → 3306

PostgreSQL → 5432


8. Add Security Group Rule

Open inbound rule:
```

Type: MySQL/PostgreSQL
Source: your-ip
```
9. Launch

Click Create database.


---

CLI Verification (Optional)
```
aws rds describe-db-instances
```

---

Connect From EC2

MySQL example:
```
mysql -h <endpoint> -u admin -p

Postgres example:

psql -h <endpoint> -U admin -d postgres

```

# 2. Amazon Bedrock – Foundation Model Inference

Overview

Amazon Bedrock provides access to foundation models (Anthropic, Amazon Titan, Llama) through a unified API.

Why It Matters

No infrastructure setup

Scalable, pay-as-you-go

Great for chatbots, Q&A, summarization, prompt engineering



---

Enable Bedrock

Go to:
AWS Console → Bedrock → Model Access → Enable models you want.


---

Python Example with AWS SDK
```
Install SDK:

pip3 install boto3 botocore

Example code:

import boto3

client = boto3.client("bedrock-runtime")

response = client.invoke_model(
    modelId="anthropic.claude-v2",
    body='{"prompt":"Tell me a quote about cloud computing."}'
)

print(response["body"].read().decode())
```

---

CLI Example
```
aws bedrock-runtime invoke-model \
  --model-id anthropic.claude-v2 \
  --body '{"prompt":"Hello"}' \
  response.json

```
# 3. AWS Budgets

Overview

AWS Budgets lets you track and control billing costs with alerts.

Why It Matters

Prevent unexpected charges

Set monthly spending limits

Receive alerts through email/SNS



---

Steps

1. Open Budgets

AWS Console → Billing → Budgets

2. Create Budget

Type: Cost budget

Period: Monthly

Amount: e.g., 5 USD


3. Alerts

Set:

80% threshold

Email notification



---

CLI Example
```
aws budgets create-budget \
--account-id <aws-account-id> \
--budget file://budget.json

budget.json example:

{
  "BudgetName": "my-monthly-budget",
  "BudgetLimit": { "Amount": 5, "Unit": "USD" },
  "TimeUnit": "MONTHLY",
  "BudgetType": "COST"
}
```

# 4. Launching an Application on EC2

Overview

EC2 is the core compute service, letting you run apps, servers, APIs, and microservices.

Why It Matters

Full OS control

Deploy any app

Foundation for real infra architectures



---

Steps

1. Launch EC2
```
AMI: Ubuntu / Amazon Linux

Type: t2.micro

Key pair: create one

Security Group:

SSH → 22

HTTP → 80

```


---

2. Install Web Server

Ubuntu:
```
sudo apt update
sudo apt install nginx -y
```
Amazon Linux:
```
sudo yum install httpd -y
sudo systemctl start httpd
```

---

3. Deploy Your App

Example HTML:
```
echo "<h1>Hello from EC2!</h1>" | sudo tee /var/www/html/index.html

```
---

# 5. Amazon S3 – Static Website Hosting

Overview

Amazon S3 provides object storage, versioning, hosting capability, and near-infinite scalability.

Why It Matters

Most cost-efficient hosting

Serverless

Super reliable



---

Steps

1. Create Bucket

S3 → Create bucket

Name: my-static-site

Region: same as EC2

Disable block public access

Enable ACL (if needed)


2. Upload Website Files
```
index.html, style.css, images, etc.

```
---

3. Enable Static Hosting
```
Bucket → Properties → Static website hosting

Index document: index.html


AWS will give you a website URL:

http://my-static-site.s3-website-us-east-1.amazonaws.com/


---
```
CLI Bucket Creation
```
aws s3 mb s3://my-static-site

Upload:

aws s3 cp index.html s3://my-static-site --acl public-read
```
