# 1º Configuraciones a nivel de máquina tras instalar centos.

-   sudo yum install -y epel-release
-   sudo yum update -y

# 2º Configuraciones para nuestro clúster
```
$ sudo swapoff -a
$ sudo vim /etc/fstab : aquí comentamos la siguiente linea : dev/mapper/centos-swap swap                    swap    defaults        0 0
$ Rebotamos el servidor
$ sudo vim /etc/sysctl.conf, y ñadimos las siguiientes lineas para que se pueda aceptar el tráfico de tipo bridge
   net/bridge/bridge-nf-call-ip6tables = 1
   net/bridge/bridge-nf-call-iptables = 1
   net/bridge/bridge-nf-call-arptables = 1
$ sudo yum install  ebtables ethtool
$ firewall-cmd --permanent --add-port=6443/tcp
$ firewall-cmd --permanent --add-port=2379-2380/tcp
$ firewall-cmd --permanent --add-port=10250/tcp
$ firewall-cmd --permanent --add-port=10251/tcp
$ firewall-cmd --permanent --add-port=10252/tcp
$ firewall-cmd --permanent --add-port=10255/tcp
$ firewall-cmd --reload
$ sudo modprobe br_netfilter
$ echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
$ Rebotamos el servidor.
```

#### $ sudo vim /etc/hosts:
```
127.0.0.1   localhost centos-kubeadm-1
::1         localhost centos-kubeadm-1

192.168.244.110 centos-kubeadm-1
192.168.244.111 centos-kubeadm-2
192.168.244.113 centos-kubeadm-3

```
# 3º Instalación de docker
```
$ sudo yum install -y yum-utils
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum install docker-ce docker-ce-cli containerd.io
$ systemctl start docker
$ systemctl enable docker
$ sudo usermod -aG docker ${USER}
$ Rebotamos el servidor
```

# 4º Instalación de kubeadm kubelet kubectl
```
$ cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

$ sudo setenforce 0
$ sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
$ sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
$ sudo systemctl enable --now kubelet
```

# 5º kubeadm init -- solo nodo control-plane
```
$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16
$ mkdir ~/Escritorio/calico
$ curl https://docs.projectcalico.org/manifests/calico.yaml -O
$ kubectl apply -f calico.yaml
```

# 6º Nodo worker 
sudo kubeadm join 192.168.244.110:6443 --token lydbwj.83remcypjjs1bhaj \
    --discovery-token-ca-cert-hash sha256:2c61e576a896edc844bf0b95175b2352158fbba79cca8b86a5ad668be6bea920 


