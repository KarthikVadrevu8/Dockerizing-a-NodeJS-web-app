# Dockerizing-a-NodeJS-web-app

This project creates a simple **nodeJs** application and deploy it onto a kubernetes cluster to demonstrate CI/CD pipeline setup in Jenkins. 

- Jenkinsfile describes various stages and steps to be run to build and deploy the application. This file is the reference for Jenkins to run the set of actions expected as part of the CI/CD pipeline.
- Dockerfile provides set of instructions to Docker to build the target image from source code.
- deployment.yaml is the kubernetes manifest file used to create deployments which in turn deploys pods that run application workloads on AKS cluster.
- package.json records important metadata about a project which is required before publishing to NPM, and also defines functional attributes of a project that npm uses to install dependencies, run scripts, and identify the entry point to our package
- server.js is the main application code
- server.test.js is the unit test script run by Jest to validate the build


### Tools used in the CI/CD process:

- **GitHub** to store source code
- **Jenkins** as DevOps tool to build and deploy the application
- **Sonarqube** to scan the source code (Static code analysis)
- **Docker/DockerHub** to build docker and store the docker images
- **AKS** to deploy the application as containers running on Azure managed Kubernetes Setup
- **node.JS** (JavaScript runtime) to build the application written in JavaScript
- **Jest** to perform unit tests on JavaScript codebase

### Scope of Testing:

Unit tests are included as part of the build process. Jest is the test framework used to run unit tests on the source code (written in JavaScript). 





