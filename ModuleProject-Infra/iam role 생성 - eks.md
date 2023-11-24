```
terraform apply                                                                                               1 ✘  1m 26s   10:07:03 AM 
Acquiring state lock. This may take a few moments...
module.bastion-host.tls_private_key.private-key: Refreshing state... [id=74aa5071913076d0d333203529d88770867f920a]
module.bastion-host.local_file.private_key: Refreshing state... [id=7981406f21bbacfe23a6876c938615eee27be253]
module.main_vpc.aws_vpc.main: Refreshing state... [id=vpc-01523766cc8f4c70f]
module.bastion-host.aws_key_pair.bastion-host-key-pair: Refreshing state... [id=bastion-host-key-pair]
module.role.aws_iam_role.eks-role: Refreshing state... [id=eks-role]
aws_dynamodb_table.terraform-state-lock: Refreshing state... [id=hanggi-terraform-state-lock]
aws_s3_bucket.terraform-backend: Refreshing state... [id=hanggi-terraform-backend]
module.main_ecr.aws_ecr_repository.main: Refreshing state... [id=hanggi-ecr]
module.role.aws_iam_role_policy_attachment.eks_cluster_policy_attachment: Refreshing state... [id=eks-role-20231123010558006200000002]
module.role.aws_iam_role_policy_attachment.eks_cni_policy_attachment: Refreshing state... [id=eks-role-20231123010557755900000001]
module.main_vpc.aws_internet_gateway.vpc_igw: Refreshing state... [id=igw-08ccfdde7a1c967f0]
module.main_vpc.module.subnet_private_1_a.aws_subnet.subnet: Refreshing state... [id=subnet-0464f14a44f0cd32f]
module.main_vpc.module.subnet_private_2_b.aws_subnet.subnet: Refreshing state... [id=subnet-0b3b0aac3e9ab4209]
module.main_vpc.module.subnet_private_1_b.aws_subnet.subnet: Refreshing state... [id=subnet-0f25d24a652f6973b]
module.main_vpc.module.subnet_public_1_a.aws_subnet.subnet: Refreshing state... [id=subnet-09cb852d7c074cfda]
module.main_vpc.module.subnet_private_2_a.aws_subnet.subnet: Refreshing state... [id=subnet-0bcd4c32515e2ce25]
module.sg.aws_security_group.bastion: Refreshing state... [id=sg-09ec5841b6ad4713a]
module.sg.aws_security_group.rds: Refreshing state... [id=sg-0293fe13e495f021f]
module.main_vpc.aws_route_table.public_route_table: Refreshing state... [id=rtb-009228e75007e26ed]
module.bastion-host.aws_instance.bastion-host: Refreshing state... [id=i-0972c35a2247b4546]
module.rds.aws_db_subnet_group.rds-subnet-group: Refreshing state... [id=rds-subnet-group]
module.main_vpc.aws_route_table_association.public_subnet_association: Refreshing state... [id=rtbassoc-0586324ccc7ce70cb]
aws_s3_bucket_public_access_block.terraform-backend-public-access: Refreshing state... [id=hanggi-terraform-backend]
aws_s3_bucket_versioning.terraform-backend-versioning: Refreshing state... [id=hanggi-terraform-backend]
aws_s3_bucket_server_side_encryption_configuration.terraform-backend-encryption: Refreshing state... [id=hanggi-terraform-backend]
module.rds.aws_db_instance.hanggi-rds: Refreshing state... [id=db-STO3DHPSQNXI6K43XMNNXACVAI]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
-/+ destroy and then create replacement

Terraform will perform the following actions:

  # module.bastion-host.aws_instance.bastion-host must be replaced
-/+ resource "aws_instance" "bastion-host" {
      ~ arn                                  = "arn:aws:ec2:us-west-2:434692986520:instance/i-0972c35a2247b4546" -> (known after apply)
      ~ associate_public_ip_address          = true -> (known after apply)
      ~ availability_zone                    = "us-west-2a" -> (known after apply)
      ~ cpu_core_count                       = 1 -> (known after apply)
      ~ cpu_threads_per_core                 = 1 -> (known after apply)
      ~ disable_api_stop                     = false -> (known after apply)
      ~ disable_api_termination              = false -> (known after apply)
      ~ ebs_optimized                        = false -> (known after apply)
      - hibernation                          = false -> null
      + host_id                              = (known after apply)
      + host_resource_group_arn              = (known after apply)
      + iam_instance_profile                 = (known after apply)
      ~ id                                   = "i-0972c35a2247b4546" -> (known after apply)
      ~ instance_initiated_shutdown_behavior = "stop" -> (known after apply)
      + instance_lifecycle                   = (known after apply)
      ~ instance_state                       = "running" -> (known after apply)
      ~ ipv6_address_count                   = 0 -> (known after apply)
      ~ ipv6_addresses                       = [] -> (known after apply)
      ~ monitoring                           = false -> (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      ~ placement_partition_number           = 0 -> (known after apply)
      ~ primary_network_interface_id         = "eni-0970755a4d4beb870" -> (known after apply)
      ~ private_dns                          = "ip-10-0-5-60.us-west-2.compute.internal" -> (known after apply)
      ~ private_ip                           = "10.0.5.60" -> (known after apply)
      ~ public_dns                           = "ec2-54-191-190-189.us-west-2.compute.amazonaws.com" -> (known after apply)
      ~ public_ip                            = "54.191.190.189" -> (known after apply)
      ~ secondary_private_ips                = [] -> (known after apply)
      ~ security_groups                      = [ # forces replacement
          + "sg-09ec5841b6ad4713a",
        ]
      + spot_instance_request_id             = (known after apply)
      - tags                                 = {} -> null
      ~ tags_all                             = {} -> (known after apply)
      ~ tenancy                              = "default" -> (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      ~ vpc_security_group_ids               = [
          - "sg-09ec5841b6ad4713a",
        ] -> (known after apply)
        # (7 unchanged attributes hidden)

      - capacity_reservation_specification {
          - capacity_reservation_preference = "open" -> null
        }

      - cpu_options {
          - core_count       = 1 -> null
          - threads_per_core = 1 -> null
        }

      - credit_specification {
          - cpu_credits = "standard" -> null
        }

      - enclave_options {
          - enabled = false -> null
        }

      - maintenance_options {
          - auto_recovery = "default" -> null
        }

      - metadata_options {
          - http_endpoint               = "enabled" -> null
          - http_protocol_ipv6          = "disabled" -> null
          - http_put_response_hop_limit = 2 -> null
          - http_tokens                 = "required" -> null
          - instance_metadata_tags      = "disabled" -> null
        }

      - private_dns_name_options {
          - enable_resource_name_dns_a_record    = false -> null
          - enable_resource_name_dns_aaaa_record = false -> null
          - hostname_type                        = "ip-name" -> null
        }

      - root_block_device {
          - delete_on_termination = true -> null
          - device_name           = "/dev/xvda" -> null
          - encrypted             = false -> null
          - iops                  = 3000 -> null
          - tags                  = {} -> null
          - throughput            = 125 -> null
          - volume_id             = "vol-0c98db48e4865d5b5" -> null
          - volume_size           = 8 -> null
          - volume_type           = "gp3" -> null
        }
    }

Plan: 1 to add, 0 to change, 1 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

module.bastion-host.aws_instance.bastion-host: Destroying... [id=i-0972c35a2247b4546]
module.bastion-host.aws_instance.bastion-host: Still destroying... [id=i-0972c35a2247b4546, 10s elapsed]
module.bastion-host.aws_instance.bastion-host: Still destroying... [id=i-0972c35a2247b4546, 20s elapsed]
module.bastion-host.aws_instance.bastion-host: Still destroying... [id=i-0972c35a2247b4546, 30s elapsed]
module.bastion-host.aws_instance.bastion-host: Destruction complete after 32s
module.bastion-host.aws_instance.bastion-host: Creating...
module.bastion-host.aws_instance.bastion-host: Still creating... [10s elapsed]
module.bastion-host.aws_instance.bastion-host: Still creating... [20s elapsed]
module.bastion-host.aws_instance.bastion-host: Still creating... [30s elapsed]
module.bastion-host.aws_instance.bastion-host: Creation complete after 33s [id=i-04c4754a72566aa7d]

Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

Outputs:

dynamodb-name = "hanggi-terraform-state-lock"
terraform-backend-arn = "arn:aws:s3:::hanggi-terraform-backend"
```