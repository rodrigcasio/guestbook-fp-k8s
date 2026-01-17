Local Kubernetes Lab: Guestbook-Go

This guide documents the deployment and management of the Guestbook-Go application. The laboratory exercises were performed on Arch Linux using Minikube as the local cluster provider and the kubectl CLI for resource orchestration. The setup emphasizes the use of Kubernetes Namespaces for resource isolation and Horizontal Pod Autoscaling (HPA) for performance management.

1. Environment Initialization

Prepare the local cluster and the metrics server for monitoring and scaling capabilities.

Start Docker Engine:
(sudo systemctl start docker)

Start Minikube:
(minikube start --driver=docker)

Enable Metrics Server for HPA support:
(minikube addons enable metrics-server)

2. Cluster Configuration and Deployment

Set up the isolated namespace and deploy the application resources.

Create the project namespace:
(kubectl create namespace guestbook-go-project)

Apply the Deployment manifest:
(kubectl apply -f deployment-v2.yml)

Apply the Service manifest:
(kubectl apply -f service.yml)

Set up the Horizontal Pod Autoscaler:
(kubectl autoscale deployment guestbook --cpu-percent=50 --min=1 --max=10 -n guestbook-go-project)

3. Verification and Access

Monitor the resources and launch the application.

Check Pod status:
(kubectl get pods -n guestbook-go-project)

Check HPA metrics:
(kubectl get hpa -n guestbook-go-project)

Access the application UI:
(minikube service guestbook -n guestbook-go-project)

4. Updates and Rollbacks

Manage deployment versions and history using imperative commands.

Trigger a Rolling Update:
(kubectl set image deployment/guestbook guestbook=rodrigocasio/guestbook-go:v2 -n guestbook-go-project)

Check deployment status:
(kubectl rollout status deployment/guestbook -n guestbook-go-project)

Perform a Rollback:
(kubectl rollout undo deployment/guestbook -n guestbook-go-project)

View deployment history:
(kubectl rollout history deployment/guestbook -n guestbook-go-project)

5. Cleanup and Shutdown

Commands to clear the environment and stop local services to free system resources.

Delete the project namespace:
(kubectl delete namespace guestbook-go-project)

Stop the Minikube cluster:
(minikube stop)

Stop the Docker service:
(sudo systemctl stop docker)

Remove local Docker images:
(docker rmi rodrigocasio/guestbook-go:v1 rodrigocasio/guestbook-go:v2)

Prune unused Docker data:
(docker system prune -f)
