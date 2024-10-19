---
title: "GitHub Actions以及它为何比Jenkins更受欢迎并成为默认CI/CD工具"
date: 2024-10-19T17:16:19-04:00
draft: false
categories: ['工程','技术'] 
tags: ['工程','技术', 'Github Actions', 'CI/CD', 'Jenkins', '持续集成'] 
---

> 本站的所有文章都默认为英文，中文版本由Google Translate 翻译。  
> 由于时间限制，并非所有文章都有中文版本。。

## 什么是 Git Actions

根据 [Github](https://docs.github.com/en/actions/writing-workflows/quickstart) 的说法：
>GitHub Actions 是一个持续集成和持续交付 (CI/CD) 平台，可让您自动化构建、测试和部署管道。您可以创建工作流，在将更改推送到存储库时运行测试，或者将合并的拉取请求部署到生产环境中。如果您有 CI/CD 工具使用经验，可能使用过 Genkins，那么您可以将 Github Actions 视为一种类似但更好的工具，可以实现相同的目的。

类似地，GitHub Actions 可用于：
>
>1. 构建和测试
>2. 部署
>3. 发布包
>4. 管理 github（问题、标签）等等

## 一个简单的例子

### 1. 创建 Github Action 工作流 YAML 文件

在 GitHub 上的存储库中，在 .github/workflows 目录中创建一个名为 github-actions-hello-world.yml 的工作流文件。

### 2. 复制并粘贴以下 Hello World YAML 内容

```YAML
name: GitHub Actions Hello World
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."

```

### 将 YAML 文件提交到 Repo

将工作流文件提交到您的分支

### 查看工作流结果
>
>1. 在 GitHub 上，导航到存储库的主页。
>2. 在您的存储库名称下，单击“操作”。
>3. 在左侧边栏中，单击要显示的工作流

![Github Action Workflow](/se/githubactions/github_action_workflows.png "Workflows")

![Github Action Workflow](/se/githubactions/workflow_runs.png "Workflow Runs")

![GitHub Action Workflow](/se/githubactions/workflow_run_logs.png "Run Logs")

## 将 GitHub Actions 与 Jenkins 进行比较

### GitHub Actions 印象
>
>1.易于学习和上手
>2. 与 Github 高度集成且对开发人员友好
>3. SaaS 解决方案和团队可以选择不维护 CI/CD 服务器。
>4. 共享和重用工作流的市场

### Jenkins
>
>1. 开源软件和插件以扩展功能
>2. 团队需要维护 jenkins 和 jenkins 代理的服务器
>3. 不锁定在一个源代码管理平台（Github），并且可以在任何存储库中拥有代码，包括 Github、Gitlab、BitBucket 等。
>4.学习曲线更陡峭
