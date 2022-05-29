# Cumulus and SONiC

![topo](./images/test_topo.drawio.png)


## .img を .vdi にコンバート

[SONiCの開発者が個人で立ち上げたサイト(？)](https://sonic.software/)からsonic-vs.img.gzを取得して，解凍．
その後，`.img`ファイルを`.vdi`に変換．

(※ `.img`ファイルを`.vdi`変換する方法として，VBoxManage.exe でコンバートする方法がよく紹介されていますがうまく起動しませんでした．)

```
> "C:\Program Files\qemu\qemu-img.exe" convert -O vdi sonic-vs.img sonic-vs.vdi
```
vdiファイルでVirtualboxを立ち上げる．

以下，設定例．

![VBox setting](./images/screenshot000194.JPG)

バージョン確認
```
$ show version
sudo: unable to resolve host sonic1: Name or service not known

SONiC Software Version: SONiC.master.102109-392899682
Distribution: Debian 11.3
Kernel: 5.10.0-12-2-amd64
Build commit: 392899682
Build date: Mon May 23 19:34:41 UTC 2022
Built by: AzDevOps@sonic-build-workers-001J3P

Platform: x86_64-kvm_x86_64-r0
HwSKU: Force10-S6000
ASIC: vs
ASIC Count: 1
Serial Number: N/A
Model Number: N/A
Hardware Revision: N/A
Uptime: 03:30:40 up 3 min,  1 user,  load average: 0.49, 0.75, 0.36
Date: Sun 29 May 2022 03:30:40

  (省略)
```



## Create Vagrant box

Virtual box guest additionsをインストール．
それぞれのバージョンは，VirtualboxやダウンロードしたSONiCに依存します．
```
sudo apt update -y
sudo apt install -y build-essential dkms wget
wget http://download.virtualbox.org/virtualbox/6.1.34/VBoxGuestAdditions_6.1.34.iso
sudo mkdir /media/VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions_6.1.34.iso /media/VBoxGuestAdditions
sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
rm VBoxGuestAdditions_6.1.34.iso
```

vagrant box の作成・追加
```
vagrant package --base "<VirtualBoxName>" --output sonic-vs.box
vagrant box add sonic-vs.box --name sonic-vs
```



## 参考
- [SONiCを少しかじってみた](https://debslink.hatenadiary.jp/entry/20210131/1612091391)
- [Vagrant, Creating a Base Box](https://www.vagrantup.com/docs/providers/virtualbox/boxes)
- [VIRTUALBOX から VAGRANT の BOX を作成する](http://vistylee.com/virtualbox-vagrant-box/)
