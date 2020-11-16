# Getting Started with Terraform

Terraform is the most popular language for defining and provisioning Infrastructure as Code (IaC).

Visit [Terraform.io](https://www.terraform.io/downloads.html) to download the application executable file for your platform, machine or environment on which you like to run code and do development.

After Terraform is installed you can define and create some infrastructure.

Most users find it easiest to create a new directory on their local machine and create Terraform configuration code inside it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file with a `.tf` extension for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file:

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

Initialize Terraform with the `init` command. The AWS provider will automatically be installed. 

```shell
$ terraform init
```

Check for any errors in the output. If `init` ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The `apply`command will take up to a few minutes to run and upon success will display a message indicating that the resource was created.

Finally, destroy the infrastructure:

```shell
$ terraform destroy
```

Look for a message at the bottom of the output asking for confirmation before destroying the associated resources. Type `yes` and hit ENTER and Terraform will destroy the resources it created earlier.

Congratulations! You just took your first steps declaring IaC with Terraform.

