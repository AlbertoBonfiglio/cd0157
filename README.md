# Deploying a Flask API

This is the project starter repo for the course Server Deployment, Containerization, and Testing.

In this project you will containerize and deploy a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

The Flask app that will be used for this project consists of a simple API with three endpoints:

- `GET '/'`: This is a simple health check, which returns the response 'Healthy'.
- `POST '/auth'`: This takes a email and password as json arguments and returns a JWT based on a custom secret.
- `GET '/contents'`: This requires a valid JWT, and returns the un-encrpyted contents of that token.

The app relies on a secret set as the environment variable `JWT_SECRET` to produce a JWT. The built-in Flask server is adequate for local development, but not production, so you will be using the production-ready [Gunicorn](https://gunicorn.org/) server when deploying the app.

## Prerequisites

- Docker Desktop - Installation instructions for all OSes can be found [here](https://docs.docker.com/install/).
- Git: [Download and install Git](https://git-scm.com/downloads) for your system.
- Code editor: You can [download and install VS code](https://code.visualstudio.com/download) here.
- AWS Account
- Python version between 3.7 and 3.9. Check the current version using:

```bash
#  Mac/Linux/Windows 
python --version
```

You can download a specific release version from [here](https://www.python.org/downloads/).

- Python package manager - PIP 19.x or higher. PIP is already installed in Python 3 >=3.4 downloaded from python.org . However, you can upgrade to a specific version, say 20.2.3, using the command:

```bash
#  Mac/Linux/Windows Check the current version
pip --version
# Mac/Linux
pip install --upgrade pip==20.2.3
# Windows
python -m pip install --upgrade pip==20.2.3
```

- Terminal
  - Mac/Linux users can use the default terminal.
  - Windows users can use either the GitBash terminal or WSL.
- Command line utilities:
  - AWS CLI installed and configured using the `aws configure` command. Another important configuration is the region. Do not use the us-east-1 because the cluster creation may fails mostly in us-east-1. Let's change the default region to:

  ```bash
  aws configure set region us-east-2  
  ```

  Ensure to create all your resources in a single region.
  - EKSCTL installed in your system. Follow the instructions [available here](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html#installing-eksctl) or [here](https://eksctl.io/introduction/#installation) to download and install `eksctl` utility.
  - The KUBECTL installed in your system. Installation instructions for kubectl can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## Initial setup

1. Fork the [Server and Deployment Containerization Github repo](https://github.com/udacity/cd0157-Server-Deployment-and-Containerization) to your Github account.
1. Locally clone your forked version to begin working on the project.

```bash
git clone https://github.com/SudKul/cd0157-Server-Deployment-and-Containerization.git
cd cd0157-Server-Deployment-and-Containerization/
```

1. These are the files relevant for the current project:

```bash
.
├── Dockerfile 
├── README.md
├── aws-auth-patch.yml #ToDo
├── buildspec.yml      #ToDo
├── ci-cd-codepipeline.cfn.yml #ToDo
├── iam-role-policy.json  #ToDo
├── main.py
├── requirements.txt
├── simple_jwt_api.yml
├── test_main.py  #ToDo
└── trust.json     #ToDo 
```

## Project Steps

Completing the project involves several steps:

1. Write a Dockerfile for a simple Flask API
2. Build and test the container locally
3. Create an EKS cluster
4. Store a secret using AWS Parameter Store
5. Create a CodePipeline pipeline triggered by GitHub checkins
6. Create a CodeBuild stage which will build, test, and deploy your code

For more detail about each of these steps, see the project lesson.

## Additional steps

To build the docker image locally for testing the environment variables need to be set in a `.env_file` file.

```bash
JWT_SECRET='myjwtsecret'
LOG_LEVEL=DEBUG
```

the command to build is

```bash
docker run --name myContainer --env-file=.env_file -p 80:8080 cd0157:latest'
```
