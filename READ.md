# Kubernetes Installation Commands

### 1. Update System
```bash
sudo apt-get update
sudo apt-get upgrade -y
```

### 2. Install Docker
```bash
sudo apt-get install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

### 3. Install Dependencies
```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
sudo mkdir -p -m 755 /etc/apt/keyrings
```

### 4. Add Kubernetes Repository
```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
```

### 5. Install Kubernetes Components
```bash
sudo apt-get install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1

sudo apt-mark hold kubeadm kubelet kubectl
```

### 6. Initialize Kubernetes Cluster
```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

### 7. Configure kubectl
```bash
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 8. Verify Installation
```bash
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
```

## Sonatype Nexus Repository Setup Using Docker

### 1. Pull Nexus Image

```bash
docker pull sonatype/nexus3
```

### 2. Create Docker Volume

```bash
docker volume create nexus-data
```

### 3. Run Nexus Container

```bash
docker run -d \
  --name nexus \
  -p 8081:8081 \
  -v nexus-data:/nexus-data \
  --restart unless-stopped \
  sonatype/nexus3
```

### 4. Verify Container

```bash
docker ps
```

### 5. Get Admin Password

```bash
docker exec nexus cat /nexus-data/admin.password
```

### 6. Access Nexus

Open in your browser:

```text
http://YOUR_SERVER_IP:8081
```

**Login Credentials:**
- Username: `admin`
- Password: Generated password from Step 5.


