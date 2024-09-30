Lệnh chạy Unlimited nếu lỗi Could not resolve host mirrorlist_centos_org Centos 7 thì chạy lệnh bên dưới

cd /tmp/ && yum install git -y && git clone https://github.com/income88/OpenVPN && cd OpenVPN/ && sed -i -e 's/\r$//' centos7.sh && chmod 755 centos7.sh && ./centos7.sh

..........................................
From first of July 2024 on CentOS 7, please switch to Vault archive repositories:

vi /etc/yum.repos.d/CentOS-Base.repo
copy/paste the following and mind your OS version. Change if needed. In this config is version 7.9.2009:

[base]
name=CentOS-$releasever - Base
baseurl=http://vault.centos.org/7.9.2009/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever - Updates
baseurl=http://vault.centos.org/7.9.2009/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-$releasever - Extras
baseurl=http://vault.centos.org/7.9.2009/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://vault.centos.org/7.9.2009/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7


If needed, do:
yum clean all

yum update

Kiểm tra phiên bản Centos
cat /etc/centos-release
