# Condition Expression:

1) terraform.tf
2) providers.tf
3) ec2.tf
4) variable.tf  -> add a env variable
5) outputs.tf   -> wirte conditional statement accordingly

```
variable.tf

variable "ec2_instance_type"{
  default="t2.micro"
  type= string
}

variable "ec2_default_root_storage_size" {
  default= 10
  type= number
}

variable "ec2_ami_id" {
  default= "ami-005e54dee72cc1d00"
  type= string
}

variable "env" {
  default= "prod"
  type= string
}


```

## EC2 instance: 


```
ec2.tf

resource "aws_instance" "my_instance" {
  count=2 # meta argument
  key_name= aws_key_pair.my_key.key_name
  security_groups= [aws_security_group.my_security_group.name]
  instance_type= var.ec2_instance_type
  ami= var.ec2_ami_id
  user_data= file("install_nginx.sh")    # install_nginx.sh file install nginx

  root_block_device{
    volume_size= var.env == "prod" ? 20 : var.ec2_default_root_storage_size    #  conditional expression
    volume_type= "gp3"
  }

  tags = {
    Name= "my_first_ec2_terraform"
  }
}


```
