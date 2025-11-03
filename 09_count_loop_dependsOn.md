# count, loop(for_eachh),depends_on:
## count(change ec2.tf & outputs.tf):

1) terraform.tf
2) providers.tf
3) ec2.tf        -> write count=2 on top (means create 2 ec2 instance)
4) varible.tf
5) outputs.tf    ->for access ip address of 2 instance change in output.tf

```
outputs.tf

output "ec2_public_ip" {
  value= aws_instance.my_instance[*].public_ip
}

output "ec2_public_dns" {
  value= aws_instance.my_instance[*].public_dns
}

output "ec2_private_ip" {
  value= aws_instance.my_instance[*].private_ip
}

```
```
ec2.tf

# ---------------------------
# EC2 Instance
# ---------------------------
resource "aws_instance" "my_instance" {
  count=2 # meta argument
  key_name= aws_key_pair.my_key.key_name
  security_groups= [aws_security_group.my_security_group.name]
  instance_type= var.ec2_instance_type
  ami= var.ec2_ami_id
  user_data= file("install_nginx.sh")    # install_nginx.sh file install nginx

  root_block_device{
    volume_size= var.ec2_root_storage_size
    volume_type= "gp3"
  }

  tags = {
    Name= "my_first_ec2_terraform"
  }
}

```

- But by using this count the name of ec2 instances is same. To solve this problem using for_each

---
---

## for_each & depends_on:

1) terraform.tf
2) providers.tf
3) ec2.tf        -> write a for_each loop
4) varible.tf
5) outputs.tf    -> write a for loop for read ip address


```
outputs.tf

output "ec2_public_ip" {
  value =  [
    for instance in aws_instance.my_instance : key.public_ip 
  ]
}

output "ec2_private_ip" {
  value =  [
    for instance in aws_instance.my_instance : key.private_ip 
  ]
}
 

```


```
ec2.tf

# ---------------------------
# EC2 Instance
# ---------------------------
resource "aws_instance" "my_instance" {
  for_each = tomap({
    my_first_micro= "t2.micro"
    my_second_medium= "t2.medium"
  })         #meta argument

  depends_on = [ aws_security_group.my_security_group, aws_key_pair.my_key ]    # depends on 

  key_name= aws_key_pair.my_key.key_name
  security_groups= [aws_security_group.my_security_group.name]
  instance_type= each.value              # Here a change 
  ami= var.ec2_ami_id
  user_data= file("install_nginx.sh")    

  root_block_device{
    volume_size= var.ec2_root_storage_size
    volume_type= "gp3"
  }

  tags = {
    Name= each.key      # Here a change 
  }
}

```

