# 🧱 **HCL (HashiCorp Configuration Language)**

Terraform uses **HCL** as its configuration language to define **infrastructure as code (IaC)**.
Each `.tf` file is written in HCL and contains blocks that describe what resources to create, configure, or manage.

---

### 🧩 Example Code

```hcl
resource "local_file" "my_file" {
  filename = "first.txt"
  content  = "This is my first file"
}
```

---

### 🔍 **Explanation**

1. **`resource` → Block (Container)**

   * Defines an infrastructure component to be managed by Terraform.

2. **`local_file` → Resource Type**

   * Comes from the **local provider**.
   * `local` provider allows you to create and manage local system resources like files.

3. **`my_file` → Resource Name (Identity Name)**

   * A unique name to identify this specific resource instance in your configuration.

4. **Inside the block → Arguments**

   * `filename` and `content` define the properties of the resource.

---

### ⚙️ **Terraform Working Flow**

| Step                        | Command              | Description                                                                                                          |
| --------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **1️⃣ Initialize**          | `terraform init`     | Creates a **Terraform working environment** and downloads required **provider plugins** (like `local`, `aws`, etc.). |
| **2️⃣ Validate (optional)** | `terraform validate` | Checks if your `.tf` file syntax is **valid and error-free**.                                                        |
| **3️⃣ Plan**                | `terraform plan`     | Shows a **preview (dry run)** of what Terraform will do — create, modify, or delete resources.                       |
| **4️⃣ Apply**               | `terraform apply`    | Executes the plan and **creates/updates** real infrastructure.                                                       |
| **5️⃣ Destroy**             | `terraform destroy`  | **Removes all resources** defined in your configuration.                                                             |

---

### 🗂️ **Output after apply**

After running:

```bash
terraform apply
```

Terraform will:

* Create `first.txt` in your working directory
* Add the text:

  ```
  This is my first file
  ```

---


