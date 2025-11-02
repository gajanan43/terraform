# Output in terraform:

1) terraform.tf
2) providers.tf
3) ec2.tf
4) varible.tf
5) outputs.tf

```
outputs.tf

output "ec2_public_ip" {
  value= aws_instance.my_instance.public_ip
}

output "ec2_public_dns" {
  value= aws_instance.my_instance.public_dns
}

output "ec2_private_ip" {
  value= aws_instance.my_instance.private_ip
}

```
