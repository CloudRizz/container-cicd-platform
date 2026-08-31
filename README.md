# Container CI/CD Platform

A container-based CI/CD platform that automates code validation, unit testing, security scanning, Docker image validation, and container publishing using GitHub Actions and GitHub Container Registry (GHCR).

The project implements automated quality and security controls across the software delivery lifecycle, providing repeatable builds and early feedback when changes are introduced.

## Architecture

```text
Developer
    │
    ▼
Git Push / Pull Request
    │
    ├── Python Linting (Flake8)
    ├── Unit Testing
    ├── Docker Build Validation
    └── Security Checks
         ├── Gitleaks
         └── Zizmor
    │
    ▼
Docker Image Build
    │
    ▼
GitHub Container Registry
```

## Key Features

* Automated CI/CD workflows using GitHub Actions
* Python unit testing using the built-in `unittest` framework
* Automated Python code-quality validation with Flake8
* Docker image build validation
* Automated Docker image publishing to GitHub Container Registry
* Hardcoded secret detection using Gitleaks
* GitHub Actions security auditing using Zizmor
* Pull-request validation for linting, container builds, and security checks
* Pinned GitHub Actions dependencies for improved supply-chain security
* Manually triggered testing across selectable Python versions

## Technology Stack

| Technology                | Purpose                                    |
| ------------------------- | ------------------------------------------ |
| GitHub Actions            | CI/CD workflow automation                  |
| Docker                    | Application containerisation               |
| GitHub Container Registry | Container image storage                    |
| Python                    | Application and automated testing          |
| unittest                  | Python unit testing                        |
| Flake8                    | Python linting and code-quality validation |
| Gitleaks                  | Hardcoded secret detection                 |
| Zizmor                    | GitHub Actions security analysis           |
| Git                       | Source control                             |

## CI Pipeline

Multiple GitHub Actions workflows provide independent validation controls across the repository.

### Unit Testing

Application unit tests are executed automatically using Python's built-in `unittest` framework.

```bash
cd app
python -m unittest discover
```

Dependencies are installed from the application's `requirements.txt` before the tests execute.

This provides automated verification that application behaviour remains valid as changes are introduced.

### Code Quality

Flake8 is used to perform automated Python linting.

The linting workflow executes on pushes and pull requests:

```bash
flake8 app
```

This provides consistent automated code-quality validation before changes are integrated.

### Docker Build Validation

Docker builds are independently validated on both pushes and pull requests.

```bash
docker build -t cicd-demo-app .
```

This verifies that application changes do not break the container build process.

## Security Controls

Security validation is integrated directly into the GitHub Actions workflow.

### Secret Scanning

Gitleaks scans the repository for accidentally committed credentials and other sensitive information.

The scan is designed to identify potential exposure of:

* AWS access keys
* GitHub tokens
* API keys
* Passwords
* Private keys
* Database credentials

### GitHub Actions Security

Zizmor is used to analyse GitHub Actions workflow configuration for potential security weaknesses.

```bash
zizmor .github/workflows/security-checks.yaml
```

The security workflow runs against changes targeting the `main` branch.

### Supply-Chain Security

Security-sensitive GitHub Actions are pinned to specific commit SHAs rather than relying solely on mutable version tags.

For example:

```yaml
uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5
```

Pinning actions reduces the risk associated with unexpected changes to third-party action versions.

Workflow permissions are also explicitly restricted where possible:

```yaml
permissions:
  contents: read
```

Container publishing receives the additional permission required to publish packages:

```yaml
permissions:
  contents: read
  packages: write
```

## Container Build and Publishing

The delivery workflow automatically builds the application as a Docker image.

GitHub Actions authenticates with GitHub Container Registry using the repository-provided `GITHUB_TOKEN`, avoiding the need to store a separate registry password.

```text
Source Code
    │
    ▼
GitHub Actions
    │
    ▼
Docker Build
    │
    ▼
Authenticate to GHCR
    │
    ▼
Push Container Image
    │
    ▼
GitHub Container Registry
```

The image is currently published using the `latest` tag.

## Manual Testing Workflow

The repository also provides a manually triggered workflow using `workflow_dispatch`.

This allows testing to be executed against a selected Python version.

Available versions currently include:

```text
Python 3.8
Python 3.9
Python 3.10
```

This provides a simple mechanism for manually validating application behaviour across different Python runtimes.

## Repository Structure

```text
.
├── .github/
│   └── workflows/
│       ├── ci.yaml
│       ├── docker-build.yaml
│       ├── lint.yaml
│       ├── manual-workflow.yaml
│       ├── security-checks.yaml
│       └── deploy.yaml
│
├── app/
│   ├── requirements.txt
│   └── ...
│
├── Dockerfile
└── README.md
```

## Engineering Decisions

### Separate CI Workflows

Testing, linting, container validation, security checks, and container publishing are separated into individual GitHub Actions workflows.

This keeps each automation responsibility independently visible and makes failures easier to identify from the Actions interface.

### Automated Validation

Automated checks reduce reliance on manual validation and provide consistent feedback whenever changes are introduced.

Testing, linting, container builds, and security analysis each validate a different aspect of the application delivery process.

### Containerisation

Docker provides a repeatable application runtime and allows the same application artifact to be distributed independently of the environment in which it was built.

CI-based Docker build validation also detects containerisation problems before an image is consumed downstream.

### GitHub Container Registry

GHCR is used as the container registry because it integrates directly with GitHub Actions and repository permissions.

The workflow authenticates using GitHub's automatically provided `GITHUB_TOKEN` rather than storing a separate long-lived registry credential.

### Security as Part of CI

Security validation is automated rather than treated solely as a manual review step.

Gitleaks provides secret detection while Zizmor evaluates GitHub Actions configuration, providing security controls across both source control and pipeline configuration.

## Running Locally

Clone the repository:

```bash
git clone https://github.com/CloudRizz/container-cicd-platform.git
cd container-cicd-platform
```

Install the application dependencies:

```bash
cd app
pip install -r requirements.txt
```

Run the unit tests:

```bash
python -m unittest discover
```

Run linting from the repository root:

```bash
flake8 app
```

Build the container:

```bash
docker build -t container-cicd-platform .
```

## GitHub Actions Workflows

Pipeline execution and individual job logs can be inspected through the repository's **Actions** tab.

The workflow definitions are maintained under:

```text
.github/workflows/
```

This keeps the delivery configuration version-controlled alongside the application.

## Future Improvements

The current platform provides the foundation for a more complete delivery pipeline.

Planned improvements include:

* Restrict container publishing to validated changes on the `main` branch
* Introduce immutable container version tags alongside `latest`
* Expand Zizmor analysis across all GitHub Actions workflows
* Add container vulnerability scanning with Trivy
* Add dependency vulnerability scanning
* Introduce GitHub branch protection and required status checks
* Add automated deployment to AWS
* Replace long-lived AWS credentials with GitHub OIDC where cloud deployment is introduced
* Provision deployment infrastructure using Terraform
* Add Kubernetes deployment automation
* Introduce environment-specific deployment stages
* Add deployment approval gates
* Implement automated rollback strategies
* Add deployment monitoring and notifications

## Project Scope

This project focuses on the automation and security controls surrounding application delivery, including:

* Continuous Integration
* Containerisation
* Automated testing
* Code-quality validation
* Secret scanning
* CI/CD security analysis
* Container artifact publishing
* Git-based development workflows

The repository forms part of my Cloud and DevOps engineering portfolio and is designed to evolve as additional infrastructure, deployment, security, and observability capabilities are implemented.
