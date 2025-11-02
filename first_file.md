# first HCL(Hashicorp Configuration Language):

```
resource local_file my_file {
  filename="first.txt"
  content="This is my first file"
}

```

1) resource -> block(container)
2) local_file -> resource type
3) my_file -> resource name(identity name)

## working flow:

1) terraform init(create a terraform envirnoment)
2) terraform validation(optional | to check .tf file syntax correct or not)
3) terraform plan (how look like your output | dry run)
4) terraform apply (actual output)
5) terraform destory (delete all)
