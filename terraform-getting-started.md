# Getting Started with Terraform

Terraform is the most popular language for defining and provisioning Infrastructure as Code (IaC).

In this guide you will learn how to get started writing and deploying Terraform configurations.

## Prerequisites

## Install Terraform

Visit [Terraform.io](https://www.terraform.io/downloads.html) to download the application executable file for your platform, machine or environment on which you like to run code and do development.

After Terraform is installed you can define and create some infrastructure.

## Define the Terraform Module

Most users find it easiest to create a new directory on their local machine and create Terraform configuration code inside it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

This directory is called a "module", and it contains your Terraform configuration files.

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

In the above example, the docker "provider" is declared to enable the docker_container and docker_image resource types. The docker_container resource accepts arguments like "image", "name" and "port" that declare the resource's desired state. Check out the [Terraform configuration language docs](https://www.terraform.io/docs/configuration/index.html) to learn more about resources and providers.

## Deploy the Configuration

Initialize Terraform with the `init` command. The AWS provider will automatically be installed. 

```shell
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "docker" (terraform-providers/docker) 2.7.2...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.docker: version = "~> 2.7"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

Check for any errors in the output. A common mistake is not starting Docker first, which produces the "Is the docker daemon running?" error. 

Once `init` runs successfully, provision the resource with the `apply` command. Enter `yes` when asked if you would like to perform these actions.

```shell
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + dns              = (known after apply)
      + dns_opts         = (known after apply)
      + entrypoint       = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = (known after apply)
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + log_opts         = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + restart          = "no"
      + rm               = false
      + shm_size         = (known after apply)
      + start            = true
      + user             = (known after apply)
      + working_dir      = (known after apply)

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id     = (known after apply)
      + latest = (known after apply)
      + name   = "nginx:latest"
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 0s [id=sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=a8d2c9a107dde9cd927cd1ed3a02d063bcd7b858d9c462405edb424e546c64f1]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

The `apply`command will take up to a few minutes to run and upon success will display a message indicating that the resource was created.

## Cleaning Up

Finally, destroy the infrastructure:

```shell
$ terraform destroy

docker_image.nginx: Refreshing state... [id=sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest]
docker_container.nginx: Refreshing state... [id=a8d2c9a107dde9cd927cd1ed3a02d063bcd7b858d9c462405edb424e546c64f1]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "a8d2c9a107dd" -> null
      - id                = "a8d2c9a107dde9cd927cd1ed3a02d063bcd7b858d9c462405edb424e546c64f1" -> null
      - image             = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35" -> null
      - ip_address        = "172.17.0.2" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway          = "172.17.0.1"
              - ip_address       = "172.17.0.2"
              - ip_prefix_length = 16
              - network_name     = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id     = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest" -> null
      - latest = "sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35" -> null
      - name   = "nginx:latest" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=a8d2c9a107dde9cd927cd1ed3a02d063bcd7b858d9c462405edb424e546c64f1]
docker_container.nginx: Destruction complete after 0s
docker_image.nginx: Destroying... [id=sha256:f35646e83998b844c3f067e5a2cff84cdf0967627031aeda3042d78996b68d35nginx:latest]
```

Look for a message at the bottom of the output asking for confirmation before destroying the associated resources. Type `yes` and hit ENTER and Terraform will destroy the resources it created earlier.

Congratulations! You just took your first steps declaring and provisioning IaC with Terraform.

## Next Steps