# Install metallb

First create namespace
```bash
kubectl create namespace metallb-system
```

And then install through helm
```bash
helm install metallb metallb/metallb --namespace metallb-system
```

After that apply config yaml file
