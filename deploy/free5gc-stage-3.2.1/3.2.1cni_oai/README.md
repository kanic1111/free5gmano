## Free5gc CN with oai-gnb B200  
> test enviroment is using kubernetes v1.23.6 and ubuntu20.04 kernel 5.4 with USRP B200

**before deploying the pod you has to install several software in order to let kubernetes get you SDR device**

**1. First you have to install the USRP software and make sure you can get the device on your pc.** 
> you can follow NI officel website installation guide here
[USRP Hardware Driver and USRP Manual](https://files.ettus.com/manual/page_install.html)

**after the installation you should be able to get your device info once you connect your device**

![image](https://cdn.discordapp.com/attachments/625953483576705024/1201785718205333544/B1ea6zLq6.png?ex=65cb152b&is=65b8a02b&hm=30c309a8bf15e3150f82e5e8925c27e86daa1c896f167edcd10f2b3ac3bac5b0&)


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
![image](https://github.com/kanic1111/free5gmano/assets/59955075/6a5f8233-8cb4-49ca-a43e-5caf1baa677f)


**after you can deploy free5gc and oai-gnb**

