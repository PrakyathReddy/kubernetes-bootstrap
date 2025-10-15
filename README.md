# kubernetes bootstrap without scripts

This repo follows the steps i took to bootstrap kubernetes the hard way, i.e. without using any scripts or package managers or kubeadm, etc.

### Pre-reqs
- launched 3 ec2 instances

1. control-plane: Debian 13 arm, t4g.medium, 30 GB storage
2. worker-1: Debian 13 arm, t4g.small, 20GB storage
3. worker-2: Debian 13 arm, t4g.small, 20GB storage

- enable passwordless authentication for all 3 hosts to make it easy going forward

$ ssh-copy-id -f "-o IdentityFile \<path-to-pem-file\>" admin@\<public-ip\>

$ ssh admin@\<public-ip\>

