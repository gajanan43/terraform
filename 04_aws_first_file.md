# AWS first file:

## prerequisties:
1) terraform init OR
2) by using terraform provider

```
terraform.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

terraform init

```

- we wil go to .terraform/providers/registry.terraform.io/ their will see the aws provider


---
---


## ☁️ **AWS First Terraform File**

### 📘 **File name:** `terraform.tf`

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}
```

---

### 🧩 **Explanation**

1. **`terraform` block → Configuration for Terraform itself**

   * Defines **which providers** Terraform should use.
   * In this case, we are using the **AWS provider**.

2. **`required_providers` → Provider configuration map**

   * Specifies the **source** (from where Terraform should download the provider)
     👉 `"hashicorp/aws"` → means Terraform will download from the **Terraform Registry**.
   * `version = "~> 6.0"` means:
     → Use **any version 6.x.x** (for example, 6.1, 6.2, etc.), but not 7.x.

---

### ⚙️ **Next Step — Initialize Terraform**

Run this command in your terminal:

```bash
terraform init
```

🪄 **What it does:**

* Initializes the current directory as a Terraform working environment.
* Downloads the **AWS provider plugin** (from the Terraform Registry).
* Creates a hidden directory:

  ```
  .terraform/
  ```

  Inside it, you’ll find:

  ```
  .terraform/
  └── providers/
      └── registry.terraform.io/
          └── hashicorp/
              └── aws/
                  └── <version>/
                      └── terraform-provider-aws_v6.x.x_x5
  ```
* Terraform uses this provider plugin to communicate with AWS services.

---

### ✅ **After Initialization**

You’re now ready to:

* Configure the **AWS provider block** (with credentials and region)
* Define **resources** (like EC2, S3, etc.)

Example:

```hcl
provider "aws" {
  region  = "us-east-1"
  profile = "default"  # Optional: if using AWS CLI configured credentials
}
```

---


