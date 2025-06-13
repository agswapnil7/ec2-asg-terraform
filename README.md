## EC2 Deployment via Terraform + Jenkins (Auto Scaling Group with user_data)

This project demonstrates how to automatically deploy an EC2 instance using Terraform through a Jenkins pipeline. The EC2 is provisioned via a Launch Template and Auto Scaling Group (ASG) and deleted automatically after testing

---

### What is being tested/proved?
- Automating infrastructure provisioning via Jenkins
- Terraform deployment of EC2 with ASG and user_data
- Automated clean-up (terraform destroy)

---

### Tools & Technologies
- Terraform
- AWS (VPC, Subnet, ASG, Launch Template, EC2)
- Jenkins (Declarative Pipeline)

---

### Success Criteria
- Jenkins applies Terraform successfully.
- EC2 launches with Apache installed and a custom index page.
- Infrastructure is cleaned up (destroyed) at the end.

---

### File Structure
```
├── main.tf          # Terraform code to deploy infrastructure
├── Jenkinsfile      # Jenkins pipeline to automate apply & destroy
├── README.md        # This documentation
```

---

### How to Reproduce

#### 1. Clone this repo
```bash
git clone <REPO_URL>
cd <REPO_NAME>
```

#### 2. Create Jenkins Pipeline Job
- Go to Jenkins > New Item > Pipeline
- Under **Pipeline Script**, use the `Jenkinsfile` from this repo.
- Add AWS credentials (e.g., via environment variables or credentials plugin).

#### 3. Run the Jenkins Job
- Click **Build Now**.
- Observe output in **Console Output**.

#### 4. Expected Output
- An EC2 instance created with Apache installed.
- A message at `/var/www/html/index.html`.
- After 2 minutes, the instance is destroyed.

---

### Auto-Cleanup
- The pipeline waits for 2 minutes and destroys the resources.
- This helps avoid unnecessary AWS charges.

---

### Jenkins Output
Refer to `console_output.txt` for full output from Jenkins.

Started by user admin
Obtained Jenkinsfile from git https://github.com/agswapnil7/ec2-asg-terraform.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in C:\Users\swapn\.jenkins\workspace\ec2-terrraform-asgg
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential github-creds
Cloning the remote Git repository
Cloning repository https://github.com/agswapnil7/ec2-asg-terraform.git
 > git.exe init C:\Users\swapn\.jenkins\workspace\ec2-terrraform-asgg # timeout=10
Fetching upstream changes from https://github.com/agswapnil7/ec2-asg-terraform.git
 > git.exe --version # timeout=10
 > git --version # 'git version 2.44.0.windows.1'
using GIT_ASKPASS to set credentials 
 > git.exe fetch --tags --force --progress -- https://github.com/agswapnil7/ec2-asg-terraform.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git.exe config remote.origin.url https://github.com/agswapnil7/ec2-asg-terraform.git # timeout=10
 > git.exe config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10
Checking out Revision 3d2a2a44c1b443c5b52de183805a3c8041bbd71a (refs/remotes/origin/main)
 > git.exe config core.sparsecheckout # timeout=10
 > git.exe checkout -f 3d2a2a44c1b443c5b52de183805a3c8041bbd71a # timeout=10
Commit message: "Update main.tf  ami ID"
First time build. Skipping changelog.
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Terraform Init)
[Pipeline] sh
+ terraform init

[0m[1mInitializing the backend...[0m

[0m[1mInitializing provider plugins...[0m
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v5.100.0...
- Installed hashicorp/aws v5.100.0 (signed by HashiCorp)

Terraform has created a lock file [1m.terraform.lock.hcl[0m to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.[0m

[0m[1m[32mTerraform has been successfully initialized![0m[32m[0m
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Terraform Plan)
[Pipeline] sh
+ terraform plan

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  [32m+[0m create[0m

Terraform will perform the following actions:

[1m  # aws_autoscaling_group.web_asg[0m will be created
[0m  [32m+[0m[0m resource "aws_autoscaling_group" "web_asg" {
      [32m+[0m[0m arn                              = (known after apply)
      [32m+[0m[0m availability_zones               = (known after apply)
      [32m+[0m[0m default_cooldown                 = (known after apply)
      [32m+[0m[0m desired_capacity                 = 1
      [32m+[0m[0m force_delete                     = false
      [32m+[0m[0m force_delete_warm_pool           = false
      [32m+[0m[0m health_check_grace_period        = 300
      [32m+[0m[0m health_check_type                = (known after apply)
      [32m+[0m[0m id                               = (known after apply)
      [32m+[0m[0m ignore_failed_scaling_activities = false
      [32m+[0m[0m load_balancers                   = (known after apply)
      [32m+[0m[0m max_size                         = 1
      [32m+[0m[0m metrics_granularity              = "1Minute"
      [32m+[0m[0m min_size                         = 1
      [32m+[0m[0m name                             = (known after apply)
      [32m+[0m[0m name_prefix                      = (known after apply)
      [32m+[0m[0m predicted_capacity               = (known after apply)
      [32m+[0m[0m protect_from_scale_in            = false
      [32m+[0m[0m service_linked_role_arn          = (known after apply)
      [32m+[0m[0m target_group_arns                = (known after apply)
      [32m+[0m[0m vpc_zone_identifier              = (known after apply)
      [32m+[0m[0m wait_for_capacity_timeout        = "10m"
      [32m+[0m[0m warm_pool_size                   = (known after apply)

      [32m+[0m[0m launch_template {
          [32m+[0m[0m id      = (known after apply)
          [32m+[0m[0m name    = (known after apply)
          [32m+[0m[0m version = "$Latest"
        }

      [32m+[0m[0m tag {
          [32m+[0m[0m key                 = "Name"
          [32m+[0m[0m propagate_at_launch = true
          [32m+[0m[0m value               = "WebServer"
        }
    }

[1m  # aws_internet_gateway.gw[0m will be created
[0m  [32m+[0m[0m resource "aws_internet_gateway" "gw" {
      [32m+[0m[0m arn      = (known after apply)
      [32m+[0m[0m id       = (known after apply)
      [32m+[0m[0m owner_id = (known after apply)
      [32m+[0m[0m tags_all = (known after apply)
      [32m+[0m[0m vpc_id   = (known after apply)
    }

[1m  # aws_launch_template.web[0m will be created
[0m  [32m+[0m[0m resource "aws_launch_template" "web" {
      [32m+[0m[0m arn                    = (known after apply)
      [32m+[0m[0m default_version        = (known after apply)
      [32m+[0m[0m id                     = (known after apply)
      [32m+[0m[0m image_id               = "ami-09e6f87a47903347c"
      [32m+[0m[0m instance_type          = "t2.micro"
      [32m+[0m[0m latest_version         = (known after apply)
      [32m+[0m[0m name                   = (known after apply)
      [32m+[0m[0m name_prefix            = "web-lt-"
      [32m+[0m[0m tags_all               = (known after apply)
      [32m+[0m[0m user_data              = "IyEvYmluL2Jhc2gNCnN1ZG8geXVtIHVwZGF0ZSAteQ0KZWNobyAiSGVsbG8gZnJvbSBFQzIgd2l0aCBBU0cgYW5kIFVzZXJEYXRhISIgPiAvdmFyL3d3dy9odG1sL2luZGV4Lmh0bWwNCnN1ZG8geXVtIGluc3RhbGwgLXkgaHR0cGQNCnN5c3RlbWN0bCBzdGFydCBodHRwZA0Kc3lzdGVtY3RsIGVuYWJsZSBodHRwZA0K"
      [32m+[0m[0m vpc_security_group_ids = (known after apply)
    }

[1m  # aws_route_table.rt[0m will be created
[0m  [32m+[0m[0m resource "aws_route_table" "rt" {
      [32m+[0m[0m arn              = (known after apply)
      [32m+[0m[0m id               = (known after apply)
      [32m+[0m[0m owner_id         = (known after apply)
      [32m+[0m[0m propagating_vgws = (known after apply)
      [32m+[0m[0m route            = [
          [32m+[0m[0m {
              [32m+[0m[0m carrier_gateway_id         = ""
              [32m+[0m[0m cidr_block                 = "0.0.0.0/0"
              [32m+[0m[0m core_network_arn           = ""
              [32m+[0m[0m destination_prefix_list_id = ""
              [32m+[0m[0m egress_only_gateway_id     = ""
              [32m+[0m[0m gateway_id                 = (known after apply)
              [32m+[0m[0m ipv6_cidr_block            = ""
              [32m+[0m[0m local_gateway_id           = ""
              [32m+[0m[0m nat_gateway_id             = ""
              [32m+[0m[0m network_interface_id       = ""
              [32m+[0m[0m transit_gateway_id         = ""
              [32m+[0m[0m vpc_endpoint_id            = ""
              [32m+[0m[0m vpc_peering_connection_id  = ""
            },
        ]
      [32m+[0m[0m tags_all         = (known after apply)
      [32m+[0m[0m vpc_id           = (known after apply)
    }

[1m  # aws_route_table_association.a[0m will be created
[0m  [32m+[0m[0m resource "aws_route_table_association" "a" {
      [32m+[0m[0m id             = (known after apply)
      [32m+[0m[0m route_table_id = (known after apply)
      [32m+[0m[0m subnet_id      = (known after apply)
    }

[1m  # aws_security_group.web_sg[0m will be created
[0m  [32m+[0m[0m resource "aws_security_group" "web_sg" {
      [32m+[0m[0m arn                    = (known after apply)
      [32m+[0m[0m description            = "Allow HTTP and SSH"
      [32m+[0m[0m egress                 = [
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 0
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "-1"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 0
            },
        ]
      [32m+[0m[0m id                     = (known after apply)
      [32m+[0m[0m ingress                = [
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 22
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "tcp"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 22
            },
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 80
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "tcp"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 80
            },
        ]
      [32m+[0m[0m name                   = "web-sg"
      [32m+[0m[0m name_prefix            = (known after apply)
      [32m+[0m[0m owner_id               = (known after apply)
      [32m+[0m[0m revoke_rules_on_delete = false
      [32m+[0m[0m tags_all               = (known after apply)
      [32m+[0m[0m vpc_id                 = (known after apply)
    }

[1m  # aws_subnet.main[0m will be created
[0m  [32m+[0m[0m resource "aws_subnet" "main" {
      [32m+[0m[0m arn                                            = (known after apply)
      [32m+[0m[0m assign_ipv6_address_on_creation                = false
      [32m+[0m[0m availability_zone                              = "us-east-1a"
      [32m+[0m[0m availability_zone_id                           = (known after apply)
      [32m+[0m[0m cidr_block                                     = "10.0.1.0/24"
      [32m+[0m[0m enable_dns64                                   = false
      [32m+[0m[0m enable_resource_name_dns_a_record_on_launch    = false
      [32m+[0m[0m enable_resource_name_dns_aaaa_record_on_launch = false
      [32m+[0m[0m id                                             = (known after apply)
      [32m+[0m[0m ipv6_cidr_block_association_id                 = (known after apply)
      [32m+[0m[0m ipv6_native                                    = false
      [32m+[0m[0m map_public_ip_on_launch                        = true
      [32m+[0m[0m owner_id                                       = (known after apply)
      [32m+[0m[0m private_dns_hostname_type_on_launch            = (known after apply)
      [32m+[0m[0m tags_all                                       = (known after apply)
      [32m+[0m[0m vpc_id                                         = (known after apply)
    }

[1m  # aws_vpc.main[0m will be created
[0m  [32m+[0m[0m resource "aws_vpc" "main" {
      [32m+[0m[0m arn                                  = (known after apply)
      [32m+[0m[0m cidr_block                           = "10.0.0.0/16"
      [32m+[0m[0m default_network_acl_id               = (known after apply)
      [32m+[0m[0m default_route_table_id               = (known after apply)
      [32m+[0m[0m default_security_group_id            = (known after apply)
      [32m+[0m[0m dhcp_options_id                      = (known after apply)
      [32m+[0m[0m enable_dns_hostnames                 = (known after apply)
      [32m+[0m[0m enable_dns_support                   = true
      [32m+[0m[0m enable_network_address_usage_metrics = (known after apply)
      [32m+[0m[0m id                                   = (known after apply)
      [32m+[0m[0m instance_tenancy                     = "default"
      [32m+[0m[0m ipv6_association_id                  = (known after apply)
      [32m+[0m[0m ipv6_cidr_block                      = (known after apply)
      [32m+[0m[0m ipv6_cidr_block_network_border_group = (known after apply)
      [32m+[0m[0m main_route_table_id                  = (known after apply)
      [32m+[0m[0m owner_id                             = (known after apply)
      [32m+[0m[0m tags_all                             = (known after apply)
    }

[1mPlan:[0m 8 to add, 0 to change, 0 to destroy.
[0m
Changes to Outputs:
  [32m+[0m[0m public_subnet_id = (known after apply)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Terraform Apply)
[Pipeline] sh
+ terraform apply -auto-approve

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  [32m+[0m create[0m

Terraform will perform the following actions:

[1m  # aws_autoscaling_group.web_asg[0m will be created
[0m  [32m+[0m[0m resource "aws_autoscaling_group" "web_asg" {
      [32m+[0m[0m arn                              = (known after apply)
      [32m+[0m[0m availability_zones               = (known after apply)
      [32m+[0m[0m default_cooldown                 = (known after apply)
      [32m+[0m[0m desired_capacity                 = 1
      [32m+[0m[0m force_delete                     = false
      [32m+[0m[0m force_delete_warm_pool           = false
      [32m+[0m[0m health_check_grace_period        = 300
      [32m+[0m[0m health_check_type                = (known after apply)
      [32m+[0m[0m id                               = (known after apply)
      [32m+[0m[0m ignore_failed_scaling_activities = false
      [32m+[0m[0m load_balancers                   = (known after apply)
      [32m+[0m[0m max_size                         = 1
      [32m+[0m[0m metrics_granularity              = "1Minute"
      [32m+[0m[0m min_size                         = 1
      [32m+[0m[0m name                             = (known after apply)
      [32m+[0m[0m name_prefix                      = (known after apply)
      [32m+[0m[0m predicted_capacity               = (known after apply)
      [32m+[0m[0m protect_from_scale_in            = false
      [32m+[0m[0m service_linked_role_arn          = (known after apply)
      [32m+[0m[0m target_group_arns                = (known after apply)
      [32m+[0m[0m vpc_zone_identifier              = (known after apply)
      [32m+[0m[0m wait_for_capacity_timeout        = "10m"
      [32m+[0m[0m warm_pool_size                   = (known after apply)

      [32m+[0m[0m launch_template {
          [32m+[0m[0m id      = (known after apply)
          [32m+[0m[0m name    = (known after apply)
          [32m+[0m[0m version = "$Latest"
        }

      [32m+[0m[0m tag {
          [32m+[0m[0m key                 = "Name"
          [32m+[0m[0m propagate_at_launch = true
          [32m+[0m[0m value               = "WebServer"
        }
    }

[1m  # aws_internet_gateway.gw[0m will be created
[0m  [32m+[0m[0m resource "aws_internet_gateway" "gw" {
      [32m+[0m[0m arn      = (known after apply)
      [32m+[0m[0m id       = (known after apply)
      [32m+[0m[0m owner_id = (known after apply)
      [32m+[0m[0m tags_all = (known after apply)
      [32m+[0m[0m vpc_id   = (known after apply)
    }

[1m  # aws_launch_template.web[0m will be created
[0m  [32m+[0m[0m resource "aws_launch_template" "web" {
      [32m+[0m[0m arn                    = (known after apply)
      [32m+[0m[0m default_version        = (known after apply)
      [32m+[0m[0m id                     = (known after apply)
      [32m+[0m[0m image_id               = "ami-09e6f87a47903347c"
      [32m+[0m[0m instance_type          = "t2.micro"
      [32m+[0m[0m latest_version         = (known after apply)
      [32m+[0m[0m name                   = (known after apply)
      [32m+[0m[0m name_prefix            = "web-lt-"
      [32m+[0m[0m tags_all               = (known after apply)
      [32m+[0m[0m user_data              = "IyEvYmluL2Jhc2gNCnN1ZG8geXVtIHVwZGF0ZSAteQ0KZWNobyAiSGVsbG8gZnJvbSBFQzIgd2l0aCBBU0cgYW5kIFVzZXJEYXRhISIgPiAvdmFyL3d3dy9odG1sL2luZGV4Lmh0bWwNCnN1ZG8geXVtIGluc3RhbGwgLXkgaHR0cGQNCnN5c3RlbWN0bCBzdGFydCBodHRwZA0Kc3lzdGVtY3RsIGVuYWJsZSBodHRwZA0K"
      [32m+[0m[0m vpc_security_group_ids = (known after apply)
    }

[1m  # aws_route_table.rt[0m will be created
[0m  [32m+[0m[0m resource "aws_route_table" "rt" {
      [32m+[0m[0m arn              = (known after apply)
      [32m+[0m[0m id               = (known after apply)
      [32m+[0m[0m owner_id         = (known after apply)
      [32m+[0m[0m propagating_vgws = (known after apply)
      [32m+[0m[0m route            = [
          [32m+[0m[0m {
              [32m+[0m[0m carrier_gateway_id         = ""
              [32m+[0m[0m cidr_block                 = "0.0.0.0/0"
              [32m+[0m[0m core_network_arn           = ""
              [32m+[0m[0m destination_prefix_list_id = ""
              [32m+[0m[0m egress_only_gateway_id     = ""
              [32m+[0m[0m gateway_id                 = (known after apply)
              [32m+[0m[0m ipv6_cidr_block            = ""
              [32m+[0m[0m local_gateway_id           = ""
              [32m+[0m[0m nat_gateway_id             = ""
              [32m+[0m[0m network_interface_id       = ""
              [32m+[0m[0m transit_gateway_id         = ""
              [32m+[0m[0m vpc_endpoint_id            = ""
              [32m+[0m[0m vpc_peering_connection_id  = ""
            },
        ]
      [32m+[0m[0m tags_all         = (known after apply)
      [32m+[0m[0m vpc_id           = (known after apply)
    }

[1m  # aws_route_table_association.a[0m will be created
[0m  [32m+[0m[0m resource "aws_route_table_association" "a" {
      [32m+[0m[0m id             = (known after apply)
      [32m+[0m[0m route_table_id = (known after apply)
      [32m+[0m[0m subnet_id      = (known after apply)
    }

[1m  # aws_security_group.web_sg[0m will be created
[0m  [32m+[0m[0m resource "aws_security_group" "web_sg" {
      [32m+[0m[0m arn                    = (known after apply)
      [32m+[0m[0m description            = "Allow HTTP and SSH"
      [32m+[0m[0m egress                 = [
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 0
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "-1"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 0
            },
        ]
      [32m+[0m[0m id                     = (known after apply)
      [32m+[0m[0m ingress                = [
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 22
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "tcp"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 22
            },
          [32m+[0m[0m {
              [32m+[0m[0m cidr_blocks      = [
                  [32m+[0m[0m "0.0.0.0/0",
                ]
              [32m+[0m[0m description      = ""
              [32m+[0m[0m from_port        = 80
              [32m+[0m[0m ipv6_cidr_blocks = []
              [32m+[0m[0m prefix_list_ids  = []
              [32m+[0m[0m protocol         = "tcp"
              [32m+[0m[0m security_groups  = []
              [32m+[0m[0m self             = false
              [32m+[0m[0m to_port          = 80
            },
        ]
      [32m+[0m[0m name                   = "web-sg"
      [32m+[0m[0m name_prefix            = (known after apply)
      [32m+[0m[0m owner_id               = (known after apply)
      [32m+[0m[0m revoke_rules_on_delete = false
      [32m+[0m[0m tags_all               = (known after apply)
      [32m+[0m[0m vpc_id                 = (known after apply)
    }

[1m  # aws_subnet.main[0m will be created
[0m  [32m+[0m[0m resource "aws_subnet" "main" {
      [32m+[0m[0m arn                                            = (known after apply)
      [32m+[0m[0m assign_ipv6_address_on_creation                = false
      [32m+[0m[0m availability_zone                              = "us-east-1a"
      [32m+[0m[0m availability_zone_id                           = (known after apply)
      [32m+[0m[0m cidr_block                                     = "10.0.1.0/24"
      [32m+[0m[0m enable_dns64                                   = false
      [32m+[0m[0m enable_resource_name_dns_a_record_on_launch    = false
      [32m+[0m[0m enable_resource_name_dns_aaaa_record_on_launch = false
      [32m+[0m[0m id                                             = (known after apply)
      [32m+[0m[0m ipv6_cidr_block_association_id                 = (known after apply)
      [32m+[0m[0m ipv6_native                                    = false
      [32m+[0m[0m map_public_ip_on_launch                        = true
      [32m+[0m[0m owner_id                                       = (known after apply)
      [32m+[0m[0m private_dns_hostname_type_on_launch            = (known after apply)
      [32m+[0m[0m tags_all                                       = (known after apply)
      [32m+[0m[0m vpc_id                                         = (known after apply)
    }

[1m  # aws_vpc.main[0m will be created
[0m  [32m+[0m[0m resource "aws_vpc" "main" {
      [32m+[0m[0m arn                                  = (known after apply)
      [32m+[0m[0m cidr_block                           = "10.0.0.0/16"
      [32m+[0m[0m default_network_acl_id               = (known after apply)
      [32m+[0m[0m default_route_table_id               = (known after apply)
      [32m+[0m[0m default_security_group_id            = (known after apply)
      [32m+[0m[0m dhcp_options_id                      = (known after apply)
      [32m+[0m[0m enable_dns_hostnames                 = (known after apply)
      [32m+[0m[0m enable_dns_support                   = true
      [32m+[0m[0m enable_network_address_usage_metrics = (known after apply)
      [32m+[0m[0m id                                   = (known after apply)
      [32m+[0m[0m instance_tenancy                     = "default"
      [32m+[0m[0m ipv6_association_id                  = (known after apply)
      [32m+[0m[0m ipv6_cidr_block                      = (known after apply)
      [32m+[0m[0m ipv6_cidr_block_network_border_group = (known after apply)
      [32m+[0m[0m main_route_table_id                  = (known after apply)
      [32m+[0m[0m owner_id                             = (known after apply)
      [32m+[0m[0m tags_all                             = (known after apply)
    }

[1mPlan:[0m 8 to add, 0 to change, 0 to destroy.
[0m
Changes to Outputs:
  [32m+[0m[0m public_subnet_id = (known after apply)
[0m[1maws_vpc.main: Creating...[0m[0m
[0m[1maws_vpc.main: Creation complete after 5s [id=vpc-0561f5e795a4e1fbb][0m
[0m[1maws_internet_gateway.gw: Creating...[0m[0m
[0m[1maws_subnet.main: Creating...[0m[0m
[0m[1maws_security_group.web_sg: Creating...[0m[0m
[0m[1maws_internet_gateway.gw: Creation complete after 3s [id=igw-03da99aa2e05f5184][0m
[0m[1maws_route_table.rt: Creating...[0m[0m
[0m[1maws_route_table.rt: Creation complete after 3s [id=rtb-0c2d410489c0acdbf][0m
[0m[1maws_security_group.web_sg: Creation complete after 7s [id=sg-023de51a7106062b5][0m
[0m[1maws_launch_template.web: Creating...[0m[0m
[0m[1maws_subnet.main: Still creating... [10s elapsed][0m[0m
[0m[1maws_subnet.main: Creation complete after 14s [id=subnet-029b05a2fe1294ee5][0m
[0m[1maws_route_table_association.a: Creating...[0m[0m
[0m[1maws_launch_template.web: Creation complete after 8s [id=lt-0a5d61d4b7eb4db36][0m
[0m[1maws_autoscaling_group.web_asg: Creating...[0m[0m
[0m[1maws_route_table_association.a: Creation complete after 1s [id=rtbassoc-050543b1173abcf8c][0m
[0m[1maws_autoscaling_group.web_asg: Still creating... [10s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still creating... [20s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still creating... [30s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still creating... [40s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Creation complete after 46s [id=terraform-20250613090658857900000004][0m
[0m[1m[32m
Apply complete! Resources: 8 added, 0 changed, 0 destroyed.
[0m[0m[1m[32m
Outputs:

[0mpublic_subnet_id = "subnet-029b05a2fe1294ee5"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Wait)
[Pipeline] echo
Sleeping for 2 minutes to simulate testing...
[Pipeline] sh
+ sleep 120
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Terraform Destroy)
[Pipeline] sh
+ terraform destroy -auto-approve
[0m[1maws_vpc.main: Refreshing state... [id=vpc-0561f5e795a4e1fbb][0m
[0m[1maws_internet_gateway.gw: Refreshing state... [id=igw-03da99aa2e05f5184][0m
[0m[1maws_subnet.main: Refreshing state... [id=subnet-029b05a2fe1294ee5][0m
[0m[1maws_security_group.web_sg: Refreshing state... [id=sg-023de51a7106062b5][0m
[0m[1maws_route_table.rt: Refreshing state... [id=rtb-0c2d410489c0acdbf][0m
[0m[1maws_launch_template.web: Refreshing state... [id=lt-0a5d61d4b7eb4db36][0m
[0m[1maws_route_table_association.a: Refreshing state... [id=rtbassoc-050543b1173abcf8c][0m
[0m[1maws_autoscaling_group.web_asg: Refreshing state... [id=terraform-20250613090658857900000004][0m

Terraform used the selected providers to generate the following execution
plan. Resource actions are indicated with the following symbols:
  [31m-[0m destroy[0m

Terraform will perform the following actions:

[1m  # aws_autoscaling_group.web_asg[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_autoscaling_group" "web_asg" {
      [31m-[0m[0m arn                              = "arn:aws:autoscaling:us-east-1:730335211981:autoScalingGroup:b8b2c415-5577-4870-8de5-3d27b556f622:autoScalingGroupName/terraform-20250613090658857900000004" [90m-> null[0m[0m
      [31m-[0m[0m availability_zones               = [
          [31m-[0m[0m "us-east-1a",
        ] [90m-> null[0m[0m
      [31m-[0m[0m capacity_rebalance               = false [90m-> null[0m[0m
      [31m-[0m[0m default_cooldown                 = 300 [90m-> null[0m[0m
      [31m-[0m[0m default_instance_warmup          = 0 [90m-> null[0m[0m
      [31m-[0m[0m desired_capacity                 = 1 [90m-> null[0m[0m
      [31m-[0m[0m enabled_metrics                  = [] [90m-> null[0m[0m
      [31m-[0m[0m force_delete                     = false [90m-> null[0m[0m
      [31m-[0m[0m force_delete_warm_pool           = false [90m-> null[0m[0m
      [31m-[0m[0m health_check_grace_period        = 300 [90m-> null[0m[0m
      [31m-[0m[0m health_check_type                = "EC2" [90m-> null[0m[0m
      [31m-[0m[0m id                               = "terraform-20250613090658857900000004" [90m-> null[0m[0m
      [31m-[0m[0m ignore_failed_scaling_activities = false [90m-> null[0m[0m
      [31m-[0m[0m load_balancers                   = [] [90m-> null[0m[0m
      [31m-[0m[0m max_instance_lifetime            = 0 [90m-> null[0m[0m
      [31m-[0m[0m max_size                         = 1 [90m-> null[0m[0m
      [31m-[0m[0m metrics_granularity              = "1Minute" [90m-> null[0m[0m
      [31m-[0m[0m min_size                         = 1 [90m-> null[0m[0m
      [31m-[0m[0m name                             = "terraform-20250613090658857900000004" [90m-> null[0m[0m
      [31m-[0m[0m name_prefix                      = "terraform-" [90m-> null[0m[0m
      [31m-[0m[0m predicted_capacity               = 0 [90m-> null[0m[0m
      [31m-[0m[0m protect_from_scale_in            = false [90m-> null[0m[0m
      [31m-[0m[0m service_linked_role_arn          = "arn:aws:iam::730335211981:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling" [90m-> null[0m[0m
      [31m-[0m[0m suspended_processes              = [] [90m-> null[0m[0m
      [31m-[0m[0m target_group_arns                = [] [90m-> null[0m[0m
      [31m-[0m[0m termination_policies             = [] [90m-> null[0m[0m
      [31m-[0m[0m vpc_zone_identifier              = [
          [31m-[0m[0m "subnet-029b05a2fe1294ee5",
        ] [90m-> null[0m[0m
      [31m-[0m[0m wait_for_capacity_timeout        = "10m" [90m-> null[0m[0m
      [31m-[0m[0m warm_pool_size                   = 0 [90m-> null[0m[0m

      [31m-[0m[0m availability_zone_distribution {
          [31m-[0m[0m capacity_distribution_strategy = "balanced-best-effort" [90m-> null[0m[0m
        }

      [31m-[0m[0m capacity_reservation_specification {
          [31m-[0m[0m capacity_reservation_preference = "default" [90m-> null[0m[0m
        }

      [31m-[0m[0m launch_template {
          [31m-[0m[0m id      = "lt-0a5d61d4b7eb4db36" [90m-> null[0m[0m
          [31m-[0m[0m name    = "web-lt-20250613090651413700000002" [90m-> null[0m[0m
          [31m-[0m[0m version = "$Latest" [90m-> null[0m[0m
        }

      [31m-[0m[0m tag {
          [31m-[0m[0m key                 = "Name" [90m-> null[0m[0m
          [31m-[0m[0m propagate_at_launch = true [90m-> null[0m[0m
          [31m-[0m[0m value               = "WebServer" [90m-> null[0m[0m
        }
    }

[1m  # aws_internet_gateway.gw[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_internet_gateway" "gw" {
      [31m-[0m[0m arn      = "arn:aws:ec2:us-east-1:730335211981:internet-gateway/igw-03da99aa2e05f5184" [90m-> null[0m[0m
      [31m-[0m[0m id       = "igw-03da99aa2e05f5184" [90m-> null[0m[0m
      [31m-[0m[0m owner_id = "730335211981" [90m-> null[0m[0m
      [31m-[0m[0m tags     = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all = {} [90m-> null[0m[0m
      [31m-[0m[0m vpc_id   = "vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
    }

[1m  # aws_launch_template.web[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_launch_template" "web" {
      [31m-[0m[0m arn                     = "arn:aws:ec2:us-east-1:730335211981:launch-template/lt-0a5d61d4b7eb4db36" [90m-> null[0m[0m
      [31m-[0m[0m default_version         = 1 [90m-> null[0m[0m
      [31m-[0m[0m disable_api_stop        = false [90m-> null[0m[0m
      [31m-[0m[0m disable_api_termination = false [90m-> null[0m[0m
      [31m-[0m[0m id                      = "lt-0a5d61d4b7eb4db36" [90m-> null[0m[0m
      [31m-[0m[0m image_id                = "ami-09e6f87a47903347c" [90m-> null[0m[0m
      [31m-[0m[0m instance_type           = "t2.micro" [90m-> null[0m[0m
      [31m-[0m[0m latest_version          = 1 [90m-> null[0m[0m
      [31m-[0m[0m name                    = "web-lt-20250613090651413700000002" [90m-> null[0m[0m
      [31m-[0m[0m name_prefix             = "web-lt-" [90m-> null[0m[0m
      [31m-[0m[0m security_group_names    = [] [90m-> null[0m[0m
      [31m-[0m[0m tags                    = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all                = {} [90m-> null[0m[0m
      [31m-[0m[0m user_data               = "IyEvYmluL2Jhc2gNCnN1ZG8geXVtIHVwZGF0ZSAteQ0KZWNobyAiSGVsbG8gZnJvbSBFQzIgd2l0aCBBU0cgYW5kIFVzZXJEYXRhISIgPiAvdmFyL3d3dy9odG1sL2luZGV4Lmh0bWwNCnN1ZG8geXVtIGluc3RhbGwgLXkgaHR0cGQNCnN5c3RlbWN0bCBzdGFydCBodHRwZA0Kc3lzdGVtY3RsIGVuYWJsZSBodHRwZA0K" [90m-> null[0m[0m
      [31m-[0m[0m vpc_security_group_ids  = [
          [31m-[0m[0m "sg-023de51a7106062b5",
        ] [90m-> null[0m[0m
    }

[1m  # aws_route_table.rt[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_route_table" "rt" {
      [31m-[0m[0m arn              = "arn:aws:ec2:us-east-1:730335211981:route-table/rtb-0c2d410489c0acdbf" [90m-> null[0m[0m
      [31m-[0m[0m id               = "rtb-0c2d410489c0acdbf" [90m-> null[0m[0m
      [31m-[0m[0m owner_id         = "730335211981" [90m-> null[0m[0m
      [31m-[0m[0m propagating_vgws = [] [90m-> null[0m[0m
      [31m-[0m[0m route            = [
          [31m-[0m[0m {
              [31m-[0m[0m carrier_gateway_id         = ""
              [31m-[0m[0m cidr_block                 = "0.0.0.0/0"
              [31m-[0m[0m core_network_arn           = ""
              [31m-[0m[0m destination_prefix_list_id = ""
              [31m-[0m[0m egress_only_gateway_id     = ""
              [31m-[0m[0m gateway_id                 = "igw-03da99aa2e05f5184"
              [31m-[0m[0m ipv6_cidr_block            = ""
              [31m-[0m[0m local_gateway_id           = ""
              [31m-[0m[0m nat_gateway_id             = ""
              [31m-[0m[0m network_interface_id       = ""
              [31m-[0m[0m transit_gateway_id         = ""
              [31m-[0m[0m vpc_endpoint_id            = ""
              [31m-[0m[0m vpc_peering_connection_id  = ""
            },
        ] [90m-> null[0m[0m
      [31m-[0m[0m tags             = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all         = {} [90m-> null[0m[0m
      [31m-[0m[0m vpc_id           = "vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
    }

[1m  # aws_route_table_association.a[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_route_table_association" "a" {
      [31m-[0m[0m id             = "rtbassoc-050543b1173abcf8c" [90m-> null[0m[0m
      [31m-[0m[0m route_table_id = "rtb-0c2d410489c0acdbf" [90m-> null[0m[0m
      [31m-[0m[0m subnet_id      = "subnet-029b05a2fe1294ee5" [90m-> null[0m[0m
    }

[1m  # aws_security_group.web_sg[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_security_group" "web_sg" {
      [31m-[0m[0m arn                    = "arn:aws:ec2:us-east-1:730335211981:security-group/sg-023de51a7106062b5" [90m-> null[0m[0m
      [31m-[0m[0m description            = "Allow HTTP and SSH" [90m-> null[0m[0m
      [31m-[0m[0m egress                 = [
          [31m-[0m[0m {
              [31m-[0m[0m cidr_blocks      = [
                  [31m-[0m[0m "0.0.0.0/0",
                ]
              [31m-[0m[0m description      = ""
              [31m-[0m[0m from_port        = 0
              [31m-[0m[0m ipv6_cidr_blocks = []
              [31m-[0m[0m prefix_list_ids  = []
              [31m-[0m[0m protocol         = "-1"
              [31m-[0m[0m security_groups  = []
              [31m-[0m[0m self             = false
              [31m-[0m[0m to_port          = 0
            },
        ] [90m-> null[0m[0m
      [31m-[0m[0m id                     = "sg-023de51a7106062b5" [90m-> null[0m[0m
      [31m-[0m[0m ingress                = [
          [31m-[0m[0m {
              [31m-[0m[0m cidr_blocks      = [
                  [31m-[0m[0m "0.0.0.0/0",
                ]
              [31m-[0m[0m description      = ""
              [31m-[0m[0m from_port        = 22
              [31m-[0m[0m ipv6_cidr_blocks = []
              [31m-[0m[0m prefix_list_ids  = []
              [31m-[0m[0m protocol         = "tcp"
              [31m-[0m[0m security_groups  = []
              [31m-[0m[0m self             = false
              [31m-[0m[0m to_port          = 22
            },
          [31m-[0m[0m {
              [31m-[0m[0m cidr_blocks      = [
                  [31m-[0m[0m "0.0.0.0/0",
                ]
              [31m-[0m[0m description      = ""
              [31m-[0m[0m from_port        = 80
              [31m-[0m[0m ipv6_cidr_blocks = []
              [31m-[0m[0m prefix_list_ids  = []
              [31m-[0m[0m protocol         = "tcp"
              [31m-[0m[0m security_groups  = []
              [31m-[0m[0m self             = false
              [31m-[0m[0m to_port          = 80
            },
        ] [90m-> null[0m[0m
      [31m-[0m[0m name                   = "web-sg" [90m-> null[0m[0m
      [31m-[0m[0m owner_id               = "730335211981" [90m-> null[0m[0m
      [31m-[0m[0m revoke_rules_on_delete = false [90m-> null[0m[0m
      [31m-[0m[0m tags                   = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all               = {} [90m-> null[0m[0m
      [31m-[0m[0m vpc_id                 = "vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
    }

[1m  # aws_subnet.main[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_subnet" "main" {
      [31m-[0m[0m arn                                            = "arn:aws:ec2:us-east-1:730335211981:subnet/subnet-029b05a2fe1294ee5" [90m-> null[0m[0m
      [31m-[0m[0m assign_ipv6_address_on_creation                = false [90m-> null[0m[0m
      [31m-[0m[0m availability_zone                              = "us-east-1a" [90m-> null[0m[0m
      [31m-[0m[0m availability_zone_id                           = "use1-az1" [90m-> null[0m[0m
      [31m-[0m[0m cidr_block                                     = "10.0.1.0/24" [90m-> null[0m[0m
      [31m-[0m[0m enable_dns64                                   = false [90m-> null[0m[0m
      [31m-[0m[0m enable_lni_at_device_index                     = 0 [90m-> null[0m[0m
      [31m-[0m[0m enable_resource_name_dns_a_record_on_launch    = false [90m-> null[0m[0m
      [31m-[0m[0m enable_resource_name_dns_aaaa_record_on_launch = false [90m-> null[0m[0m
      [31m-[0m[0m id                                             = "subnet-029b05a2fe1294ee5" [90m-> null[0m[0m
      [31m-[0m[0m ipv6_native                                    = false [90m-> null[0m[0m
      [31m-[0m[0m map_customer_owned_ip_on_launch                = false [90m-> null[0m[0m
      [31m-[0m[0m map_public_ip_on_launch                        = true [90m-> null[0m[0m
      [31m-[0m[0m owner_id                                       = "730335211981" [90m-> null[0m[0m
      [31m-[0m[0m private_dns_hostname_type_on_launch            = "ip-name" [90m-> null[0m[0m
      [31m-[0m[0m tags                                           = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all                                       = {} [90m-> null[0m[0m
      [31m-[0m[0m vpc_id                                         = "vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
    }

[1m  # aws_vpc.main[0m will be [1m[31mdestroyed[0m
[0m  [31m-[0m[0m resource "aws_vpc" "main" {
      [31m-[0m[0m arn                                  = "arn:aws:ec2:us-east-1:730335211981:vpc/vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
      [31m-[0m[0m assign_generated_ipv6_cidr_block     = false [90m-> null[0m[0m
      [31m-[0m[0m cidr_block                           = "10.0.0.0/16" [90m-> null[0m[0m
      [31m-[0m[0m default_network_acl_id               = "acl-0325d301fb7a5d19e" [90m-> null[0m[0m
      [31m-[0m[0m default_route_table_id               = "rtb-0c6203e982f450b63" [90m-> null[0m[0m
      [31m-[0m[0m default_security_group_id            = "sg-062febe62ce116d0c" [90m-> null[0m[0m
      [31m-[0m[0m dhcp_options_id                      = "dopt-0e91d62fce6b625fb" [90m-> null[0m[0m
      [31m-[0m[0m enable_dns_hostnames                 = false [90m-> null[0m[0m
      [31m-[0m[0m enable_dns_support                   = true [90m-> null[0m[0m
      [31m-[0m[0m enable_network_address_usage_metrics = false [90m-> null[0m[0m
      [31m-[0m[0m id                                   = "vpc-0561f5e795a4e1fbb" [90m-> null[0m[0m
      [31m-[0m[0m instance_tenancy                     = "default" [90m-> null[0m[0m
      [31m-[0m[0m ipv6_netmask_length                  = 0 [90m-> null[0m[0m
      [31m-[0m[0m main_route_table_id                  = "rtb-0c6203e982f450b63" [90m-> null[0m[0m
      [31m-[0m[0m owner_id                             = "730335211981" [90m-> null[0m[0m
      [31m-[0m[0m tags                                 = {} [90m-> null[0m[0m
      [31m-[0m[0m tags_all                             = {} [90m-> null[0m[0m
    }

[1mPlan:[0m 0 to add, 0 to change, 8 to destroy.
[0m
Changes to Outputs:
  [31m-[0m[0m public_subnet_id = "subnet-029b05a2fe1294ee5" [90m-> null[0m[0m
[0m[1maws_route_table_association.a: Destroying... [id=rtbassoc-050543b1173abcf8c][0m[0m
[0m[1maws_autoscaling_group.web_asg: Destroying... [id=terraform-20250613090658857900000004][0m[0m
[0m[1maws_route_table_association.a: Destruction complete after 2s[0m
[0m[1maws_route_table.rt: Destroying... [id=rtb-0c2d410489c0acdbf][0m[0m
[0m[1maws_route_table.rt: Destruction complete after 4s[0m
[0m[1maws_internet_gateway.gw: Destroying... [id=igw-03da99aa2e05f5184][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 10s elapsed][0m[0m
[0m[1maws_internet_gateway.gw: Still destroying... [id=igw-03da99aa2e05f5184, 10s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 20s elapsed][0m[0m
[0m[1maws_internet_gateway.gw: Still destroying... [id=igw-03da99aa2e05f5184, 20s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 30s elapsed][0m[0m
[0m[1maws_internet_gateway.gw: Destruction complete after 28s[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 40s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 50s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Still destroying... [id=terraform-20250613090658857900000004, 1m0s elapsed][0m[0m
[0m[1maws_autoscaling_group.web_asg: Destruction complete after 1m3s[0m
[0m[1maws_subnet.main: Destroying... [id=subnet-029b05a2fe1294ee5][0m[0m
[0m[1maws_launch_template.web: Destroying... [id=lt-0a5d61d4b7eb4db36][0m[0m
[0m[1maws_launch_template.web: Destruction complete after 0s[0m
[0m[1maws_security_group.web_sg: Destroying... [id=sg-023de51a7106062b5][0m[0m
[0m[1maws_subnet.main: Destruction complete after 1s[0m
[0m[1maws_security_group.web_sg: Destruction complete after 3s[0m
[0m[1maws_vpc.main: Destroying... [id=vpc-0561f5e795a4e1fbb][0m[0m
[0m[1maws_vpc.main: Destruction complete after 1s[0m
[0m[1m[32m
Destroy complete! Resources: 8 destroyed.
[0m
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
---
