# 🧱 **Basic Structure**

```hcl
<block> "<parameter>" {
    arguments
}
```

Each **block** defines a specific type of configuration — like variables, resources, outputs, etc.

---

### 1️⃣ **variable block**

Used to **define input variables** that you can pass values to.

```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}
```

* **Block:** `variable`
* **Parameter:** `"instance_type"`
* **Arguments:** description, type, default

---

### 2️⃣ **resource block**

Used to **create or manage infrastructure resources** (like EC2 instances, buckets, etc.).

```hcl
resource "aws_instance" "my_ec2" {
  ami           = "ami-0abcdef1234567890"
  instance_type = var.instance_type
}
```

* **Block:** `resource`
* **Parameter 1:** `"aws_instance"` → provider/type of resource
* **Parameter 2:** `"my_ec2"` → local name for the resource
* **Arguments:** resource properties (like `ami`, `instance_type`, etc.)

---

### 3️⃣ **output block**

Used to **display values after Terraform apply** — such as IP addresses, IDs, etc.

```hcl
output "instance_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.my_ec2.public_ip
}
```

* **Block:** `output`
* **Parameter:** `"instance_ip"`
* **Arguments:** description, value

---

### ⚙️ Other Common Blocks

You’ll also encounter:

* `provider` → defines which cloud provider to use (e.g., AWS, Azure, GCP)
* `locals` → define reusable local variables
* `module` → reference reusable Terraform modules

Example:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

---


