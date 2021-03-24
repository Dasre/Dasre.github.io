---
title: Vagrant2
date: 2019-11-11 14:22:15
tags:
---

### 其他虛擬機控制指令
`vagrant supend`
暫停虛擬機，可以使用`vagrant up`再啟動

`vagrant halt`
關閉虛擬機

`vagrant destroy`
刪除虛擬機，砍掉後可再透過`vagrant up`重新建立虛擬機

---

## Vagrantfile文件說明

Vagrantfile內文件是使用ruby攥寫的，在原始的Vagrantfile內，有許多的註解，也因此若要進行修改，是十分方便的。以下會將Vagrantfile內的註解去除，進行簡單的說明。

``` ruby
Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"

  # config.vm.box_check_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # config.vm.network "private_network", ip: "192.168.33.10"

  # config.vm.network "public_network"

  # config.vm.synced_folder "../data", "/vagrant_data"

  # config.vm.provider "virtualbox" do |vb|
  #   vb.gui = true
  #
  #   vb.memory = "1024"
  # end
  
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
```
基本上在|config|後面都是虛擬機的相關設定。
* Vagrant.configure("2")，這個2是指Vagrant版本的意思。
* config.vm.box = "centos/7"，這邊的box就是指映像欓是哪個。基本上box的命名都是 username/boxname，以centos/7為例，這個就是centos所提供的box，boxname就是7。
* config.vm.box_check_update = false，這邊是指說在使用vagrant up啟動虛擬機時，要不要檢查此box是否有新的版本。
* config.vm.network，這是指網路配置的部分。guest是指虛擬機的部分，host是指主機。
* config.vm.synced_folder，這是指主機和虛擬機共用資料夾的部分。後面第一個參數是本機目錄位置，第二個參數為虛擬機目錄位置。預設會將虛擬機的/vagrant資料夾和本機 Vagrantfile所在的資料夾共享。
* config.vm.provision，這是指虛擬機環境建立起來後會執行的事件，預設是安裝apache2套件。
