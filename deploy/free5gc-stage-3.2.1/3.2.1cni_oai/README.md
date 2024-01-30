## Free5gc CN with oai-gnb B200  
**test enviroment is using kubernetes v1.23.6 and ubuntu20.04 kernel 5.4 with USRP B200**

**before deploying the pod you has to install several software in order to let kubernetes get you SDR device**

**First you have to install the USRP software and make sure you can get the device info**

**you can follow NI officel website installation guide**
[USRP Hardware Driver and USRP Manual](https://files.ettus.com/manual/page_install.html)

**after the installation you should be able to get your device info once you connect your device**

![image](https://hackmd.io/_uploads/B1ea6zLq6.png)


**next for k8s to use your device you has to install the device plugin**

```bash=
git clone https://github.com/Gradiant/ettus-device-plugin.git
cd ettus-device-plugin
sudo make docker
# the whole process has to be done with root 
sudo kubectl apply -f ettus-daemonset.yaml
```

**the plugin will pull the USRP device driver and mount into the pod when requested(might took 1~2mins when pulling drivers)**

**you can check your node info to check if the k8s get your USRP device**

```bash=
kubectl get nodes -o go-template --template='{{range .items}}{{printf "%s %s\n" .metadata.name .status.allocatable}}{{end}}'
```
![image](https://hackmd.io/_uploads/BJKgRGLqT.png)

**after you can deploy free5gc and oai-gnb**

