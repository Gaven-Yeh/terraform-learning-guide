# terraform-learning-guide

## Contents
1. Intro
2. Installing Terraform
3. How to use Terraform
   1. Set up credentials
   2. Create infrastructure
   3. Change infrastructure
   4. Destroy infrastructure
   5. Resource Dependencies
   6. Provisioners
   7. Input and Output Variables
4. Using Terraform as a Team
   1. Remote State
   2. State Locking
5. Modules
   1. Using pre-existing modules
   2. Creating modules
   3. Hosting modules
6. Code organisation
   1. Workspaces
   2. File layout

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

https://learn.hashicorp.com/terraform/getting-started/provision

**Remember to do `terraform destroy` after you're done.**

### 3.7 Input and Output Variables
https://learn.hashicorp.com/terraform/getting-started/variables
https://learn.hashicorp.com/terraform/getting-started/outputs
