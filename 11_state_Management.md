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
-  Store terraform.tfstate S3 bucket
-  Sate file locking using DynamoDB

-  first one change .tfstate file then trigger goes form s3 -> DynamoDB table & DynamoDB generate LockID
-  If second want to change the file at that he cann't change because the LockID was generated
-  If someone want to access S3 bucket LockId cann't be generated

