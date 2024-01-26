# Terraform Project
## Kanban 3-tiers tool on Ec2 docker



3 tiers kanban tools running on Ec2 instance with docker-compose


## Infrastructure

- EC2 with docker installation (installed with userdata script)
- NLB in front of EC2
- 3 tiers tool with docker-compose (credits https://github.com/wkrzywiec/kanban-board).
- Terraform scripts for all infrastructure

Unzip the 3T app and copy all files on the S3 bucket created with TF after AWS credentials configured (SSO login, AK and ASK or AWS Profile).
This step could be replaced by CI/CD tool like Jenkins or GH Actions.

```sh
aws s3 sync . s3://bucket-name --delete
```

After this steps it is possible to plan and apply the TF scripts.
This steps is verified but not tested by my side.

```sh
terraform init
terraform validate
terraform plan
terraform apply
```

After the apply command, the resource will be initialized.
On the Ec2 instance will be installed docker and docker-compose tool.

With the docker-compose.yml will be created 3 images and 3 container.
The service will be exposed on 4200 port.

The target group in front of Ec2 will checks the 4200 port 
on the instance and then the EC" will be registered on it.

The network load balancer is configured to forward the requests received on 80 port to the target group.