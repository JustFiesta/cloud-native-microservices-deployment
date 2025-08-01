﻿# Docker + Kubernetes deployment

This repository contains resources for successful deployment of [microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo) project.

It was made as a practical task to test my knowlage for containers creation/security and Kubernetes on Cloud.

## Technologies

* Docker
* AWS
* Terraform
* Managed Kubernetes
* Helm

## Goals

1. Create infrastructure for AWS managed Kubernetes Cluster
2. Containerize the apps from [microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo), with the smallest possible image without drawbacks
3. Scan containers with sec tools: Trivy, Checkov, Docker Scout
4. Push to cloud managed repository
5. Create K8 manifests do deploy application
6. Scan manifests with sec tools
7. Create and deploy app into Helm Charts
8. Add Redis to application stack
