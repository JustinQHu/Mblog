---
title: "Terraform 和基础设施即代码"
date: 2024-10-19T17:17:04-04:00
draft: false
categories: ['工程','技术', '2024'] 
tags: ['工程','技术', 'IaC', 'Terraform', '基础设施即代码'] 
---

## 基础设施即代码

### 配置云资源的挑战

大规模手动配置基础设施既缓慢又繁琐。通过点击式 GUI 配置基础设施容易出错、效率低下且无法扩展。云是为自动化而构建的，但为每项服务和每个用例编写自定义脚本对于开发团队来说既耗时又昂贵。

### 解决方案

基础设施即代码 (IaC) 可在任何云上自动配置基础设施。多云配置应使用声明式基础设施即代码实现自动化。应该有一种简单易学的配置语言，允许人们定义基础设施资源，以便可以在所有环境中以一致的工作流对基础设施进行编码、共享、版本控制和执行。

## Terraform

基础设施即代码有几十种不同的工具，HashiCorp 开发的 Terraform 是目前业内最受欢迎的 IaC 工具之一。

另外，OpenTofu 是从 terraform 1.5.6 分叉的开源工具，提供类似的功能和界面。

## Terraform 快速入门

### Terraform 的工作原理

Terraform 依赖不同的服务提供商来实际管理不同平台上的基础设施资源。服务提供商实际上是一个使用上游平台 API 来配置资源的模块。

![Terraform Flow](/engr/terraform/terraform_flow.png "Terraform Flow")
使用 Terraform 部署基础设施有 5 个步骤：

>
>1.范围 - 确定项目的基础设施  
>2.作者 - 编写基础设施的配置  
>3.初始化 - 安装 Terraform 管理基础设施所需的插件  
>4.计划 - 预览 Terraform 将进行的更改以匹配您的配置  
>5.应用 - 进行计划的更改

### 安装 Terraform CLI

使用 Homebrew 在 Mac 上安装 Terraform

```shell
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

验证安装

```shell
terraform -help

Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.
```

### Terraform 通过 Docker 配置 NGINX 服务器

#### 先决条件

1.Docker Desktop 已安装并正在运行  
2.Terraform 已安装

#### 为项目创建目录

为项目创建目录并切换到该目录

```shell
mkdir learn-terraform-docker-container
cd learn-terraform-docker-container
```

#### 创建 Terraform 配置文件

在工作目录中，使用以下配置创建名为 ***main.tf*** 的配置文件

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


#### 初始化项目

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

#### 规划和应用配置

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

这将预览 terraform 将应用哪种更改。如果一切看起来不错，我们可以继续应用它。需要输入“是”以确认

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

#### 验证正在创建的 Nginx 容器

```shell
docker ps

CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS                  NAMES
89fb0fe9d560   a72860cb95fd   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8000->80/tcp   tutorial
```

在浏览器中访问 localhost:8000:

![Terraform NGINX](/se/terraform/terraform_docker_nginx.png "Terraform Run NGINX")

### Terraform 提供 Azure 云资源

#### 先决条件

1.Terraform 安装  
2.有效的 Azure 订阅（免费订阅即可）  
3.Azure CLI 安装（适用于 MacOS 的 brew install）  

```shell
brew update && brew install azure-cli
```

#### 使用 Azure CLI 进行身份验证

##### 登录

```shell
az login
```

或使用此命令与非默认浏览器

```shell
az login --use-device-code
```

##### 设置订阅

```shell
az account set --subscription "your-subscription-id"
```

##### 创建服务主体

服务主体是 Azure Active Directory 中的应用程序，具有 Terraform 代表您执行操作所需的身份验证令牌。

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

##### 设置环境变量

```shell
export ARM_CLIENT_ID="<APPID_VALUE>"
export ARM_CLIENT_SECRET="<PASSWORD_VALUE>"
export ARM_SUBSCRIPTION_ID="<SUBSCRIPTION_ID>"
export ARM_TENANT_ID="<TENANT_VALUE>"
```

##### 创建项目文件夹

```shell
mkdir learn-terraform-azure
cd learn-terraform-azure
```

###### 创建 Terraform 配置

创建一个名为 ***main.tf*** 的文件，内容如下

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

##### 初始化并应用 Terraform 配置

初始化

```shell
terraform init
```

格式化配置

```shell
terraform fmt
```

验证配置

```shell
terraform validate
```

应用配置

```shell
terraform apply
```

检查您的状态

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

## Terraform 语言

Terraform 语言是一种易于学习且易于使用的语言，用于声明基础设施资源。详细信息可参见：<https://developer.hashicorp.com/terraform/language>
