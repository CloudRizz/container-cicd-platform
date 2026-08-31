# Container CI/CD Platform

A container-based CI/CD pipeline that automatically validates, tests, scans, builds, and publishes a Python application using GitHub Actions and GitHub Container Registry.

The project focuses on repeatable delivery, automated quality checks, container security, and a pull-request-driven development workflow.

## Key Features

- Automated linting and code-quality validation
- Automated test execution on pull requests
- Docker image build validation
- Container image publishing to GitHub Container Registry (GHCR)
- Security scanning integrated into the CI/CD workflow
- Pull-request validation before changes are merged
- Automated workflows implemented with GitHub Actions

## Project Purpose

The purpose of this repository is to demonstrate practical CI/CD implementation using GitHub Actions.

This project shows how automated pipelines can be used to check code quality, run tests, build Docker images, validate pull requests, and trigger deployment-style workflows when changes are made.

## Application Overview

The application is intentionally simple so the focus remains on the CI/CD pipeline design and automation.

The Python application provides a small codebase that supports testing, linting, Dockerisation, and deployment workflow demonstrations.

## Workflows Included

This repository includes several GitHub Actions workflows, each designed to demonstrate a specific CI/CD capability.

### CI Workflow

The CI workflow runs automated checks against the application.

It confirms that the application behaves as expected whenever changes are pushed or submitted through a pull request.

### Lint Workflow

The lint workflow checks the Python code using `flake8`.

This helps enforce code quality standards and catch formatting issues before changes are merged into the main branch.

Example lint command:

```bash
flake8 app
````

### Docker Build Workflow

The Docker build workflow checks that the application can be successfully built into a Docker image.

This validates the Dockerfile and confirms that the application can be containerised consistently.

Example Docker build command:

```bash
docker build -t cicd-demo-app .
```

### Manual Workflow

The manual workflow demonstrates controlled workflow execution using `workflow_dispatch`.

This allows selected workflows to be triggered manually from the GitHub Actions tab when an on-demand pipeline run is required.

### Deploy Workflow

The deploy workflow demonstrates a simple continuous delivery process.

It builds a Docker image and pushes it to GitHub Container Registry.

This creates a deployable container image automatically when changes are pushed to the main branch.

### Security Checks Workflow

The security checks workflow adds an additional security layer to the CI/CD process.

It runs automatically on pushes and pull requests into the main branch.

This workflow checks for hardcoded secrets and reviews GitHub Actions workflow files for common security issues.

The purpose of this workflow is to reduce the risk of committing sensitive information such as API keys, tokens, passwords, or private keys into the repository.

It also helps enforce safer GitHub Actions practices by checking workflow configuration for potential security weaknesses.

## CI/CD Concepts Demonstrated

This project demonstrates the following CI/CD concepts:

| Concept                | Description                                                     |
| ---------------------- | --------------------------------------------------------------- |
| Continuous Integration | Automatically validating code changes through pipeline checks   |
| Pull Request Triggers  | Running workflows when a pull request is opened or updated      |
| Linting                | Enforcing Python code quality using `flake8`                    |
| Unit Testing           | Running automated tests to validate application behaviour       |
| Docker Build Checks    | Confirming the application can be built as a Docker image       |
| Continuous Delivery    | Building and publishing a deployable Docker image automatically |
| Manual Triggers        | Running workflows manually using GitHub Actions                 |

## GitHub Actions Triggers

The workflows use common GitHub Actions triggers such as:

```yaml
on:
  push:
  pull_request:
```

Some workflows also use:

```yaml
on:
  workflow_dispatch:
```

These triggers allow workflows to run automatically or manually depending on their purpose.

## Docker

The project includes a Dockerfile that packages the Python application into a container image.

The Docker build workflow validates that the image can be created successfully.

The deploy workflow builds and publishes the image to GitHub Container Registry as part of the CD process.

## How to Run Locally

Run the Python application:

```bash
python app/hello.py
```

Run tests:

```bash
python -m unittest discover -s app
```

Run linting:

```bash
flake8 app
```

Build the Docker image locally:

```bash
docker build -t cicd-demo-app .
```

Run the Docker container:

```bash
docker run cicd-demo-app
```

## Skills Demonstrated

This project demonstrates practical experience with:

* Creating and managing GitHub Actions workflow files
* Configuring workflow triggers for pushes and pull requests
* Using jobs and steps to structure pipeline automation
* Running automated commands on GitHub-hosted runners
* Enforcing Python code quality with `flake8`
* Running automated tests as part of a CI process
* Building Docker images through automated pipelines
* Validating pull requests before merge
* Creating a basic continuous delivery workflow
* Publishing Docker images to GitHub Container Registry
* Structuring a repository to support CI/CD automation

## Summary

This repository is a practical CI/CD demonstration project.

It shows how GitHub Actions can be used to automate testing, linting, Docker image builds, pull request validation, and deployment-style workflows.

The aim of this project is to demonstrate hands-on DevOps skills using a Python application, Docker, and GitHub Actions.

