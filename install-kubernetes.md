Tutorial Install Kubernetes

Disabling image swap for kubelet
sudo swapoff -a
sudo nano /etc/fstab
comment -> /swap.img none swap sw 0 0

Update Depedencies
sudo apt update
sudo apt upgrade -y

Install Depedencies
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

Install Docker
https://github.com/nekaartajaya/script
docker.sh

Change Docker daemon to use systemd
Opsi 1
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
sudo nano /etc/containerd/config.toml
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options] SystemdCgroup = false -> change to true
sudo systemctl restart containerd
sudo systemctl enable containerd

Opsi 2
cat <<EOF | sudo tee /etc/docker/daemon.json
{
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": {
"max-size": "100m"
},
"storage-driver": "overlay2"
}
EOF
sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker

Enable Forward IP
sudo nano /etc/sysctl.conf
remove # in net.ipv4.ip_forward = 1

Install K8S
https://github.com/nekaartajaya/script
k8s.sh

Init Control Plane (Master k8s)
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 -> for calico network
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 -> for flannel network

Create folder config for user
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

Install Calico for network
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/master/manifests/calico.yaml -n kube-system

Verify nodes and pods
kubectl get nodes
kubectl get pods

Delete Taint in Control Plane (Master) for single node (Optional)
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
