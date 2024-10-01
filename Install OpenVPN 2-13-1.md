FIX LỖI Could not retrieve mirrorlist

sed -i.bak 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i.bak 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

CÀI TIỆN ÍCH VÀ TẮT FIREWALL CHO CENTOS 7

yum install -y epel-release vim curl wget net-tools telnet && yum update -y

systemctl disable --now firewalld

VÔ HIỆU HOÁ SELinux VĨNH VIỄN TRÊN CentOS 7

Sử dụng lệnh 

vi /etc/sysconfig/selinux

để chỉnh sửa tệp tin cấu hình của SELinux.

Tìm đến dòng SELINUX=enforcing chọn phím i để vào trạng thái chỉnh sửa tệp tin và chỉnh thành SELINUX=disabled.

Cấu hình sysctl

# sysctl net.ipv4.ip_forward

net.ipv4.ip_forward = 0

Thay đổi giá trị của parameter nói trên:

Cài thêm sysctl nếu lệnh bên dưới sudo sysctl -w net.ipv4.ip_forward=1 lỗi

sudo yum install procps-ng

# sudo sysctl -w net.ipv4.ip_forward=1

net.ipv4.ip_forward = 1

Vậy là chúng ta đã vô hiệu hóa tạm thời SELinux thành công. Điểm lợi thế của cách này là không cần phải khởi động lại VPS/Server của bạn.

CÀI WGET

yum install wget -y

CÀI PYTHON 3.8

sudo yum update -y

sudo yum groupinstall "Development Tools" -y

sudo yum install gcc openssl-devel bzip2-devel libffi-devel -y

cd /usr/src

sudo wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz

sudo tar xzf Python-3.8.0.tgz

cd Python-3.8.0

sudo ./configure --enable-optimizations

sudo make altinstall

python3.8 --version

sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.8 1

CÀI RH-PYTHON 3.8

sudo yum install centos-release-scl-rh -y

FIX LỖI Could not retrieve mirrorlist thêm lần nữa do cài python mới là 3.8

sed -i.bak 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i.bak 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

sudo yum install rh-python38 rh-python38-python-lxml rh-python38-python-pycparser rh-python38-python-idna rh-python38-python-cryptography -y

sudo yum install ./openvpn-as-latest-CentOS7.x86_64.rpm

sudo yum install ./openvpn-as-latest-CentOS7.x86_64.rpm --skip-broken

CÀI OPENVPN

yum -y install https://as-repository.openvpn.net/as-repo-centos7.rpm

yum -y install openvpn-as

wget https://openvpn.net/downloads/openvpn-as-latest-CentOS7.x86_64.rpm

wget https://openvpn.net/downloads/openvpn-as-bundled-clients-latest.rpm

yum install -y openvpn-as-*

systemctl start openvpnas

systemctl status openvpnas

firewall-cmd --zone=public --add-port=943/tcp --permanent

firewall-cmd --reload

CRACK OPENVPN

yum install -y python3 unzip zip

ls -alh /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.8.egg

mkdir -p /root/openvpnas/compile && cd $_

cp /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.8.egg .

unzip -q ./pyovpn-2.0-py3.8.egg

cd ./pyovpn/lic/

mv uprop.pyc uprop2.pyc

Chạy hết các lệnh bên dưới:

cat >uprop.py<<EOF

from pyovpn.lic import uprop2

old_figure = None

def new_figure(self, licdict):

    ret = old_figure(self, licdict)
    
    ret['concurrent_connections'] = 3979
    
    return ret

for x in dir(uprop2):

    if x[:2] == '__':
    
        continue
        
    if x == 'UsageProperties':
    
        exec('old_figure = uprop2.UsageProperties.figure')
        
        exec('uprop2.UsageProperties.figure = new_figure')
        
    exec('%s = uprop2.%s' % (x, x))
    
EOF

cat uprop.py

........................

Tiếp theo

python3 -O -m compileall uprop.py && mv __pycache__/uprop.*.pyc uprop.pyc

cd ../../

zip -rq pyovpn-2.0-py3.8.egg.cracked_2.12.0 ./pyovpn ./EGG-INFO ./common

mv /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.8.egg /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.8.egg.orginal

cp ./pyovpn-2.0-py3.8.egg.cracked_2.12.0 /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.8.egg

systemctl restart openvpnas

systemctl status openvpnas
