# Pre-requisites

## EC2 Instances
- launched 3 ec2 instances

1. control-plane: Debian 13 arm, t4g.medium, 30 GB storage
2. worker-1: Debian 13 arm, t4g.small, 20GB storage
3. worker-2: Debian 13 arm, t4g.small, 20GB storage

## Enable passwordless authentication
- enable passwordless authentication for all 3 hosts to make it easy going forward

```bash
$ ssh-copy-id -f "-o IdentityFile <path-to-pem-file>" admin@<public-ip>

$ ssh admin@<public-ip>
```

## Install dependencies on local machine (Mac OS)
```bash
$ brew update
$ brew install wget curl openssl vim git
```

## Download and extract binaries

Download binaries for various k8s components and place them in the newly created downloads directory:
```bash
$ wget -q --show-progress \
  --https-only \
  --timestamping \
  -P downloads \
  -i source/kubernetes-the-hard-way/downloads-arm64.txt
```

Components downloaded: kube-apiserver, kube-controller-manager, kubectl, kube-scheduler, kube-proxy, kubelet, crictl-v1.32.0-linux-arm64, cni-plugins-linux-arm64-v1.6.2, containerd-2.1.0-beto.0-linux-arm64, etcd-v3.6.0-rc.3-linux-arm64

Extract component binaries:
```bash
$ mkdir -p downloads/{client,cni-plugins,controller,worker}
$ tar -xvf downloads/crictl-v1.32.0-linux-${ARCH}.tar.gz \
    -C downloads/worker/
$ tar -xvf downloads/containerd-2.1.0-beta.0-linux-arm64.tar.gz \
    --strip-components 1 \
    -C downloads/worker/
$ tar -xvf downloads/cni-plugins-linux-arm64-v1.6.2.tgz \
    -C downloads/cni-plugins/
$ tar -xvf downloads/etcd-v3.6.0-rc.3-linux-arm64.tar.gz \
    -C downloads/ \
    --strip-components 1 \
    etcd-v3.6.0-rc.3-linux-arm64/etcdctl \
    etcd-v3.6.0-rc.3-linux-arm64/etcd
$ mv downloads/{etcdctl,kubectl} downloads/client/
$ mv downloads/{etcd,kube-apiserver,kube-controller-manager,kube-scheduler} \
    downloads/controller/
$ mv downloads/{kubelet,kube-proxy} downloads/worker/
$ mv downloads/runc.${ARCH} downloads/worker/runc
$ rm -rf downloads/*gz
```

Modify permissions to make binaries executable:
```bash
$ chmod +x downloads/{client,cni-plugins,controller,worker}/*
```

## Install kubectl on local machine
```bash
$ sudo cp downloads/client/kubectl /usr/local/bin
```