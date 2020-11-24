---
layout: post
title:  "Terraform Notes"
date:   2020-11-19
categories: Technology
---
<link rel="stylesheet" type="text/css" media="all" href="assets/css/markdown_styles.css" />

# Terraform notes #
### Requirements:
<sup>:heavy_check_mark: <strong>[Install Terraform](https://www.terraform.io/downloads.html)</strong></sup>
  
<sup>:heavy_check_mark: <strong>[Create AWS Account](https://console.aws.amazon.com/console)</strong></sup>  


## Table of contents
- [Useful Commands](#Useful-Commands)
- [Spin up a t2.micro instance on AWS using Terraform](#Spin-up-a-t2.micro-instance-on-AWS-using-Terraform)
- [Spin up a docker Nginx image locally with Terraform](#Spin-up-a-docker-Nginx-image-locally-with-Terraform)


#### Useful Commands
```
$ terraform plan                                # shows what will be used when building base on terraform file
$ terraform apply                               # shortcut for (terraform plan -out file; terraform apply file; rm file;)
                                                avoid this in production
$ terraform plan -out out.terraform             # terraform plan and write the plan to an out file
$ terraform apply out.terraform                 # apply terraform plan using out file
$ terraform show                                # show current state
$ cat terraform.tfstate                         # show state in JSON format
$ terraform destroy                             # terminate instance 
$ terraform fmt                                 # format configuration
$ terraform validate                            # validate configuration
$ terraform apply -var-file="sensitive.tfvars"  # load variable from file
$ export TF_VAR_region="us-east-1"              # set env variable
$ unset TF_VAR_region
```

### Spin up a t2.micro instance on AWS using Terraform
<details><summary>Create AWS admin user</summary>
Create new user named <strong>terraform</strong><br/>
Create new group named <strong>Admin</strong> with <strong>AdministrationAccess</strong> policy<br/>
Add user <strong>terraform</strong> to <strong>Admin<strong> group<br/>
Save csv credentials<br />
</details>
<details><summary>Terraform Configs</summary>
provider.tf

```
provider "aws" {
    access_key = var.AWS_ACCESS_KEY
    secret_key = var.AWS_SECRET_KEY
    region     = var.AWS_REGION
}
```
instance.tf

```
resource "aws_instance" "example" {
    ami = var.AMIS[var.AWS_REGION]
    instance_type = "t2.micro"
}
```
var.tf

```
variable "AWS_ACCESS_KEY" {}
variable "AWS_SECRET_KEY" {}
variable "AWS_REGION" {
  default = "us-east-1"
}
variable "AMIS" {
  type = map
  default = {
    us-east-1 = "ami-0947d2ba12ee1ff75",
    us-east-1 = "ami-0885b1f6bd170450c"
  }
}
``` 
terraform.tfvars

```
AWS_ACCESS_KEY = "REPLACE_ACCESS_KEY"
AWS_SECRET_KEY = "REPLACE_SECRET_KEY"
AWS_REGION = "us-east-1 "
```
</details>

<details><summary>Terraform Commands</summary>
  
```
terraform init
terraform plan
terraform plan -out out.terraform
terraform apply -var-file="out.terraform
terraform show
terraform destroy
```
</details>



    
### Spin up a docker Nginx image locally with Terraform
  
### Test Ansible build AMIs with Packer
