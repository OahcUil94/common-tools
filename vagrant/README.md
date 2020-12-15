# vagrant

vagrant是用于创建和部署虚拟化开发环境的工具

## vagrant box

box是开发人员或者组织打包好的镜像: [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

### vagrant box add

该命令用于添加一个镜像: `vagrant box add ubuntu/xenial64`, 如果本地没有`ubuntu/xenial64`镜像, 此时会自动进行镜像下载.

但是在国内的网络中, 通常会下载失败, 此时可下载离线包, 本地导入:

1. [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)找到自己要下载的镜像, 点进去, 并选择合适的版本, 复制`URL`, 例如: [https://app.vagrantup.com/centos/boxes/7/versions/2004.01](https://app.vagrantup.com/centos/boxes/7/versions/2004.01)
2. 链接拼接`/providers/虚拟化平台.box`, 这里例如: `/providers/virtualbox.box`, [https://app.vagrantup.com/centos/boxes/7/versions/2004.01/providers/virtualbox.box](https://app.vagrantup.com/centos/boxes/7/versions/2004.01/providers/virtualbox.box)
3. 执行添加命令: `vagrant box add centos/7 /d/.vagrant/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box`

```bash
$ vagrant box add centos/7 /d/.vagrant/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'centos/7' (v0) for provider:
    box: Unpacking necessary files from: file:///D://.vagrant/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box
    box:
==> box: Successfully added box 'centos/7' (v0) for 'virtualbox'!
```

> 以上是在windows系统下git bash中实验

### vagrant box list

查看本地已经存在的镜像列表:

```
$ vagrant box list
centos7         (virtualbox, 0)
centos8         (virtualbox, 0)
ubuntu/xenial64 (virtualbox, 0)
```

## vagrant up

根据配置文件Vagrantfile启动虚拟机, 启动的时候可指定虚拟化平台: `vagrant up --provider=virtualbox`

### Mac OSX执行vagrant up启动centos7时报错

```
Complete!
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.huaweicloud.com
 * extras: mirrors.huaweicloud.com
 * updates: mirrors.bfsu.edu.cn
No package kernel-devel-3.10.0-1127.el7.x86_64 available.
Error: Nothing to do
Unmounting Virtualbox Guest Additions ISO from: /mnt
umount: /mnt: not mounted
==> default: Checking for guest additions in VM...
    default: No guest additions were detected on the base box for this VM! Guest
    default: additions are required for forwarded ports, shared folders, host only
    default: networking, and more. If SSH fails on this machine, please install
    default: the guest additions and repackage the box to continue.
    default:
    default: This is not an error message; everything may continue to work properly,
    default: in which case you may ignore this message.
The following SSH command responded with a non-zero exit status.
Vagrant assumes that this means the command failed!

umount /mnt

Stdout from the command:



Stderr from the command:

umount: /mnt: not mounted
```

关键问题: `No package kernel-devel-3.10.0-1127.el7.x86_64 available.`, 解决办法:

```
vagrant up # 直到失败
vagrant ssh # 登录进虚拟机
sudo yum -y update kernel # 升级内核
exit # 退出虚拟机
vagrant reload --provision # 重启虚拟机并重新执行shell脚本
```

## vagrant其它命令

- `vagrant ssh` 登录虚拟机
- `vagrant halt` 关闭虚拟机
- `vagrant suspend` 挂起虚拟机
- `vagrant destroy [-f]` 销毁虚拟机, `-f`参数用于强制销毁
- `vagrant reload [--provision]` 重启虚拟机, `--provision`重启的时候再次执行shell命令

## vagrant启动虚拟机时重新执行Vagrantfile中的shell命令

- `vagrant reload --provision`
- `vagrant up --provision`
- `vagrant provision`

## vagrant配置文件

vagrant init命令可以初始化一个默认的Vagrantfile

```bash
$ vagrant init
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```


### 网络配置:Forwarded Ports端口转发

把主机的8080转发到虚拟机的80端口:
`config.vm.network "forwarded_port", guest: 80, host: 8080`

### 网络配置:Private Networks私有网络

把虚拟机作为一个私有网络, 虚拟机在一个网段里面, 宿主机可以通过ip地址进行访问, 但是外部的机器是无法访问到的:
`config.vm.network "private_network", ip: "192.168.50.4"`

如果不想设置私有网络固定的ip, 可以选择使用dhcp去动态的分配:
`config.vm.network "private_network", type: "dhcp"`

### 网络配置:Public Network公有网络

在局域网中的其他终端上可以访问到这台虚拟机: `config.vm.network "public_network"`

### 账号配置:配置root信息

```ruby
config.ssh.username = 'root'
config.ssh.password = 'vagrant'
config.ssh.insert_key = 'true'
```

- [https://stackoverflow.com/questions/25758737/vagrant-login-as-root-by-default](https://stackoverflow.com/questions/25758737/vagrant-login-as-root-by-default)

### 共享目录配置:使用virtualbox设置共享目录报错问题

挂载命令: `config.vm.synced_folder "~/code/go", "/home/vagrant/code/go", type: "nfs"`, 执行`vagrant reload`, 出现了如下的报错信息:

```bash
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

The error output from the command was:

mount: unknown filesystem type 'vboxsf'
```

需要安装vagrant-vbguest插件:
`vagrant plugin install vagrant-vbguest --plugin-clean-sources --plugin-source https://gems.ruby-china.com/`

### provision配置:默认是以root账户执行shell命令, 可切换成默认用户vagrant

指定privileged为false进行默认账户切换: `config.vm.provision "shell", path: "install.sh", privileged: false`

如果想要脚本在执行的过程中进行用户切换, 可在shell脚本中将相关命令放入`su - vagrant >>EOF ... EOF`:

```bash
echo '====switch default user===='
su - vagrant <<EOF
echo '====emacs config===='
mkdir -p ~/.emacs.d
echo "(setq backup-directory-alist (quote((\".\" . \"~/.emacs.d/.saves\"))))" >> ~/.emacs.d/init.el

echo '====goup install golang===='
export GOUP_UPDATE_ROOT=https://gh.api.99988866.xyz/https://github.com/owenthereal/goup/releases/latest/download;
export GOUP_GO_HOST=golang.google.cn;
curl -sSf https://cdn.jsdelivr.net/gh/owenthereal/goup@master/install.sh | sh -s -- '--skip-prompt'
EOF
```
