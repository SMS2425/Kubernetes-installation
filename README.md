# Kubernetes-installation
kubernetes installation
-----------------------------
Step 💯

sudo apt update
sudo apt install docker
sudo apt install docker.io
sudo docker images
 
Requirements : 
Master: 2 vCPUs
Worker: 1 vCPUs
OS:     Ubuntu 

Step 1: Install docker along with below tools in Master 

kubeadm - bootstraps a Kubernetes cluster
kubelet - configures containers to run on a host
kubectl - deploys and manages apps on Kubernetes

apt-get update && apt-get install -y apt-transport-https

curl -s https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

apt update && apt install -qy docker-ce
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update

systemctl enable docker && systemctl start docker

go to /etc/docker, inside the location we need to keep daemon file (daemon.json)
daemon.json containes below configuration :
{

"exec-opts": ["native.cgroupdriver=systemd"]

}
chmod 777 daemon.json

after we updated above content in daemon.json then we have to restart docker daemon service by using below command 
sudo systemctl restart docker 


 
apt-get update && apt-get install -y kubeadm kubelet kubectl


#Disable swap memory (if running) on both the nodes
sudo swapoff -a

-------------------------------------------------------------------------------------------------------------
Step 2: once you installed requred softwares ,make sure to up and running 

# Start and enable docker and kubectl

systemctl enable docker && systemctl start docker
systemctl enable kubelet && systemctl start kubelet

#apche2 installation
apt-cache search apache
sudo apt-get install apache2

**Setting Unique Hostnames**
sudo hostnamectl set-hostname kubernetes-master
sudo hostnamectl set-hostname kubernetes-worker1
sudo hostnamectl set-hostname kubernetes-worker2

----------------------------------------------------------------------------------------------------------
Step 3: configure kubernets in master node 
kubeadm init --pod-network-cidr=10.240.0.0/16

Once you entered above command you will get join commads for worknodes 

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

other one we need to copy join command for worknodes 
-----------------------------------------------------------------------------------------------------------------------------
Step 4: 
Installing Flannel network-plug-in for cluster network

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl get pods --all-namespaces
--------------------------------------------------------------------------------------------------------------------
Step 5: configure work node and install all required packages in step1 and step2.

-------------------------------------------------------------------------------------------

step 6 : Get exact join command from previous kubeadm init command output.

-----------------------------------------------------------------------------------------------------------------------------
step 7 : testing 

# Display nodes status
kubectl get nodes

# Deploying sample application
kubectl apply -f https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/controllers/nginx-deployment.yaml

# Displaying Pod status
kubectl get po -o wide

# Displaying all name spaces 
kubectl get pods --all-namespaces

