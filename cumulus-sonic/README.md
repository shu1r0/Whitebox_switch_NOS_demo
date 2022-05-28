# 


## Convert .img to .vdi

[sonic-vs.img.gz source](https://sonic.software/)

```
> "C:\Program Files\qemu\qemu-img.exe" convert -O vdi sonic-vs.img test.vdi
```


## Virtalbox to Vagrant box

install Virtual box guest additions
```
sudo apt update -y
sudo apt install -y linux-headers-4.19 build-essential dkms

sudo apt install -y wget
wget http://download.virtualbox.org/virtualbox/4.3.8/VBoxGuestAdditions_4.3.8.iso
sudo mkdir /media/VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions_4.3.8.iso /media/VBoxGuestAdditions
sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
```

create vagrant box

```
vagrant package --base "<VirtualBoxName>" --output sonic-vs.box

# vagrant box add sonic-vs.box --name sonic-vs
```

