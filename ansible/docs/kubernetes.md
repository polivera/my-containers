# Steps to install k3s on debian 12

 Review these steps

```
# Update packages
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install -y curl openssh-server

# Disable swap (important for Kubernetes)
sudo swapoff -a
# Make swap disable permanent
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Enable IP forwarding
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```


## On master
Install k3s on master node
```
curl -sfL https://get.k3s.io | sh -s - --disable=traefik --write-kubeconfig-mode 644
```

Get the token from
```
sudo cat /var/lib/rancher/k3s/server/node-token
```

Add resource usage limit
```
# On each node, create a systemd override
sudo mkdir -p /etc/systemd/system/k3s.service.d/
echo -e "[Service]\nCPUQuota=80%\nMemoryLimit=3G" | sudo tee /etc/systemd/system/k3s.service.d/resources.conf
```


## On worker nodes
Install k3s on worker nodes
```
curl -sfL https://get.k3s.io | K3S_URL=https://MASTER_IP:6443 K3S_TOKEN=MASTER_TOKEN sh -
```

Add resource usage limit
```
# On each node, create a systemd override
sudo mkdir -p /etc/systemd/system/k3s-agent.service.d/
echo -e "[Service]\nCPUQuota=80%\nMemoryLimit=3G" | sudo tee /etc/systemd/system/k3s-agent.service.d/resources.conf
```

