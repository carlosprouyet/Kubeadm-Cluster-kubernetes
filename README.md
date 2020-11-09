# Descargar Máquinas virtuales
-   https://mega.nz/folder/CVkkhRAb#au9ZmkjGU85ORWkyJPmnIg
# Cluster-Kubeam-en-Centos
-   Clúster de kubernetes con kubeadm en sistema opperativo centos7.
-   Son tres máquinas virtuales de VMware.
-   Entorno no productivo debido a los poco recursos de las máquinas pero si la metodología de instalación y configuración de las mismas.
-   Para las tres másquinas :
```
Usuario: kubeadm
Contraseña: kubeadm
Contraseña de root: kubeadm
```

# Configuraciones a tener en cuenta
-   En la carpeta de configuraciones podremos ver los pasos que hemos seguido para la configuración de las máquinas.
-   Las máquinas están asignadas con un ip por lo que tendremos que asignarles estas ips o eliminar el clúster y volver hacer un "kubeadm init" para volver a crearlo y volver añadir los nodos al clúster.
-   Ips de las máquinas:
```
192.168.244.110 centos-kubeadm-1
192.168.244.111 centos-kubeadm-2
192.168.244.113 centos-kubeadm-3
```
-   Si estamos en nuestro pc podremos seguir el siguiente tutorial para configurar ips fijas a nuestras máquinas virtuales.
-   https://www.vichaunter.org/como-se-hace/como-poner-ip-fija-en-vmware-workstation-a-cualquier-vm-en-nat
-   centos-kubeadm-1 es el nodo master y ya tiene las configuraiones apropiadas para ejecutar kubectl.
# Red de Pods
-   Como red de pods hemos instalado Calico sin almacén de datos os dejo la url del proyecto por si queréis hacer configuraciones adicionales.
-   https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises
-   Dejo documentación oficial para configuraciones de red de kubernetes.io.
-   https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/

# Instalación kubeadm kubelet kubectl
-   Para su instalación hemos seguido la documentación oficial de kubernetes.io, es recomendable leersela debido a que si estamos utilizando un container runtime diferente a docker habrá que hacer configuraciones a dicionales.
-   https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
# Upgrade clúster
-    En la siguiente url podremos consultar como subir de versión nuestro clúster.
-   https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

# NOTA
-   Cuando hagamos por primeravez kubeadm init nos dará una serie de instrucciones para poder configurar correctamente kubectl, la carpeta de configuración la podemos encontrar en /home/kubeadm/.kube

