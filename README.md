# Getting Started with Terraform

This document explains how to install and configure Terraform on Linux.

## Prerequisites

- An application that can extract zip files.
- A text editor application.
- Docker

## Download and Install Terraform

Download Terraform by visiting [Terraform.io] and selecting the binary file for your operating system.

Extract the Terraform zip file.

Move the terraform binary to the bin directory.

```shell
$ mv terraform /user/local/bin
```

## Create a Terraform Configuration File

Create a directory for your configuration files.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Copy and paste the following lines into the file using your preferred text editor.

```hcl
provider "docker" {
    host = "unix:///var/run/docker.sock"
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Initialize Terraform with the `init` command. The AWS provider will be installed.

```shell
$ terraform init
```

### Example Output

> Initializing the backend...
>
> Initializing provider plugins...
> \- Using previously-installed terraform-providers/docker v2.7.2
>
> The following providers do not have any version constraints in configuration, so the latest version was installed.
>
> To prevent automatic upgrades to new major versions that may contain breaking changes, we recommend adding version constraints in a required_providers block in your configuration, with the constraint strings suggested below.
>
> \* terraform-providers/docker: version = "~> 2.7.2"
>
> Terraform has been successfully initialized!
>
> You may now begin working with Terraform. Try running "terraform plan" to see any changes that are required for your infrastructure. All Terraform commands should now work.
>
> If you ever set or change modules or backend configuration for Terraform, rerun this command to reinitialize your working directory. If you forget, other commands will detect it and remind you to do so if necessary.
>
>You should check for any errors and correct them before proceeding.

## Resource Provisioning

Provision the resources with the `apply` command.

```shell
$ terraform apply
```

### Example Output

> An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
>
> \+ create
>
>Terraform will perform the following actions:
>
> ```hcl
> # docker_container.nginx will be created
> + resource "docker_container" "nginx" {
>      + attach           = false
>      + bridge           = (known after apply)
>      + command          = (known after apply)
>      + container_logs   = (known after apply)
>      + dns              = (known after apply)
>      + dns_opts         = (known after apply)
>      + entrypoint       = (known after apply)
>      + exit_code        = (known after apply)
>      + gateway          = (known after apply)
>      + hostname         = (known after apply)
>      + id               = (known after apply)
>      + image            = (known after apply)
>      + ip_address       = (known after apply)
>      + ip_prefix_length = (known after apply)
>      + ipc_mode         = (known after apply)
>      + log_driver       = (known after apply)
>      + log_opts         = (known after apply)
>      + logs             = false
>      + must_run         = true
>      + name             = "training"
>      + network_data     = (known after apply)
>      + read_only        = false
>      + restart          = "no"
>      + rm               = false
>      + shm_size         = (known after apply)
>      + start            = true
>      + user             = (known after apply)
>      + working_dir      = (known after apply)
>
>      + ports {
>          + external = 80
>          + internal = 80
>          + ip       = "0.0.0.0"
>          + protocol = "tcp"
>        }
>    }
>
> # docker_image.nginx will be created  
> + resource "docker_image" "nginx" {
>      + id     = (known after apply)
>      + latest = (known after apply)
>      + name   = "nginx:latest"
>    }
> ```
>
> Plan: 2 to add, 0 to change, 0 to destroy.
>
> Do you want to perform these actions?  
> Terraform will perform the actions described above.  
> Only 'yes' will be accepted to approve.
>
> Enter a value:
>
> Terraform will prompt you for confirmation before proceeding.  
> Type `yes` and press ENTER.

The `apply` command will take a few minutes to run. If it is successful, it will display a message that the resource was created.

### Example Output

> docker_image.nginx: Creating...  
> docker_image.nginx: Creation complete after 7s [id=sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest]  
> docker_container.nginx: Creating...  
> docker_container.nginx: Creation complete after 0s [id=98ca69995a6c50c257d7ed7804ba0101adb9ef37b2e5b233804c04614f2c5c5c]
>
> Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

## Destroy the Infrastructure

Remove the infrastructure with the `destroy` command.

```shell
$ terraform destroy
```
### Example Output

> An execution plan has been generated and is shown below.  
> Resource actions are indicated with the following symbols:  
> \- destroy
>
> Terraform will perform the following actions:
>
> ```hcl
> # docker_container.nginx will be destroyed
> - resource "docker_container" "nginx" {
>      - attach            = false -> null
>      - command           = [
>          - "nginx",
>          - "-g",
>          - "daemon off;",
>        ] -> null
>      - cpu_shares        = 0 -> null
>      - dns               = [] -> null
>      - dns_opts          = [] -> null
>      - dns_search        = [] -> null
>      - entrypoint        = [
>          - "/docker-entrypoint.sh",
>        ] -> null
>      - gateway           = "172.17.0.1" -> null
>      - group_add         = [] -> null
>      - hostname          = "98ca69995a6c" -> null
>      - id                = "98ca69995a6c50c257d7ed7804ba0101adb9ef37b2e5b233804c04614f2c5c5c" -> null
>      - image             = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35" -> null
>      - ip_address        = "172.17.0.2" -> null
>      - ip_prefix_length  = 16 -> null
>      - ipc_mode          = "private" -> null
>      - links             = [] -> null
>      - log_driver        = "json-file" -> null
>      - log_opts          = {} -> null
>      - logs              = false -> null
>      - max_retry_count   = 0 -> null
>      - memory            = 0 -> null
>      - memory_swap       = 0 -> null
>      - must_run          = true -> null
>      - name              = "training" -> null
>      - network_data      = [
>          - {
>              - gateway          = "172.17.0.1"
>              - ip_address       = "172.17.0.2"
>              - ip_prefix_length = 16
>              - network_name     = "bridge"
>            },
>        ] -> null
>      - network_mode      = "default" -> null
>      - privileged        = false -> null
>      - publish_all_ports = false -> null
>      - read_only         = false -> null
>      - restart           = "no" -> null
>      - rm                = false -> null
>      - shm_size          = 64 -> null
>      - start             = true -> null
>      - sysctls           = {} -> null
>      - tmpfs             = {} -> null
>
>      - ports {
>          - external = 80 -> null
>          - internal = 80 -> null
>          - ip       = "0.0.0.0" -> null
>          - protocol = "tcp" -> null
>        }
>    }
>
>  # docker_image.nginx will be destroyed
>  - resource "docker_image" "nginx" {
>      - id     = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest" -> null
>      - latest = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35" -> null
>      - name   = "nginx:latest" -> null
>    }
> ```
>
> Plan: 0 to add, 0 to change, 2 to destroy.
>
> Do you really want to destroy all resources?
> Terraform will destroy all your managed infrastructure, as shown above.
> There is no undo. Only 'yes' will be accepted to confirm.
>
> Enter a value:

Terraform will prompt you for confirmation before proceeding. Type `yes` and press ENTER. Terraform will destroy the resources it created earlier.

### Example Output

> docker_container.nginx: Destroying... [id=98ca69995a6c50c257d7ed7804ba0101adb9ef37b2e5b233804c04614f2c5c5c]  
> docker_container.nginx: Destruction complete after 2s  
> docker_image.nginx: Destroying... [id=sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest]  
> docker_image.nginx: Destruction complete after 0s
>
>Destroy complete! Resources: 2 destroyed.

## Next steps

In this tutorial, you learned how to create and destroy cloud services using Terraform. Save time and money with infrastructure as code. Visit our [Build Infrastructure] tutorial to learn more.

[Terraform.io]: https://www.terraform.io/downloads.html

[Build Infrastructure]: https://learn.hashicorp.com/tutorials/terraform/aws-build?in=terraform/aws-get-started
