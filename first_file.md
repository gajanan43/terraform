# first HCL(Hashicorp Configuration Language):

```
resource local_file my_file {
  filename="first.txt"
  content="This is my first file"
}

```

## working flow:

1) terraform init(create a terraform envirnoment)
2) terraform validation(optional | to check .tf file syntax correct or not)
3) terraform plan (how look like your output | dry run)
4) terraform apply (actual output)
5) terraform destory (delete all)
