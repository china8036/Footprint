- [Metasplotable3 NOTE](#metasplotable3-note)
  - [install in VirtualBox](#install-in-virtualbox)
  - [Refer Links](#refer-links)

# Metasplotable3 NOTE

## install in VirtualBox

1. 安装[VirtualBox及其扩展包](https://www.virtualbox.org/wiki/Downloads)：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/31/49232922487e7380d278b0806141c253.jpg)

2. 安装[Vagrant](https://www.vagrantup.com/downloads.html)

    安装后重启，会自动将vagrant添加至系统环境变量。

3. 下载 metasploitable3 相关文件

    ```shell
    git clone https://github.com/rapid7/metasploitable3.git
    ```
    将以下内容替换 /metasploitable3-master/Vagrantfile 中的内容：
    ```
    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    # All Vagrant configuration is done below. The "2" in Vagrant.configure
    # configures the configuration version (we support older styles for
    # backwards compatibility). Please don't change it unless you know what
    # you're doing.
    Vagrant.configure("2") do |config|
      # The most common configuration options are documented and commented below.
      # For a complete reference, please see the online documentation at
      # https://docs.vagrantup.com.

      # Every Vagrant development environment requires a box. You can search for
      # boxes at https://atlas.hashicorp.com/search.
      config.vm.box = "Metasploitable3"
      #config.vm.box_version = "0.1.0"
      config.vm.provider :virtualbox do |vb|
          vb.name = "Metasploitable 3"
      end
      config.vm.hostname = "Metasploitable3"
      config.vm.communicator = "winrm"
      config.winrm.retry_limit = 60
      config.winrm.retry_delay = 10

      # The url from where the 'config.vm.box' box will be fetched if it
      # doesn't already exist on the user's system.
      # config.vm.box_url = "file:///D:/VPC/Metasploitable3-src/windows_2008_r2_virtualbox-iso.box"

      # Create a forwarded port mapping which allows access to a specific port
      # within the machine from a port on the host machine. In the example below,
      # accessing "localhost:8080" will access port 80 on the guest machine.
      # config.vm.network "forwarded_port", guest: 80, host: 8080

      # Create a private network, which allows host-only access to the machine
      # using a specific IP.
      # config.vm.network "private_network", ip: "192.168.33.10"

      # Create a public network, which generally matched to bridged network.
      # Bridged networks make the machine appear as another physical device on
      # your network.
      # config.vm.network "public_network"

      # Share an additional folder to the guest VM. The first argument is
      # the path on the host to the actual folder. The second argument is
      # the path on the guest to mount the folder. And the optional third
      # argument is a set of non-required options.
      # config.vm.synced_folder "../data", "/vagrant_data"

      # Provider-specific configuration so you can fine-tune various
      # backing providers for Vagrant. These expose provider-specific options.
      # Example for VirtualBox:
      #
      # config.vm.provider "virtualbox" do |vb|
      #   # Display the VirtualBox GUI when booting the machine
      #   vb.gui = true
      #
      #   # Customize the amount of memory on the VM:
      #   vb.memory = "1024"
      # end
      #
      # View the documentation for the provider you are using for more
      # information on available options.

      # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
      # such as FTP and Heroku are also available. See the documentation at
      # https://docs.vagrantup.com/v2/push/atlas.html for more information.
      # config.push.define "atlas" do |push|
      #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
      # end

      # Enable provisioning with a shell script. Additional provisioners such as
      # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
      # documentation for more information about their specific syntax and use.
      # config.vm.provision "shell", inline: <<-SHELL
      #   apt-get update
      #   apt-get install -y apache2
      # SHELL
    end

    ```

4. 下载 [metasploitable3-win2k8.box](https://app.vagrantup.com/jbarnett-r7/boxes/metasploitable3-win2k8)

5. 构建metasploitable3的vagrant环境

    在metasploitable3-win2k8.box目录下执行：
    ```powershell
    vagrant box add metasploitable3-win2k8.box --name Metasploitable3
    ```

    执行完毕后会在C:\user\xxx\.vagrant\boxes\Metasploitable3\0\中生成metasploitable3-win2k8-disk001.vmdk，至此，**可以直接复制该vmdk文件到其它地方，直接在virtualbox中使用现有虚拟硬盘新建虚拟机，即可使用**。

6. 使用vagrant添加并运行虚拟机

    在/metasploitable3-master/中执行：
    ```powershell
    vagrant plugin install vagrant-reload
    ```
    启动虚拟机
    ```
    vagrant up
    ```



## Refer Links

SAINTSEC教你玩转Metasploitable 3：http://www.freebuf.com/sectool/122626.html

