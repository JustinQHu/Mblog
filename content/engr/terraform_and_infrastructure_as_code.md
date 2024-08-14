---
title: "Terraform and Infrastructure as Code"  
date: 2024-08-10T20:36:29-04:00  
draft: false  
categories: ['engineering',]  
tags: ['engineering', 'infrastructure as code', 'IaC', 'terraform']
---

## Infrastructure as Code

### Challenge of Provisioning Cloud Resources 

Manual Provisioning of infrastructure at scale is slow and cumbersome. Provisioning infrastructure through
point-and-click GUIs is error-prone, inefficient, and doesn't scale. The cloud was built for 
automation, but writing custom scripts for every service and every use case is time-consuming
and costly for development teams. 

### Solution

Infrastructure as Code(IaC) to automate infrastructure provisioning on any cloud.  Multi-cloud provisioning
should be automated with declarative infrastructure as code. There should be a simple, easy to learn 
configuration language to allow people to define infrastructure resources such that infrastructure can be codified,
shared, versioned, and executed with a consistent workflow across all environments. 

## Terraform

There are dozens of different tools for infrastructure as code, and
Terraform,  developed by HashiCorp, is one of the most popular IaC tool in the industry now.  

Alternatively,  OpenTofu is an open-source tool forked from terraform 1.5.6 and offers similar
features and interfaces. 

## Terraform Quick Start

### How Terraform Works

Terraform relies on different service providers to actual manage infrastructure resources on different platforms. 
Service provider is actually a module that use upstream platform APIs to provision resources. 

![Terraform Flow](/se/terraform/terraform_flow.png "Terraform Flow")

There are 5 steps to deploy infrastructure with Terraform:
> 1. Scope - Identify the infrastructure for the project 
> 2. Author - Write the configuration for the infrastructure
> 3. Initialize - Install the plugins Terraform needs to manage the infrastructure
> 4. Plan - Preview the changes Terraform will make to match your configuration
> 5. Apply - Make the planned changes


### Install Terraform CLI
Use Homebrew to install Terraform on Mac

```shell
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

Verify the Installation
```shell
terraform -help

Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.
```

### Terraform Provisions a NGINX server via Docker

#### Prerequisite
1. Docker Desktop Installed and Running
2. Terraform Installed

#### Create a director for the project

Create and navigate to a director for the project
```shell
mkdir learn-terraform-docker-container
cd learn-terraform-docker-container
```

#### Create Terraform configuration file
In the working directory,  create a fill named **main.tf** with the following configuration

```shell
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}

```

#### Initialize the Project
```shell
terraform init

Initializing the backend...
Initializing provider plugins...
- Reusing previous version of kreuzwerker/docker from the dependency lock file
- Using previously-installed kreuzwerker/docker v3.0.2

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

This will install the required plugins for the providers in the configuration. 

#### Plan and Apply the config
```shell
terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "tutorial"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck (known after apply)

      + labels (known after apply)

      + ports {
          + external = 8000
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id           = (known after apply)
      + image_id     = (known after apply)
      + keep_locally = false
      + name         = "nginx"
      + repo_digest  = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

```
This will preview what kind of changes terraform will apply. 
If everything looks good, we can proceed to apply it.   It will require an input of 'Yes' to confirm
```shell
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = "tutorial"
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + healthcheck (known after apply)

      + labels (known after apply)

      + ports {
          + external = 8000
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id           = (known after apply)
      + image_id     = (known after apply)
      + keep_locally = false
      + name         = "nginx"
      + repo_digest  = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Still creating... [10s elapsed]
docker_image.nginx: Still creating... [20s elapsed]
docker_image.nginx: Creation complete after 26s [id=sha256:a72860cb95fd59e9c696c66441c64f18e66915fa26b249911e83c3854477ed9anginx]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 3s [id=89fb0fe9d56082357e1059cf4229dee92979154d73402b2758c872ffda0cf104]

```

#### Verify Nginx Container Being Created

```shell
docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS                  NAMES
89fb0fe9d560   a72860cb95fd   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8000->80/tcp   tutorial
```

visit localhost:8000 in the browser:

![Terraform NGINX](/se/terraform/terraform_docker_nginx.png "Terraform Run NGINX")



### Terraform Provisions Azure Cloud Resource 

#### Prerequisite
1. Terraform installed
2. An active Azure subscription (a free subscription would work)
3. Azure CLI installed (brew install for MacOS)

```shell
brew update && brew install azure-cli
```

#### Authenticate using Azure CLI

##### Login
```shell
az login
```
or using this command with non default browser
```shell
az login --use-device-code
```

##### Set the Subscription
```shell
az account set --subscription "your-subscription-id"
```

##### Create a Service Principal
A Service Principal is an application within Azure Active Directory with the authentication tokens Terraform needs to perform actions on your behalf.

```shell
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<SUBSCRIPTION_ID>"

Creating 'Contributor' role assignment under scope '/subscriptions/your-subscription-id'
The output includes credentials that you must protect. Be sure that you do not include these credentials in your code or check the credentials into your source control. For more information, see https://aka.ms/azadsp-cli
{
  "appId": "xxxxxx-xxx-xxxx-xxxx-xxxxxxxxxx",
  "displayName": "azure-cli-2022-xxxx",
  "password": "xxxxxx~xxxxxx~xxxxx",
  "tenant": "xxxxx-xxxx-xxxxx-xxxx-xxxxx"
}
```

##### Set the Environment Variables

```shell
export ARM_CLIENT_ID="<APPID_VALUE>"
export ARM_CLIENT_SECRET="<PASSWORD_VALUE>"
export ARM_SUBSCRIPTION_ID="<SUBSCRIPTION_ID>"
export ARM_TENANT_ID="<TENANT_VALUE>"
```

#### Create a Project Folder
```shell
mkdir learn-terraform-azure
cd learn-terraform-azure
```

#### Create Terraform CConfiguration

Create a file named **main.tf** with the following content

```shell
# Configure the Azure provider
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0.2"
    }
  }

  required_version = ">= 1.1.0"
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "myTFResourceGroup"
  location = "westus2"
}

```

#### Initialize and Apply Terraform Configuration

Initialize
```shell
terraform init
```
Format the config
```shell
terraform fmt
```
Validate the config
```shell
terraform validate
```

Apply the config
```shell
terraform apply
```

Inspect your state
```shell
terraform show

azurerm_resource_group.rg:
resource "azurerm_resource_group" "rg" {
    id       = "/subscriptions/c9ed8610-47a3-4107-a2b2-a322114dfb29/resourceGroups/myTFResourceGroup"
    location = "westus2"
    name     = "myTFResourceGroup"
}
```

```shell
terraform state list

azurerm_resource_group.rg
```

```shell
terraform state

Usage: terraform state <subcommand> [options] [args]

  This command has subcommands for advanced state management.

  These subcommands can be used to slice and dice the Terraform state.
  This is sometimes necessary in advanced cases. For your safety, all
  state management commands that modify the state create a timestamped
  backup of the state prior to making modifications.

  The structure and output of the commands is specifically tailored to work
  well with the common Unix utilities such as grep, awk, etc. We recommend
  using those tools to perform more advanced state tasks.

Subcommands:
    list                List resources in the state
    mv                  Move an item in the state
    pull                Pull current state and output to stdout
    push                Update remote state from a local state file
    replace-provider    Replace provider in the state
    rm                  Remove instances from the state
    show                Show a resource in the state
```


## Terraform Language

Terraform language is an easy to learn and simple to use language to declare infrastructure resources. 
Can read about it in details at:   https://developer.hashicorp.com/terraform/language