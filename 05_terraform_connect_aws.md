# Connect terraform with AWS:

1) install AWS CLI

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

```
2) aws configure
- create a IAM user & give policies(s3 fullaccess)

3) aws s3 ls (check configure or not)
4) create a s3 bucket

```
bucket.tf

resource aws_s3_bucket my_bucket{
  bucket="my_first_bucket"
}

```
- if want to create bucket into a specific region then

```
provider.tf

provider aws {
  region = "ap-south-1"
}

```

---
---


## ☁️ **Connect Terraform with AWS**

Terraform uses the **AWS provider** to interact with your AWS account.
Before running any Terraform commands, you need to **install and configure AWS CLI**.

---

### 🪜 **Step-by-Step Setup**

---

### **1️⃣ Install AWS CLI**

Run the following commands (for Linux):

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

To verify installation:

```bash
aws --version
```

✅ Example output:

```
aws-cli/2.17.0 Python/3.11.4 Linux/5.15.0 botocore/2.17.0
```

---

### **2️⃣ Configure AWS Credentials**

Run:

```bash
aws configure
```

It will ask for:

```
AWS Access Key ID [None]: <Your Access Key>
AWS Secret Access Key [None]: <Your Secret Key>
Default region name [None]: ap-south-1
Default output format [None]: json
```

> 💡 These credentials come from an **IAM User** you create in the AWS Console.

#### 🧑‍💼 **IAM User Setup**

1. Go to AWS Management Console → IAM → Users → **Add user**
2. Select **Programmatic access**
3. Attach policy → **AmazonS3FullAccess** (for S3 operations)
4. Save the **Access Key** and **Secret Key**

---

### **3️⃣ Verify AWS CLI Connection**

Run:

```bash
aws s3 ls
```

If configured correctly, you’ll see your list of S3 buckets (or empty list if none exist).

---

### **4️⃣ Create S3 Bucket with Terraform**

📁 **File: `bucket.tf`**

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-first-terraform-bucket-12345"
}
```

> ⚠️ Bucket names must be **globally unique**, so add a unique suffix (like your name or date).

---

### **5️⃣ Set AWS Region (Optional but Recommended)**

📁 **File: `provider.tf`**

```hcl
provider "aws" {
  region = "ap-south-1"   # Mumbai region
}
```

---

### **6️⃣ Initialize and Apply**

Run:

```bash
terraform init
terraform plan
terraform apply
```

✅ Output:

```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

You can now verify in AWS Console → **S3 → Buckets**.

---

### **7️⃣ Destroy (Cleanup)**

When you’re done:

```bash
terraform destroy
```

---

### 🧩 **Terraform Project Structure**

```
aws-s3/
├── provider.tf       → AWS region/provider setup
├── bucket.tf         → S3 bucket resource
└── terraform.tfstate → State file (created automatically)
```

---


