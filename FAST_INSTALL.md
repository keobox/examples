Fast Installation
=================
The following steps install the little HA Cluster of the book.
* 3 Master/Control-Plane node
* 1 Worker Node

Environment
-----------
* MacOS Ventura 13.2
* Intel 6 Core i7
* 32G RAM
* VirtualBox 7.0.6
* Vagrant >= 2.3.4
* Python >= 3.8.12
* Pyenv >= 2.10
* Ansible latest
* kubectl latest

Steps
=====

Requirements
------------

```shell
pyenv virtualenv k8s-ha
pyenv shell k8s-ha
pip install --upgrade pip
cd setup
pip install ansible jmespath
pip list
ansible-galaxy collection install -r collections/requirements.yaml
cd ../chapter-06
vagrant up
VBoxManage list runningvms
```

Grab the KUBECONFIG file
------------------------
```shell
vagrant ssh host01

mkdir .kube
chmod 700 .kube
sudo cp /etc/kubernetes/admin.conf .kube/config
sudo chmod vagrant:vagrant .kube/config
export KUBECONFIG=/home/vagrant/.kube/config
kubectl get nodes
exit

scp -P 2222 -i .vagrant/machines/host01/virtualbox/private_key vagrant@127.0.0.1:.kube/config ~/.kube
kubectl get nodes
kubectl get ns
```

Graceful Cluster Shutdown
-------------------------
```shell
vagrant ssh host01
sudo shutdown -h 1
exit

vagrant ssh host02
sudo shutdown -h 1
exit

vagrant ssh host03
sudo shutdown -h 1
exit

vagrant ssh host04
sudo shutdown -h 1
exit
```
