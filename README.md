# Minikube kubernetes cluster on Apple M1 without Docker Desktop

What you need:
* Apple computer running macOS
* [Homebrew](https://brew.sh)


## Install colima (Containers on Linux on Mac)

$ `brew install colima`

Note: colima has an option --with-kuberenetes, at the time of writing I couldn't get this to work properly and went with minikube instead.


## Install minikube and helm

$ `brew install minikube` (installs minikube and cubectl)

$ `brew install helm`


## Create start script

$ `sudo nano /usr/local/bin/minikube-start`

Paste the following lines, adjust limits to your system/needs:
```bash
#!/bin/bash
colima start --arch aarch64 --cpu 8 --memory 10 --disk 250
minikube start --driver=docker --container-runtime=containerd --cpus=max --memory=max --wait-timeout=10m --base-image=kicbase/stable:v0.0.29
```
Hit ctrl+x, y, enter

$ `sudo chmod +x /usr/local/bin/minikube-start`


## Start minikube cluster

$ `minikube-start`

Output should be similar to this:
```
INFO[0000] using docker runtime                         
INFO[0000] starting colima                              
INFO[0000] starting ...                                  context=vm
INFO[0022] provisioning ...                              context=docker
INFO[0022] starting ...                                  context=docker
INFO[0027] waiting for startup to complete ...           context=docker
INFO[0027] done                                         
😄  minikube v1.25.1 on Darwin 12.2.1 (arm64)
✨  Using the docker driver based on existing profile
❗  You cannot change the CPUs for an existing minikube cluster. Please first delete the cluster.
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
📦  Preparing Kubernetes v1.23.1 on containerd 1.4.12 ...
    ▪ kubelet.housekeeping-interval=5m
    ▪ kubelet.cni-conf-dir=/etc/cni/net.mk
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

🎉 Concratulations! You now have an arm-based kubernets cluster running locally.

Note: If you have trouble with minikube not downloading images try to run `docker load kicbase/stable:v0.0.29`

## Resources

* https://kubernetes.io
* https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* https://minikube.sigs.k8s.io/docs/
* https://github.com/abiosoft/colima
* https://github.com/lima-vm/lima
