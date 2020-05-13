# Ansible Role: K8s master

[![Build Status](https://travis-ci.com/yanehi/ansible-role-k8smaster.svg?branch=master)](https://travis-ci.org/yanehi/ansible-role-k8smaster)

An Ansible Role that installs a Kubernetes master node with kubeadm on Ubuntu.

## Requirements

Requires Docker; recommended role for Docker installation: `yanehi.docker`.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yml
k8s_major_version: 1
k8s_minor_version: 18
k8s_patch_version: 1
k8s_version: "{{ k8s_major_version }}.{{ k8s_minor_version }}.{{ k8s_patch_version }}"
```
Specify the k8s version you want to use. Set `major`, `minor` and `patch` of the k8s version.

```yml
k8s_repo: deb http://apt.kubernetes.io/ kubernetes-xenial main
debian: apt
k8s_apt_gpg_key_url: https://packages.cloud.google.com/{{ debian }}/doc/apt-key.gpg
```
Add the GPG key and repository for the kubeadm installation components. 


```yml
calico_network_plugin: https://docs.projectcalico.org/manifests/calico.yaml
pod_subnet: 192.168.0.0/16
```
The k8s master node uses [calico](https://docs.projectcalico.org/introduction/) as network solution. Specify the pod subnet you want to pass to `kubeadm init` command.

```yml
k8s_master_port: 6443
etcd_connection_port: 2379
etcd_raft_port: 2380
k8s_master_ip: 192.168.56.160
k8s_master_hostname: k8smaster
k8s_home: /root
k8s_user: root
```
Change the k8s-master ip for the `etc/hosts` entry and `kubeadm init` command.

```yml
k8s_components:
    - kubeadm={{ k8s_version }}-00
    - kubelet={{ k8s_version }}-00
    - kubectl={{ k8s_version }}-00
```

Installs the k8s components which will be needed on every node. The version you specified in `default/main.yml`.

## Disable swap on host

```yml
- name: Disable swap
  shell: swapoff -a

- name: Disable swap permanently
  replace:
    path: /etc/fstab
    regexp: '^(\s*)([^#\n]+\s+)(\w+\s+)swap(\s+.*)$'
    replace: '#\1\2\3swap\4'
    backup: yes
```

Kubernetes nodes require to disable swap.

## License

MIT

## Author Information

This role was created in 2020 by Yannic Nevado.