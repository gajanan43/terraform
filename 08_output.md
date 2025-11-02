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

---
---

# Install nginx:

1) terraform.tf
2) providers.tf
3) ec2.tf
4) varible.tf
5) outputs.tf
6) install_nginx.sh

```
install_nginx.sh

sudo apt-get update
sudo apt-get install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx

echo "<h1>This is my terraform file with install nginx on ec2 instance</h1>" | sudo tee /var/www/html/index.html


# ---------------------------
# EC2 Instance
# ---------------------------
resource "aws_instance" "my_instance" {
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
