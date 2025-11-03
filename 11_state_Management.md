# State Management & Backends

1) Role of state in infrastructure management  
2) Secure state management best practices  
3) Remote state backends  
   i) AWS S3 for remote storage  
   ii) State locking with DynamoDB


# Role of state in infrastructure management:   

##  Commands:

1) terraform state list
2) terraform state show state_name
3) terraform state show rm state_name
4) terraform import aws_key_pair.my_key key_id_on_aws



## importing existing ec2(manual created):

```
ec2.tf

resource "aws_instance" "my_new_instance" {
  ami= "unknown"
  instance_type= "unknown"
}
```
- Apply this commands
  
```terraform import aws_instance.my_new_instance ami_id```

---
---

# Secure state management best practices:

## Problem with terraform.tfstate
1) commit on github
2) state conflict(Between persions on single account first want to run 1 instance & second persion want to run 2 instance)

## Solution is Remote Backend:
1)  Store terraform.tfstate S3 bucket
2)  State file locking using DynamoDB

-  first one change .tfstate file then trigger goes form s3 -> DynamoDB table & DynamoDB generate LockID
-  If second want to change the file at that he cann't change because the LockID was generated
-  If someone want to access S3 bucket LockId cann't be generated


---
---

# Practical:

## Step 1:

1) create new folder
2) terraform.tf
3) providers.tf
4) s3.tf
5) dynamodb.tf

```
s3.tf

resource "aws_s3_bucket" "remote_s3" {
  bucket = "my_first_bucket"

  tags = {
    Name        = "my_first_bucket"
  }
}

```
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

```

```
providers.tf

provider "aws" {
  region = "us-east-1"
}

```

```
dynamodb.tf

resource "aws_dynamodb_table" "basic-dynamodb-table" {
  name           = "my_first_table"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }

  tags = {
    Name        = "my_first_table"
  }
}

```

- apply command ```terraform refresh```
- if delete the tf.state file & backup file we cann't get it

## Step 2:

1) terraform.tf -> add a backup(S3 & DynamoDB)
2) providers.tf
3) ec2.tf
4) varible.tf
5) outputs.tf

```
terraform.tf

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }

backend "s3" {
   bucket = "my_first_bucket"
   key = "terraform.tfstate"
   region = "us-east-1"
   dynamodb_table = "my_first_table"
}
}

- Now if delete the tf.state file we can get backup file form S3
