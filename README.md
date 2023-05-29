# Infrastructur Setup With Kubernetes

## Prerequisites

Make sure you have installed all of the following prerequisites on your development machine:

- Git - [Download & Install Git](https://git-scm.com/downloads). OSX and Linux machines typically have this already installed.
- Node.js - [Download & Install Node.js](https://nodejs.org/en/download/) and the npm package manager.
- Docker - [Download & Install Docker](https://docs.docker.com/get-docker/).
- Minikube - [Download & Install Minikube](https://minikube.sigs.k8s.io/docs/start/) for local development only.

## Local Development

Clone all the service repositories into your local machine and make sure you arrange the folder as shown below,

![Directory structure](images/directory.png)

### Local Development With Docker Compose

1. Enter `infra/proxy` directory
1. Copy `.env.example` into `.env` and make necessary changes
1. Run docker compose, `docker-compose up --build`

### Local Development With Minikube

1. Install Minikube
1. Start Minikube: `minikube start --memory=16384 --cpus=4` (ref: [https://istio.io/latest/docs/setup/platform-setup/minikube/#installation-steps](https://istio.io/latest/docs/setup/platform-setup/minikube/#installation-steps))
1. Install Istioctl (ref: [https://istio.io/latest/docs/setup/install/istioctl/](https://istio.io/latest/docs/setup/install/istioctl/))
1. Run: `istioctl install`
1. Install Skaffold (ref: [https://skaffold.dev/docs/install/](https://skaffold.dev/docs/install/))
1. Set `default` namespace with label `istio-injection=enabled`, run `kubectl label --overwrite ns default istio-injection=enabled`
1. Make sure you're in `infra` directory, run `cd infra`
1. Run: `skaffold dev -f skaffold-local.yaml`

## Remote Deployment

When deploying to remote Kubernetes cluster such as Alibaba Container Service for Kubernetes (CSK) or Google Kubernetes Engine (GKE), we use private [container registry from Github Packages](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

### Authenticating to the Container Registry

You're going to need a Github Personal Access Token (PAT), please refer the [documentation](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) on how to get this.

With PAT you just created, create a secret in Kubernetes cluster using the command below,

```
kubectl create secret docker-registry ghcr \
--docker-server=ghcr.io \
--docker-username=YOUR_GITHUB_USER_NAME \
--docker-password=YOUR_PAT_TOKEN \
--docker-email=YOUR_EMAIL
```

### Deploying to Kubernetes Cluster

Once the `ghcr` secret created, use the following command to deploy,

`skaffold run --default-repo=ghcr.io/wofood -f skaffold-remote-istio.yaml`
