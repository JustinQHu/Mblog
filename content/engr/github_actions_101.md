---
title: "GitHub Actions and Why It's Gaining Popularity over Jenkins as Default CI/CD Tool "
date: 2024-10-19T15:50:06-04:00
draft: false
categories: ['engineering','2024']
tags: ['engineering', 'CI/CD', 'GitHub Actions','Jenkins']
---

> The Current CI/CD tooling for our company is moving away from Jenkins and towards GitHub Actions. Learn some basics about GitHub Actions as a reulst.

## What's Git Actions

According to [Github](https://docs.github.com/en/actions/writing-workflows/quickstart):
>GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that run tests whenever you push a change to your repository, or that deploy merged pull requests to production.

If you have experience with CI/CD tooling, potentially with Genkins, you can view Github Actions as a similar but better tool to achive the same purpose.

Similarly,  GitHub Actions can be used for:
>
> 1. build and test
> 2. deployment
> 3. publish packges
> 4. manage github(issues, lablels)
> 5. and so so

## A Quick Example

### 1. Create a Github Action Workflow YAML File

In your repository on GitHub, create a workflow file called ***github-actions-hello-world.yml*** in the .github/workflows directory.

### 2. Copy and Paste the Following Hello World YAML Content

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

### Commit the YAML File to the Repo

Committing the workflow file to your branch

### View the Workflow Reulst

>
> 1. On GitHub, navigate to the main page of the repository.
> 2. Under your repository name, click  Actions.
> 3. In the left sidebar, click the workflow you want to display

![Github Action Workflow](/se/githubactions/github_action_workflows.png "Workflows")

![Github Action Workflow](/se/githubactions/workflow_runs.png "Workflow Runs")

![GitHub Action Workflow](/se/githubactions/workflow_run_logs.png "Run Logs")

## Compare GitHub Actions with Jenkins

### GitHub Actions Impressions
>
> 1. easy to learn and get started
> 2. highly integrated with Github and developer friendly
> 3. SaaS solution and team can choose to not maintain servers for CI/CD.
> 4. Market place for sharing and reusing workflows

### Jenkins
>
> 1. open source software and plug-ins to extend functionality
> 2. team need to maintain the servers for jenkins and jenkins agents
> 3. not locked to one source code management platform(Github) and can have code on any repository, including Github, Gitlab, BitBucket, and others.
> 4. learning curve is more steep
