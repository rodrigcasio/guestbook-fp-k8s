Local Lab Execution Guide: Guestbook-py (Go Version)

Guide for the Guestbook Final Project locally on Arch Linux using Minikube and Docker Hub.

1. Environment Setup

OS: Arch Linux

Container Runtime: Docker (sudo systemctl start docker)

Local Cluster: Minikube (minikube start --driver=docker)

Add-ons: minikube addons enable metrics-server (Required for HPA)

2. Image Management (Docker Hub)

Instead of using IBM Cloud Container Registry, images were pushed to a personal Docker Hub account.

V1 Build: docker build -t rodrigocasio/guestbook-py:v1 .

V2 Build: docker build -t rodrigocasio/guestbook-py:v2 . (After modifying public/index.html)

3. Declarative Deployment Configuration

Two manifest files were created to maintain state:

deployment-v1.yml: Configured with image: v1 and initial resource requests.

deployment-v2.yml: Updated with image: v2 and optimized resource limits (5m CPU) for local HPA testing.

4. Kubernetes Operations

Deployment: kubectl apply -f deployment-v2.yml

Autoscaling: kubectl autoscale deployment guestbook --cpu-percent=50 --min=1 --max=10

Verification: kubectl get hpa (Verified TARGETS reached 0%/50% or higher).

Rolling Update: kubectl set image deployment/guestbook guestbook=rodrigocasio/guestbook-py:v2

Rollback: kubectl rollout undo deployment/guestbook

5. Accessing the App

Command: minikube service guestbook or kubectl port-forward deployment/guestbook 3000:3000
