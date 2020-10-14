# Getting Started with Terraform

This document explains how to install and configure Terraform.

## Prerequisites

- An application that can extract zip files.
- A text editor application.

## Download and Install Terraform

Download Terraform by visiting [Terraform.io] and selecting the binary file for your operating system.

Extract the Terraform zip file into a new directory on your machine. For this tutorial, we recommend putting your Terraform configuration code inside this directory.

## Create Terraform Configuration Files

The following commands are written for the Linux shell.

Open a shell prompt and navigate to the Terraform directory you created in the previous step.

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

You should check for any errors and correct them before proceeding. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The apply command will take a few minutes to run. If it is successful, it will display a message that the resource was created.

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

Terraform will prompt you for confirmation. Type `yes` and press ENTER. Terraform will destroy the resources it created earlier.

## Next steps

In this tutorial, you learned how to create and destroy cloud services using Terraform. Save time and money with infrastructure as code. Visit our [Build Infrastructure] tutorial to learn more.

[Terraform.io]: https://www.terraform.io/downloads.html

[Build Infrastructure]: https://learn.hashicorp.com/tutorials/terraform/aws-build?in=terraform/aws-get-started
