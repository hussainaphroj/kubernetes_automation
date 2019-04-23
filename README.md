# kubernetes_automation
Setup Kubernetes Cluster with Zero Cost

This tutorial will help you to setup Kubernetes cluster (1 Master and 2 worker Nodes) on your Laptop. You need have windows 10 with minimum 8 GB of RAM. We will bootstrap all 3 machine using vagrant and Oracle Virtual Box.
Master: 192.168.56.100
worker1:192.168.56.101
worker2: 192.168.56.102
 To start with, please install latest vagrant, virtual Box and Git on your windows and reboot it. Please make sure the VT-visualization is enable at BIOS.
 Clone this repository and cd to Kubernetes_Ubuntu
 
 Create Virtual Machines:
Please follow the following steps to create 3 ubuntu-18.04 boxes with Vagrant. Before start, we should know our Ethernet adapter VirtualBox Host-Only Network ips. It is required since each box created with Vagrant will have 10.0.2.15 for eth0 by default and we want to setup our own private ips which will help us to communicate internally and outside network(internet). You can get to know this ip by executing ipconfig in command prompt.
Once all 3 boxes are ready, you can login to those boxes by running "vagrant ssh" from respective directory/folder.

Run on only on Master:
 sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.56.100 (as vagrant user).
 you will get the following output from above command:
 Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 192.168.56.100:6443 --token sgqytl.0e2fuz8h8d40insu --discovery-token-ca-cert-hash sha256:f48196753fdfddddfdd

  
  kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
  
  
  kubectl get nodes (wait for sometime, it status will become ready)
  
  Run on worker1/woroker2 to join these workers on masters.
  sudo kubeadm join 192.168.56.100:6443 --token sgqytl.0e2fuz8h8d40insu --discovery-token-ca-cert-hash sha256:f48196753dfdfddd
  
 Congratulation! You have successfully setup the Kubernetes cluster. You can verify by running kubectl get nodes on master
 
 We will deploy a apache pod and access it with service which exposed to Nodeport 30080:
 
 cd /vagrant/apache on master instance, the apache directory containing 
 deployment.yaml  service.yaml
 
 create deployment by running   
 kubectl create -f deployment.yaml
 
 kubectl get deployment

vagrant@master:/vagrant/apache$ kubectl get pods

You can access the apache using one of nodeips such http://192.168.56.101:30080

This completes the setup up kubernetes cluster  and deployment of apache.