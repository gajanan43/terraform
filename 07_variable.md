# Variable in Terraform

1) terraform.tf
2) providers.tf
3) ec2.tf
4) variable.tf

```
variable.tf

variable "ec2_instance_type"{
  default="t2.micro"
  type= string
}

variable "ec2_root_storage_size" {
  default= 15
  type= number
}

variable "ec2_ami_id" {
  default= "ami-005e54dee72cc1d00"
  type= string
}


```

## EC2 instance: 


```
ec2.tf

# ---------------------------
# Key Pair
# ---------------------------
resource "aws_key_pair" "my_key"{
  key_name= "terra-key-ec2"
  public_key= file("terra-key-ec2.pub")
}

# ---------------------------
# Default VPC
# ---------------------------
resource "aws_default_vpc" "default"{

}

# ---------------------------
# Security Group
# ---------------------------
resource "aws_security_group" "my_security_group"{
  name= "automate-sg"
  description= "this will add a TF generated security group"
  vpc_id= aws_default_vpc.default.id  # interpolation

  # inbound rules
  ingress{
    from_port= 22
    to_port= 22
    protocol= "tcp"
    cidr_blocks= ["0.0.0.0/0"]
    description= "SSH Open"
  }

  ingress{
    from_port= 80
    to_port= 80
    protocol= "tcp"
    cidr_blocks= ["0.0.0.0/0"]
    description= "HTTP Open"
  }

  # outbound rules
  egress{
    from_port= 0
    to_port= 0
    protocol= "-1"
    cidr_blocks= ["0.0.0.0/0"]
    description= "all access open outbound"
  }

  tags = {
    Name = "automate-sg"
  }
}

# ---------------------------
# EC2 Instance
# ---------------------------
resource "aws_instance" "my_instance" {
  key_name= aws_key_pair.my_key.key_name
  security_groups= [aws_security_group.my_security_group.name]
  instance_type= var.ec2_instance_type
  ami= var.ec2_ami_id

  root_block_device{
    volume_size= var.ec2_root_storage_size
    volume_type= "gp3"
  }

  tags = {
    Name= "my_first_ec2_terraform"
  }
}

```
