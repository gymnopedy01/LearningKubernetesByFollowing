# Vagrant 명령어
- vagrant init  : 프로비저닝을 해주는 예제 스크립트를 생성
- vagrant up : Vagrantfil을 읽어 들여 프로비저닝을 진행
- vagrant halt : 베이크런트에서 다루는 호스트들을 종료
- vagrant destroy : 베이그런트의 호스트들을 삭제
- vagrant ssh : 베이그런트의 호스트에 ssh 로 접속
- vargrant provision = 베이그런트의 호스트의 설정 변경을 적용

```sh
> cd c:\Hashi Corp
> vagrant init
> vagrant plugin install vagrant-vbguest
> notepad Vagrantfile
```

Vagrantfile
```text
config.vm.box = "centos/7"
config.vm.synced_folder ".", "/vagrant", disabled: true
```


```sh
vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'centos/7' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'centos/7'
    default: URL: https://vagrantcloud.com/centos/7
==> default: Adding box 'centos/7' (v2004.01) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/2004.01/providers/virtualbox.box
Download redirected to host: cloud.centos.org
    default:
    default: Calculating and comparing box checksum...
==> default: Successfully added box 'centos/7' (v2004.01) for 'virtualbox'!
==> default: Importing base box 'centos/7'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'centos/7' version '2004.01' is up to date...
==> default: Setting the name of the VM: HashiCorp_default_1666587702925_46199
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
[default] No Virtualbox Guest Additions installation found.
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.anigil.com
 * extras: mirror.anigil.com
 * updates: mirror.anigil.com
Resolving Dependencies
--> Running transaction check
---> Package centos-release.x86_64 0:7-8.2003.0.el7.centos will be updated
---> Package centos-release.x86_64 0:7-9.2009.1.el7.centos will be an update
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package             Arch        Version                     Repository    Size
================================================================================
Updating:
 centos-release      x86_64      7-9.2009.1.el7.centos       updates       27 k

Transaction Summary
================================================================================
Upgrade  1 Package

Total download size: 27 k
Downloading packages:
No Presto metadata available for updates
Public key for centos-release-7-9.2009.1.el7.centos.x86_64.rpm is not installed
warning: /var/cache/yum/x86_64/7/updates/packages/centos-release-7-9.2009.1.el7.centos.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-8.2003.0.el7.centos.x86_64 (@anaconda)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : centos-release-7-9.2009.1.el7.centos.x86_64                  1/2
  Cleanup    : centos-release-7-8.2003.0.el7.centos.x86_64                  2/2
  Verifying  : centos-release-7-9.2009.1.el7.centos.x86_64                  1/2
  Verifying  : centos-release-7-8.2003.0.el7.centos.x86_64                  2/2

Updated:
  centos-release.x86_64 0:7-9.2009.1.el7.centos

Complete!
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.anigil.com
 * extras: mirror.anigil.com
 * updates: mirror.anigil.com
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


# Vagrantfile 수정 & bootstrap.sh 생성
## Vagraintfile
1. 베이그런트에서 부르는 호스트 이름 작성
2. 버추얼박스에서 구분하는 호스트 이름작성
3. 가상머신의 호스트 이름을 변경
4. 호스트PC와 가상머신 간에는 공유 디렉터리는 사용하지 않음
5. 가상머신에서 인터넷으로 연결되는 IP 설정
6. 호스트PC의 포트를 IP 주소와 유사하게 변경
###
```text
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
	config.vm.define:"ansible-server" do |cfg|
		cfg.vm.box = "centos/7"
		cfg.vm.provider:virtualbox do |vb|
			vb.name="Ansible-Server(Udemy-Bloter)"
		end
		cfg.vm.host_name="ansible-server"
		cfg.vm.synced_folder "/", "vagrant", disabled: true
		cfg.vm.network "public_network", ip: "192.168.1.10"
		cfg.vm.network "forwarded_port", guest: 22, host: 19210, auto_correct: false, id: "ssh"
		cfg.vm.provision "shell", path: "bootstrap.sh"
	end
  
end
```
## bootstrap.sh
1. Yum을 통해서 EPEL을 설치
2. Yum을 통해서 ansible을 설치
### bootstrap.sh
```sh
#! /usr/bin/env bash

yum install epel-release -y
yum install ansible -y
```

### 실행
```sh
> vagrant up 
vagrant ssh ansible-server
```
