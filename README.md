# 🚀 Kubernetes Deployment and Monitoring with Kind, Prometheus & Grafana

![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerization-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-Cloud-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)

## 📌 Project Overview

This project demonstrates how to deploy containerized applications using Kubernetes and monitor application and cluster metrics using Prometheus and Grafana.

The project uses **Kind (Kubernetes in Docker)** to create a local Kubernetes cluster, Kubernetes manifests to deploy application components, and monitoring tools to visualize metrics through dashboards.

It also includes Docker Compose configurations and application components such as `vote`, `result`, and `worker`.

The project demonstrates container orchestration, application deployment, service communication, monitoring, and troubleshooting.

## 🎯 Project Objectives

- Create and manage a Kubernetes cluster using Kind.
- Deploy containerized applications to Kubernetes.
- Configure Kubernetes Deployments, Services, and other resources.
- Understand communication between application components.
- Configure monitoring using Prometheus and Grafana.
- Visualize monitoring metrics using Grafana dashboards.
- Understand container health checks and application health monitoring.
- Learn Kubernetes troubleshooting and resource management.
- Explore Kubernetes configuration and deployment automation.

## 🏗️ Architecture

The project follows a containerized application architecture in which Kubernetes manages application workloads and the monitoring stack collects and visualizes metrics.

![Kubernetes Monitoring Architecture](architecture.excalidraw.png)

### Architecture Components

| Component | Description |
|---|---|
| Kind | Creates a local Kubernetes cluster using Docker containers as nodes. |
| Kubernetes | Manages application deployment, scheduling, services, and container recovery. |
| Docker | Builds and runs container images. |
| Vote Application | Provides the voting application component. |
| Result Application | Provides the results application component. |
| Worker | Processes application data between relevant services. |
| Seed Data | Provides supporting data for the application. |
| Kubernetes Manifests | Define application deployments, services, and other Kubernetes resources. |
| Prometheus | Collects metrics from configured monitoring targets. |
| Grafana | Visualizes metrics through dashboards. |
| Health Checks | Help verify application or container health. |

> **Note:** The exact components monitored by Prometheus depend on the exporters, ServiceMonitors, and scrape configurations installed in the cluster.

## 🛠️ Technologies Used

### Containerization
- Docker
- Docker Compose
- Docker images and containers

### Container Orchestration
- Kubernetes
- Kind
- kubectl
- Kubernetes YAML manifests

### Monitoring and Visualization
- Prometheus
- Grafana
- Kubernetes monitoring metrics

### Cloud and DevOps
- AWS EC2, if deploying the environment on AWS
- Git and GitHub
- YAML configuration
- Linux command-line tools

## 📂 Project Structure

```text
kubernetes-kind-grafanaDashboard-project/
│
├── .github/
├── .vscode/
├── healthchecks/
├── k8s-specifications/
├── kind-cluster/
├── result/
├── seed-data/
├── vote/
├── worker/
│
├── architecture.excalidraw.png
├── docker-compose.images.yml
├── docker-compose.yml
├── docker-stack.yml
├── grafana.png
├── .gitignore
├── LICENSE
└── README.md
```

### Directory Explanation

- **`.github/`** — GitHub-related configuration and workflows, if configured.
- **`.vscode/`** — Visual Studio Code workspace settings.
- **`healthchecks/`** — Health-check-related configuration or supporting files.
- **`k8s-specifications/`** — Kubernetes YAML manifests.
- **`kind-cluster/`** — Kind cluster configuration files.
- **`result/`** — Result application files.
- **`seed-data/`** — Supporting data or data initialization files.
- **`vote/`** — Voting application files.
- **`worker/`** — Worker application files.
- **`architecture.excalidraw.png`** — Project architecture diagram.
- **`docker-compose.yml`** — Docker Compose service configuration.
- **`docker-compose.images.yml`** — Additional Docker Compose image configuration.
- **`docker-stack.yml`** — Docker Stack configuration.
- **`grafana.png`** — Grafana dashboard screenshot.

## ⚙️ Prerequisites

Install the following tools before running the project:

1. Docker Desktop or Docker Engine
2. kubectl
3. Kind
4. Helm, if using Helm to install the monitoring stack
5. Git
6. A code editor such as Visual Studio Code

Verify the installations:

```bash
docker --version
kubectl version --client
kind version
helm version
git --version
```

> Helm is required only if you use Helm-based installation instructions. Install it before running the corresponding commands.

## 🚀 Step 1: Clone the Repository

```bash
git clone https://github.com/shubhamdadwal1/kubernetes-kind-grafanaDashboard-project.git
```

Navigate to the project directory:

```bash
cd kubernetes-kind-grafanaDashboard-project
```

Review the project files:

```bash
ls
```

On Windows PowerShell, you can also use:

```powershell
Get-ChildItem
```

## 🐳 Step 2: Verify Docker

Check that Docker is running:

```bash
docker info
```

List running containers:

```bash
docker ps
```

Docker must be running before you create a Kind cluster.

## ☸️ Step 3: Create a Kind Cluster

First, inspect the available configuration files:

```bash
ls kind-cluster
```

If the directory contains a valid Kind configuration file, create the cluster using its actual filename:

```bash
kind create cluster --config kind-cluster/<your-config-file>.yaml
```

Replace `<your-config-file>.yaml` with the real configuration filename.

If you do not have a custom configuration file, create a default cluster:

```bash
kind create cluster --name monitoring-cluster
```

Check the cluster:

```bash
kind get clusters
```

Check the Kubernetes nodes:

```bash
kubectl get nodes
```

Verify the active context:

```bash
kubectl config current-context
```

For the default cluster created above, the context is normally:

```text
kind-monitoring-cluster
```

## 📦 Step 4: Deploy Kubernetes Resources

Inspect the Kubernetes manifests:

```bash
ls k8s-specifications
```

Before applying them, review the YAML files and confirm that image names, namespaces, ports, and dependencies are configured correctly.

If the directory contains the complete application manifests, deploy them using:

```bash
kubectl apply -f k8s-specifications/
```

Check the deployments:

```bash
kubectl get deployments -A
```

Check the pods:

```bash
kubectl get pods -A
```

Check the services:

```bash
kubectl get services -A
```

Check the resources in a specific namespace:

```bash
kubectl get all -n default
```

> Apply only the manifests intended for your current cluster. If monitoring resources are stored in another directory, follow the deployment instructions for those files separately.

## 🔍 Step 5: Verify Application Deployment

Check whether the application pods are running:

```bash
kubectl get pods
```

Inspect a particular pod:

```bash
kubectl describe pod <pod-name>
```

View application logs:

```bash
kubectl logs <pod-name>
```

Follow logs continuously:

```bash
kubectl logs -f <pod-name>
```

If the pod contains multiple containers, specify the container:

```bash
kubectl logs <pod-name> -c <container-name>
```

Check application services:

```bash
kubectl get svc
```

If a service uses NodePort, access it using the appropriate node address and port, provided that the Kind networking configuration exposes that port.

For local testing, port forwarding is another option:

```bash
kubectl port-forward service/<service-name> 8080:<service-port>
```

Then open:

```text
http://localhost:8080
```

Replace the service name and port with the actual values in your manifests.

## 📊 Step 6: Install Prometheus and Grafana

If Prometheus and Grafana are not already installed, one option is to use the Prometheus Community Helm chart.

Add the Helm repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update the repository:

```bash
helm repo update
```

Create a monitoring namespace:

```bash
kubectl create namespace monitoring
```

Install the monitoring stack:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

Check the deployed resources:

```bash
kubectl get pods -n monitoring
```

Check the services:

```bash
kubectl get svc -n monitoring
```

Wait for the monitoring pods to become ready:

```bash
kubectl wait --for=condition=Ready pods --all -n monitoring --timeout=300s
```

> If the project already contains monitoring manifests or a Helm configuration, review those files first. Avoid installing a second monitoring stack unnecessarily.

## 📈 Step 7: Access Grafana

Find the Grafana service:

```bash
kubectl get svc -n monitoring
```

For the standard `kube-prometheus-stack` chart, the service is commonly named `monitoring-grafana`.

Forward the service port:

```bash
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
```

Open Grafana in your browser:

```text
http://localhost:3000
```

### Retrieve the Initial Admin Password

For the Helm installation above, retrieve the password using:

```bash
kubectl get secret monitoring-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode
```

On Windows PowerShell, use:

```powershell
$secret = kubectl get secret monitoring-grafana -n monitoring -o jsonpath="{.data.admin-password}"
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($secret))
```

The default username for this chart is generally:

```text
admin
```

Use the password returned by the command.

> The secret name can differ if you use a different Helm release name or custom configuration. Never publish your Grafana password or Kubernetes secrets in this repository.

## 📉 Step 8: Explore Grafana Dashboards

After logging in:

1. Open the Grafana dashboard interface.
2. Navigate to the available dashboards.
3. Explore Kubernetes cluster and workload metrics.
4. Review CPU and memory utilization where metrics are available.
5. Inspect pod and node health.
6. Check workload performance and resource usage.
7. Create custom dashboards if required.

### Example Metrics

Depending on the monitoring configuration, dashboards may display:

- CPU utilization
- Memory utilization
- Node readiness
- Pod status
- Container restarts
- Network traffic
- Kubernetes workload health
- Resource requests and limits
- Application-specific metrics

## 📷 Grafana Dashboard

The following screenshot is included in this repository:

![Grafana Dashboard](grafana.png)

## 🧪 Step 9: Kubernetes Troubleshooting Commands

### Check Nodes

```bash
kubectl get nodes -o wide
```

### Check Pods

```bash
kubectl get pods -A
```

### Check Deployments

```bash
kubectl get deployments -A
```

### Check Services

```bash
kubectl get services -A
```

### Inspect Pod Events

```bash
kubectl describe pod <pod-name>
```

### Check Pod Logs

```bash
kubectl logs <pod-name>
```

### Check Recent Events

```bash
kubectl get events -A --sort-by=.metadata.creationTimestamp
```

### Check Resource Usage

```bash
kubectl top nodes
kubectl top pods -A
```

The `kubectl top` commands require Metrics Server or another compatible metrics provider to be installed and working.

## 🛠️ Common Problems and Solutions

### 1. Docker Is Not Running

**Problem:** Kind cannot create a cluster.

**Solution:** Start Docker Desktop or the Docker service and verify it:

```bash
docker info
```

### 2. Pods Are in Pending State

Inspect the pod:

```bash
kubectl describe pod <pod-name>
```

Check for insufficient resources, scheduling constraints, missing persistent volumes, or other events.

### 3. Pods Are in ImagePullBackOff

Inspect the pod:

```bash
kubectl describe pod <pod-name>
```

Verify the image name, image tag, registry access, and image pull secrets.

### 4. Pods Are in CrashLoopBackOff

Check the logs:

```bash
kubectl logs <pod-name>
```

If the container restarted, inspect its previous logs:

```bash
kubectl logs <pod-name> --previous
```

Review application configuration, environment variables, dependencies, and health probes.

### 5. Grafana Is Not Accessible

Check the Grafana pod:

```bash
kubectl get pods -n monitoring
```

Check the service:

```bash
kubectl get svc -n monitoring
```

Confirm that port forwarding is running and that the local port is available.

### 6. Prometheus Is Not Collecting Metrics

Check the monitoring pods:

```bash
kubectl get pods -n monitoring
```

Review Prometheus targets, scrape configuration, ServiceMonitors, and exporter availability.

Remember that deploying Prometheus does not automatically guarantee that every application exposes metrics.

## 🔐 Best Practices

- Use meaningful names for Kubernetes resources.
- Keep credentials and sensitive configuration outside Git.
- Use Kubernetes Secrets for sensitive application values.
- Define CPU and memory requests and limits where appropriate.
- Configure readiness and liveness probes according to application requirements.
- Use appropriate health checks.
- Review application logs when troubleshooting.
- Monitor cluster and workload resource consumption.
- Use version-controlled Kubernetes manifests.
- Avoid exposing monitoring dashboards publicly without appropriate authentication and network controls.
- Keep container images and Kubernetes dependencies updated.
- Test configuration changes before applying them to production environments.

## 🧹 Step 10: Clean Up

Delete the Kubernetes resources created from the project manifests:

```bash
kubectl delete -f k8s-specifications/
```

This command deletes the resources described by the manifests. Review the files first and ensure you do not need to preserve any of the resources or data.

If you installed the monitoring stack using the Helm command above, uninstall it:

```bash
helm uninstall monitoring -n monitoring
```

Delete the monitoring namespace if it is no longer required:

```bash
kubectl delete namespace monitoring
```

Delete the Kind cluster if you created the default cluster using the earlier command:

```bash
kind delete cluster --name monitoring-cluster
```

If you used a different cluster name, substitute that name in the command.

## 💼 Skills Demonstrated

This project provides practical experience with:

- Containerization using Docker
- Kubernetes cluster management
- Local Kubernetes development using Kind
- Kubernetes deployments and services
- YAML-based infrastructure configuration
- Application health checks
- Monitoring with Prometheus
- Dashboard visualization using Grafana
- Kubernetes troubleshooting
- Container and application log analysis
- DevOps workflow fundamentals
- Version control using Git and GitHub

## 📚 Learning Outcomes

By working on this project, you can learn how to:

1. Build and run containerized applications.
2. Create a local Kubernetes cluster.
3. Deploy and inspect Kubernetes resources.
4. Understand how application components communicate.
5. Collect and visualize infrastructure metrics.
6. Investigate pod failures and deployment problems.
7. Use dashboards to understand application and cluster health.
8. Maintain Kubernetes configuration files in Git.

## 🚀 Future Improvements

Potential improvements include:

- Configure application-specific Prometheus metrics.
- Add custom Grafana dashboards and alerts.
- Configure resource requests and limits for all workloads.
- Implement Horizontal Pod Autoscaling where suitable.
- Add automated deployment pipelines using Jenkins or GitHub Actions.
- Use Argo CD for GitOps-based deployment if required.
- Deploy the architecture on AWS using an appropriate managed Kubernetes service.
- Add centralized logging using tools such as Loki.
- Document recovery, backup, and operational procedures.

These are potential extensions and should not be considered implemented unless they are configured in the project.

## 👨‍💻 Author

**Shubham Dadwal**

Cloud Computing | DevOps | Kubernetes | AWS

GitHub: [@shubhamdadwal1](https://github.com/shubhamdadwal1)

## 🔗 Project Repository

[View Kubernetes Kind Grafana Dashboard Project on GitHub](https://github.com/shubhamdadwal1/kubernetes-kind-grafanaDashboard-project)

## ⭐ Support

If you find this project useful for learning Kubernetes, Docker, Prometheus, and Grafana, consider giving the repository a star.

---

**Built to practice Kubernetes deployment, container orchestration, and monitoring.**
