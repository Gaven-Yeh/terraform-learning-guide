# terraform-learning-guide

## Contents
1. [Intro](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#1-intro)
2. [Installing Terraform](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#2-installing-terraform-windows)
3. [How to use Terraform](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#3-using-terraform)
   1. [Set up credentials](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#31-set-up-credentials)
   2. [Create infrastructure](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#32-create-infrastructure)
   3. [Change infrastructure](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#33-change-infrastructure)
   4. [Destroy infrastructure](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#34-destroy-infrastructure)
   5. [Resource Dependencies](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#35-resource-dependencies)
   6. [Provisioners](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#36-provisioners)
   7. [Input and Output Variables](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#37-input-and-output-variables)
4. [Using Terraform as a Team](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#4-using-terraform-as-a-team)
   1. [Remote State](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#41-remote-state)
   2. [State Locking](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#42-state-locking)
5. [Modules](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#5-modules)
   1. [Using pre-existing modules](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#51-using-pre-existing-modules)
   2. [Creating modules](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#52-creating-modules)
   3. [Hosting modules](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#53-hosting-modules)
6. [Code organisation](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#6-code-organisation)
   1. [Workspaces](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#61-workspaces)
   2. [File layout](https://github.com/Gaven-Yeh/terraform-learning-guide/blob/master/README.md#62-file-layout)

## 1. Intro
Check out [Hashicorp's Terraform Intro](https://learn.hashicorp.com/terraform/getting-started/intro). (5-10 minutes)

(Optional) Watch the following introduction video (20 minutes) - [Introduction to HashiCorp Terraform with Armon Dadgar](https://www.youtube.com/watch?v=h970ZBgKINg&feature=youtu.be) 

## 2. Installing Terraform (Windows)
1. Download Terraform for your OS [here](https://www.terraform.io/downloads.html).
2. Unzip the package
3. Add the path to the Terraform folder to your `PATH` environment variable. (stackoverflow tutorial [here](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows))
4. Verify installation. Run `$ terraform -help`.

Check out [Hashicorp's Install Terraform Tutorial](https://learn.hashicorp.com/terraform/getting-started/install#install-terraform) for other installation methods, and installation for other OS)

## 3. Using Terraform
### 3.1 Set up credentials
This guide involve creating AWS resources, therefore you need to add configurations so that Terraform has permissions to access AWS.

1. Install the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
2. Run `$ aws configure`.
3. When prompted, input your AWS credentials that can be found [here](https://console.aws.amazon.com/iam/home?#/security_credentials).

This stores your credentials at `%UserProfile%\.aws\credentials`. This is where Terraform will check for credentials by default.

### 3.2 Create infrastructure

Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/build).

You might have to change the region and ami in the tutorial. ([Finding a Quick Start AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html#finding-quick-start-ami))

Make sure you can do `terraform init`, `terraform plan` and `terraform apply`.

#### Warning: 
- Avoid using git branches
- Avoid changing the .tfstate file directly
Doing the above can result in the real world not matching your .tfstate, and that will give you weird errors when you try `terraform plan`. You can also end up with unused resources not being destroyed.

### 3.3 Change infrastructure

Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/change)

Understand that when you run `terraform plan` or `apply`, Terraform automatically checks the difference between the infrastructure described in the `.tfstate` file and the infrastructure described in all the `.tf` files in the directory.

### 3.4 Destroy infrastructure

Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/destroy).

**Remember to do `terraform destroy` after you're done with any tutorial.**

### 3.5 Resource dependencies
#### Implicit dependencies
Terraform automatically recognises which resources are dependent of which and will build them in the right order. 
#### Explicit dependencies
Sometimes there are hidden dependencies not visable to Terraform. You can specify those dependencies with `depends_on`.
#### Non-dependent resources
Non-dependent resources will be built in parallel with other resources

More info on [Hashicorp website](https://learn.hashicorp.com/terraform/getting-started/dependencies).

### 3.6 Provisioners
Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/provision).

**Remember to do `terraform destroy` after you're done.**

### 3.7 Input and Output Variables
1. Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/variables) to learn about 
   - defining variables and 
   - assigning variables.
2. Follow this [tutorial](https://learn.hashicorp.com/terraform/getting-started/outputs) to learn about outputs

**Remember to do `terraform destroy` after you're done.**

## 4. Using Terraform as a Team
### 4.1 Remote state
The `.tfstate` files can be stored remotely so that everyone from a team uses the same state. This guide will use AWS S3 buckets.

1. Create an S3 bucket and create a folder to store your terraform files. 
2. Add this to your main.tf
```
terraform {
  backend "s3" {
    bucket = "<bucket name>"
    key    = "path/to/my/key"
    region = "ap-southeast-1"
  }
}
```
3. After doing `terraform apply` you should see a `.tfstate` file in the folder in the bucket you created.

More info [here](https://www.terraform.io/docs/backends/types/s3.html).

### 4.2 State locking
State locking can be used to prevent race conditions when multiple people are trying to change the state at the same time.

To enable this:
1. Create a table in DynamoDB.
2. Set the `dynamodb_table` field in the terraform backend config to an existing DynammoDB table name.

**Remember to do `terraform destroy` after you're done.**

## 5. Modules
Modules are a way to reuse configurations to avoid duplicate code. Read [this](https://learn.hashicorp.com/terraform/modules/modules-overview) for an intro. 

### 5.1 Using pre-existing modules
Follow this [tutorial](https://learn.hashicorp.com/terraform/modules/using-modules) to learn about how to use modules from the Terraform Registry.

Remember to **avoid** using git branches. If you do use branches, remember to do `terraform destroy` before switching branches.

### 5.2 Creating modules
Follow this [tutorial](https://learn.hashicorp.com/terraform/modules/creating-modules) to learn how to create your own modules.

Remember to **avoid** using git branches. If you do use branches, remember to do `terraform destroy` before switching branches.

### 5.3 Hosting modules
You can host modules on the **public** Terraform Registry or a **private registry**.

#### Terraform registry (public)
You can publish public modules to terraform registry directly from a public GitHub repo. As long as your repo meets some requirements. (See [link](https://www.terraform.io/docs/registry/modules/publish.html))

#### Private registry
You need a Terraform Cloud account to have private modules. See [tutorial](https://learn.hashicorp.com/terraform/modules/private-modules).

## 6. Code Organisation
### 6.1 Workspaces
### 6.2 File Layout

