# Bài 1: Cài đặt máy chủ EC2/Linux

```bash
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1> Xin chao </h1></html>" > index.html
```

# Bài 2: Cách sử dụng vi editor trên AWS EC2 Linux

🏃🏼‍♀️ Lab document
1. Tạo file
vi hello.txt
bấm phím i trên bàn phím và bắt đầu soạn thảo nội dung file
2. Save file
2.1 save và thoát
Gõ tổ hợp phím ESC và phím :
sau đó gõ vào   wq! và bấm Enter
2.2 Chỉ lưu
Gõ tổ hợp phím ESC và phím :
sau đó gõ vào   w và bấm Enter
2.3 Thoát không lưu
Gõ tổ hợp phím ESC và phím :
sau đó gõ vào   q! và bấm Enter
2.4 Undo nội dung đã soạn thảo
Gõ tổ hợp phím ESC và phím U

# Bài 3: Cách lệnh cơ bản trên AWS EC2 Linux cd, ls, mkdir, cp, rm, history, pwd

🏃🏼‍♀️ Lab document
1. Tạo thư mục
mkidr hanoi
2. Xem thư mục
ls hanoi
3. copy toàn bộ file & thư mục tới một thư mục khác
cp -Rf hanoi/* haiphong
4. Kiểm tra đường dẫn thư mục
pwd
5. Xem lịch sử đã thao tác trên server
history
6. Xóa toàn bộ file và thư mục
rm -Rf hanoi

# Bài 4: Tạo user/group và phân quyền file và folder trên AWS EC2 Linux

🏃🏼‍♀️ Lab document

1.Thao tác với user

- Tạo user
useradd [tên user cần tạo]
ví dụ
useradd bao
- Đặt mật khẩu cho user
passwd  [tên user cần tạo]
ví dụ: passwd bao
- Xóa user
userdel　[tên user cần tạo]
ví dụ: userdel　bao

2. Thao tác với nhóm

- Tạo nhóm
groupadd ketoan

- Thêm user vào group
usermod a -G  groupname username
usermod -a -G ketoan bao

- Kiểm tra xem user đã nằm trong group hay chưa
id bao

3. File and folder permission

- Kiểm tra permisson hiện tại
ls -la

d rwx rwx ---  3 root     ketoan    15 Mar 16 07:05 data-ketoan
-rw-r--r--  1 root     root       4 Mar 16 07:43 hellotxt

- Thứ tự phân quyền user/group/other
d: là thư mục
r = read permission
w = write permission
x = execute permission
- = no permission
https://www.guru99.com/file-permissio...

- Set permission cho file and folder

Bản phân quyền
No.   Permission Type   Symbol
0  No Permission   ---
1  Execute     --x
2  Write     -w-
3  Execute + Write   -wx
4  Read     r--
5  Read + Execute   r-x
6  Read + Write          rw-
7  Read + Write +Execute rwx

Lệnh set quyền cho file và folder
chmod [quyền] [tên file hoặc thư mục]
ví dụ: chmod -Rf 777 ketoan

- Lệnh gán quyền cho user và group
chown [tên user]: [tên nhòm] [tên file hoặc thư mục]
ví dụ: chown -Rf bao:bao ketoan

# Bài 5: Tạo SSH KEY trên AWS EC2 Linux

🏃🏼‍♀️ Lab document
Step1: Tạo user
Step2: Tạo Public key & Private key
$ ssh-keygen -t rsa
$ cd /home/yuki/.ssh
$ mv id_rsa.pub authorized_keys
$ chmod 600 authorized_keys
Step3: Đăng nhập thử
Step4: visudo

# Bài 6: Tạo web server Apache trên AWS EC2 Linux

🏃🏼‍♀️ Lab document
1. Cấu hình DNS
2. Cài đặt Apache
3. Mở firewall port 80
4. Cấu hình tên miền

NameVirtualHost *:80

{VirtualHost *:80}
    ServerAdmin info@coderhanoi.com
    DocumentRoot /var/www/html/tinhocthatladongian
    ServerName tinhocthatladongian.coderhanoi.com
    {Directory "/var/www/html/tinhocthatladongian"}      
   Order deny,allow
           Allow from all
           AllowOverride All
          Require all granted
   {/Directory}
{/VirtualHost}

# Bài 7: Cài SSL miễn phí cho web server Apache trên AWS EC2 Linux

🏃🏼‍♀️ Lab document
Step1: Cài đặt mod ssl cho Apache
yum -y install mod_ssl

Step2:Cài đặt Cài đặt openssl
yum install openssl

Step3: Tạo private key
mkdir -p /etc/httpd/conf/tinhocthatladongian-ssl
cd /etc/httpd/conf/tinhocthatladongian-ssl
openssl genrsa } server.key

Step4: Tiếp theo, tạo file CSR, file CSR này được sử dụng khi cơ quan cấp chứng chỉ cấp chứng chỉ cho máy chủ
openssl req -new -key server.key } server.csr

Step5: Gửi file server.csr ở trên cho nhà cung cấp SSL(nơi bạn muốn mua SSL) bạn sẽ nhận được các file cerfiticate
ví dụn mình dùng sslforfree để thực hành bài này

Step6: Copy các file cerfiticate nhận được vào thư mục /etc/httpd/conf/tinhocthatladongian-ssl và change quyền cho các file này
chown root:root  ca_bundle.crt
chown root:root  certificate.crt
chmod 600  ca_bundle.crt chown 600  certificate.crt


Step7: Cấu hình apache
{VirtualHost _default_:443}
    SSLEngine On
    ServerAdmin info@coderhanoi.com
    DocumentRoot "/var/www/html/tinhocthatladongian/"
    ServerName coderhanoi.com
    SSLCertificateFile "/etc/httpd/conf/tinhocthatladongian-ssl/certificate.crt"
    SSLCertificateChainFile "/etc/httpd/conf/tinhocthatladongian-ssl/ca_bundle.crt"
    SSLCertificateKeyFile "/etc/httpd/conf/tinhocthatladongian-ssl/server.key"
    {Directory "/var/www/html/tinhocthatladongian/"}        
 Order deny,allow        
 Allow from all        
 AllowOverride All        
 Require all granted
   {/Directory}
{/VirtualHost}

Step:  Khởi động lại apache
service restart httpd

Step9:  Mở cổng 443 cho ssl trên firewall

# Bài 8: Cài đặt FTP Server trên AWS EC2 Linux


🏃🏼‍♀️ Lab document
1. Cài FTP
yum -y install vsftpd

2. Thêm các dòng lệnh sau vào /etc/vsftpd/vsftpd.conf
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=NO
pasv_enable=YES
pasv_address=[IP của máy chủ]
pasv_min_port=60001
pasv_max_port=60010
force_dot_files=YES
use_localtime=YES
allow_writeable_chroot=YES
listen=YES
listen_ipv6=NO
pasv_addr_resolve=NO

3. Bỏ các comment sau trong /etc/vsftpd/vsftpd.conf
Bỏ comment
chroot_local_user=NO
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list

Thêm dòng
user_config_dir=/etc/vsftpd/user_conf

4. edit file sau và thêm vào user ftp-user
vi /etc/vsftpd/chroot_list
bao

5. Tạo thư mục 
mkdir /etc/vsftpd/user_conf

6.Tạo file /etc/vsftpd/user_conf/bao
vi /etc/vsftpd/user_conf/bao
local_root=/var/www/bao

7. Tạo user và set quyền cho user FTP
Tạo user
useradd bao
passwd bao

Set quyền bao cho thư mục bao
chown bao /var/www/bao
chmod 755 /var/www/bao

8. Mởi port 21, 60001-60010 trên Firewall
9. Test kết nối

# Bài 9: Cài database MySQL phân quyền User trên AWS EC2 Linux

🏃🏼‍♀️ Lab document
1. Kiểm tra xem mariabdb có cài trên EC2/Linux hay không
yum list installed | grep mariadb

2. Kiểm tra xem MySQL đã được cài đặt hay chưa
yum list installed | grep mysql

3. Thêm Repository mysql8.0 vào thư viện yum(trong Repository 8 này thì bao gồm cả version 5.7)
yum localinstall https://dev.mysql.com/get/mysql80-com... -y

4. Mình sẽ cài mysql 5.7 nên mình sẽ disable mysql 5.8 đi bằng lệnh
yum-config-manager --disable mysql80-community

5. Enable mysql 5.7 lên bằng lệnh
yum-config-manager --enable mysql57-community

6. Confirm thông tin version
yum info mysql-community-server

7. Cài đặt
yum install mysql-community-server -y

8. Kiểm tra version
mysqld --version 

9. Khởi động
systemctl start mysqld.service

10. Auto start-up
systemctl enable mysqld.service

11. Xem status trạng thái xem đã chạy hay chưa
systemctl status mysqld.service

12. Xem mật khẩu root mặc định
cat /var/log/mysqld.log | grep password

13. Thiết lập lần đầu cho MySQL
mysql_secure_installation

14. Xem charset hiện tại
mysql -uroot -p
show global variables like 'character%';

15. Set default charset cho MySQL
vi /etc/my.cnf
{mysqld}
character_set_server=utf8

{client}
default-character-set=utf8


16. Tạo database
create database tinhocthatladongian character set UTF8 collate utf8_bin;

17. Tạo local user
CREATE USER 'bao'@'localhost' IDENTIFIED BY 'Asdf123@!';

 Gán quyền cho phép quản lý database tinhocthatladongian
mysql} GRANT ALL PRIVILEGES ON tinhocthatladongian.* TO 'bao'@'localhost';

18. Tạo remote user
CREATE USER 'bao'@'%' IDENTIFIED BY 'Asdf123@!';

Gán quyền cho phép quản lý database tinhocthatladongian
GRANT ALL PRIVILEGES ON tinhocthatladongian.* TO 'bao'@'%';

19. Mở firewall port 3306
Kết nối thử

# Bài 10: AWS IAM tạo người dùng quản lý AWS

# Bài 11: AWS S3 dịch vụ quản lý files

🏃🏼‍♀️ Lab document
aws s3 ls  hiển thị list bucket
aws s3 ls s3://{buckname}/{path} dir bucket
aws s3 mb s3://{buckname}  tạo bucket
aws s3 rb s3://{buckname}   xóa bucket trường hợp bucket có data
aws s3 rb s3://{buckname} --force xóa bucket trường hợp bucket không có dữ liệu
aws s3 sync {folder path} s3://{buckname}/{path}   Đồng bộ lên bucket trường hợp này chỉ thêm và update mà không xóa
aws s3 sync {folder path} s3://{buckname}/{path} --delete Đồng bộ với thư mục local sẽ xóa file nếu không tồn tại ở local
aws s3 cp {file path} s3://{buckname}/{path}    copy file từ local lên s3
aws s3 mv {file path} s3://{buckname}/{path}    move file từ local lên s3
aws s3 rm s3://{buckname}/{file path} xóa file
aws s3 rm s3://{buckname}/{folder path} --recursive  xóa thư mụcaws s3 ls  hiển thị list bucket
aws s3 ls s3://{buckname}/{path} dir bucket
aws s3 mb s3://{buckname}  tạo bucket
aws s3 rb s3://{buckname}   xóa bucket trường hợp bucket có data
aws s3 rb s3://{buckname} --force xóa bucket trường hợp bucket không có dữ liệu
aws s3 sync {folder path} s3://{buckname}/{path}   Đồng bộ lên bucket trường hợp này chỉ thêm và update mà không xóa
aws s3 sync {folder path} s3://{buckname}/{path} --delete Đồng bộ với thư mục local sẽ xóa file nếu không tồn tại ở local
aws s3 cp {file path} s3://{buckname}/{path}    copy file từ local lên s3
aws s3 mv {file path} s3://{buckname}/{path}    move file từ local lên s3
aws s3 rm s3://{buckname}/{file path} xóa file
aws s3 rm s3://{buckname}/{folder path} --recursive  xóa thư mục

# Bài 12: AWS RDB Cơ sở dữ liệu Cài EC2 và wordpress để test RDS

🏃🏼‍♀️ Lab document
1. Tạo DB MySQL RDS
2. Tạo kết nối tới DB từ xa
3. Cài EC2 và wordpress để test RDS
#!/bin/bash
yum install httpd php php-mysql -y
cd /var/www/html
wget https://wordpress.org/wordpress-5.1.1...
tar -xzf wordpress-5.1.1.tar.gz
cp -r wordpress/* /var/www/html/
rm -rf wordpress
rm -rf wordpress-5.1.1.tar.gz
chmod -R 755 wp-content
chown -R apache:apache wp-content
service httpd start
chkconfig httpd on

# Bài 13: Elastic IP Address cấu hình IP tĩnh trên AWS/EC2

# Bài 14: Cài đặt web server Nginx Trên AWS/EC2

🏃🏼‍♀️ Lab document
1. Cấu hình Virtual host http thường
server {
    listen        80;
    server_name   tinhocthatladongian.coderhanoi.com;
    access_log    /var/log/nginx/tinhocthatladongian.coderhanoi.com.access.log main;
    error_log    /var/log/nginx/tinhocthatladongian.coderhanoi.com.error.log;
    root /var/www/tinhocthatladongian.coderhanoi.com;
    index index.php;
    location ~ \.php$ {
        root           /var/www/tinhocthatladongian.coderhanoi.com;        
  fastcgi_pass   unix:/run/php-fpm/www.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}

2. Cấu hình Virtual host ssl https
 server {
    listen 80;
 listen 443;
 
 ssl on;
 ssl_certificate      /path/tinhocthatladongian.coderhanoi.com.crt;　
 ssl_certificate_key  /path/tinhocthatladongian.coderhanoi.com.key;
 
    server_name   tinhocthatladongian.coderhanoi.com;
    access_log    /var/log/nginx/tinhocthatladongian.coderhanoi.com.access.log main;
    error_log    /var/log/nginx/tinhocthatladongian.coderhanoi.com.error.log;
    root /var/www/tinhocthatladongian.coderhanoi.com;
    index index.php;
    location ~ \.php$ {
        root           /var/www/tinhocthatladongian.coderhanoi.com;        
  fastcgi_pass   unix:/run/php-fpm/www.sock;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}

# Bài 15: Thiết lập AWS Cloudfront CDN trên AWS/EC2

# Bài 16: AWS Route53 đăng ký tên miền và cấu hình DNS trên AWS Cloud

🏃🏼‍♀️ Lab document
#!/bin/bash
yum update -y
yum install httpd -y
chkconfig httpd on
service httpd start
cd /var/www/html
echo "Hello Tin Hoc That La Don Gian" } index.html

# Bài 17: DynamonDB NoSQL trên AWS Cloud

🏃🏼‍♀️ Lab document
Command Line(CLI)
- Tạo bảng
aws dynamodb create-table --table-name 'hocsinh' \
--attribute-definitions '[{"AttributeName":"mahocsinh","AttributeType": "S"}]' \
--key-schema '[{"AttributeName":"mahocsinh","KeyType": "HASH"}]' \
--provisioned-throughput '{"ReadCapacityUnits": 5,"WriteCapacityUnits": 5}'

- Insert data vào bảng
aws dynamodb put-item --table-name hocsinh --item '{ "mahocsinh": { "S": "a001" }, "hoten": { "S": "Nguyen QUoc Bao" }, "ngaysinh": { "S": "1989-01-23"} }'
aws dynamodb put-item --table-name hocsinh --item '{ "mahocsinh": { "S": "a002" }, "hoten": { "S": "Nguyen Van Doanh" }, "ngaysinh": { "S": "2000-01-11"} }'

- Lấy data bằng Scan
aws dynamodb scan --table-name hocsinh

- Lấy data theo điều kiện
aws dynamodb get-item --table-name hocsinh  --key '{ "mahocsinh": {"S": "a001" }  }'

- Xoa record theo điều kiện
aws dynamodb delete-item --table-name hocsinh  --key '{"mahocsinh": {"S": "a001"}}'

- Đếm số records
aws dynamodb scan --table-name hocsinh --select "COUNT"

- Xóa bảng
aws dynamodb delete-table --table-name hocsinh

- List bảng đang có
aws dynamodb list-tables

- Xem thuộc tính của bảng
aws dynamodb describe-table --table-name hocsinh

# Bài 19: Lập trình Lambda 2 /DynamoDB trên AWS Cloud

# Bài 20: Lập trình Lambda Serverless 3 / S3 trên AWS Cloud

# Bài 21: AWS API Gateway trên AWS Cloud

# Bài 22: AWS Email Marketing SES trên AWS Cloud

# Bài 23: AWS CodeCommit / Devops quản lý source code bằng GIT trên AWS Cloud

🏃🏼‍♀️ Lab document
1. Tạo User để sử dụng cho git IAM
2. Tạo repository 
3. Cài AWS CLI trên windows
     aws configure
4. Cài và cấu hình Git client trên Windows
git config --global credential.helper "!aws codecommit credential-helper $@"
git config --global credential.UseHttpPath true
5. Các thao tác với GIT
Clone
Commit
Push

# Bài 24: (CI/CD) Pipeline / Devops tự động deploy code PHP, Nodejs, Python trên AWS

🏃🏼‍♀️ Lab document
1. Tạo CodeCommit
http://tinhocthatladongian.com/downlo...
2. Tạo Role cho EC2 có quyền access vào S3
3. Tạo Role CodeDeploy
Cài EC2 để deploy code
#!/bin/bash
yum -y update
yum install -y ruby
yum install -y aws-cli
cd /home/ec2-user
aws s3 cp s3://aws-codedeploy-us-east-2/latest/install . --region us-east-2
chmod +x ./install
./install auto
4. Tạo CodeDeploy
5. Tạo CodePipeline
Test deploy tự động

# Bài 25: Docker 1 / DevOps Docker là gì? cài docker và docker-compose trên EC2/AWS và Windows

🏃🏼‍♀️ Lab document
Cài docker và docker-compose trên EC2/AWS
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user

- Cài docker-compose
sudo curl -L "https://github.com/docker/compose/rel... -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose


# Bài 26: Docker 2/DevOps triển khai tạo Image, Container&Docker Hub cho khách hàng và đội lập trình

🏃🏼‍♀️ Lab document
Các lệnh với image
- Tìm image để cài đặt
docker search centos
- List các image đang có
docker image ls
- Xóa image
sudo docker rmi [Image ID]
Chú ý: Các image mà có container đang chạy thì sẽ không xóa được mà phải xóa container liên quan tới nó trước
- Pull image về
docker pull centos:7

Làm việc với container

- Chạy 1 container từ image
 docker run --privileged -d -p 80:80 [Image Name] /sbin/init
docker run --privileged -d -p 80:80 centos:7 /sbin/init

- Vào container để chạy lệnh
docker exec -it [Container ID] /bin/bash 

- Cài đặt apache
yum -y install httpd
systemctl start httpd
systemctl enable httpd

echo "Hello Tin Hoc That La Don Gian" } /var/www/html/index.html

- Thoát ra khỏi container
exit

- Tạo image để triển khai cho máy khác
docker commit -m "Comment" -a "Tác giả"  [Container ID] [Image Name]
docker commit -m "Centos Project01" -a "Nguyen Quoc Bao" d452f1a1b69d tinhocthatladongian/project01:v1

- Đăng nhập vào docker/hub
docker login

- Đưa image lên docker hup để mọi người cùng sử dụng
docker push [Tên image]
docker push tinhocthatladongian/project01:v1
 
- Check các container đang chạy
sudo docker ps -a

- Xem trạng thái container
docker container ls -a

- Xóa containner
sudo docker rm [Container ID]

- Stop container
docker container stop [Container ID]

- Restart container
docker container restart [Container ID]

- Pause container
docker container pause  [Container ID]

- Truy cập vào các container đang chạy
docker container attach [Container ID]


- Lệnh stop toàn bộ container
docker stop $(docker ps -a -q)

- Lệnh xóa toàn bộ container
docker rm $(docker ps -a -q)

- Lệnh xóa toàn bộ image
docker rmi -f $(docker images -a -q)

# Bài 27:DevOps Docker 3 / Dockerfile đây chính là lý do tăng lương, tăng vị trí cho các bạn developer

🏃🏼‍♀️ Lab document
1. Tạo Dockerfile

# Get base image
FROM centos:7

# Author
MAINTAINER "Nguyen Quoc Bao"

#Cai dat apache
RUN yum update -y
RUN yum install -y sudo
RUN yum install -y epel-release
RUN yum install -y http://rpms.famillecollet.com/enterpr...
RUN yum clean all
RUN yum -y install wget
RUN yum -y install httpd
RUN yum -y install --enablerepo=remi,remi-php71 php php-devel php-mbstring php-pdo php-gd php-xml php-mcrypt php-pgsql
 
#Thiet lap thu muc lam viec 
WORKDIR /var/www/html

#copy code tu thu muc code vao image
ADD ./code /var/www/html

# start httpd
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]

EXPOSE 80


2. Build image
docker build -t httpd_sample .

3. Tạo và chạy container
docker run -d -p 80:80 httpd_sample

# Bài 28: DevOps Docker 4 / Docker-compose docker nâng cao

🏃🏼‍♀️ Lab document
Script
http://tinhocthatladongian.com/downlo...

Sample
https://github.com/dockersamples/exam...

Dockerfile là gì
Là file config dùng để build các image mới dựa trên một image có sẵn

Các lệnh trong Dockerfile
FROM: Lấy 1 image trên docker hub
LABEL: Thông tin người bảo trì dockerfile
ENV: thiết lập một biến môi trường
RUN: Chỉ chạy khi build image, được sử dụng để cài đặt các package vào container
COPY: Sao chép các file và thư mục vào container
ADD: Sao chép các file và thư mục vào container
CMD: trong 1 Dockerfile chỉ có 1 CMD, chỉ chạy khi thực hiện lện docker run để tạo ra 1 container
WORKDIR: Thiết lập thư mục làm việc cho các chỉ thị khác như: RUN, CMD, ENTRYPOINT, COPY, ADD,…
ARG: Định nghĩa giá trị biến được dùng trong lúc build image
ENTRYPOINT: cung cấp lệnh và đối số cho một container thực thi
EXPOSE: khai báo port lắng nghe của image
VOLUME: tạo một điểm gắn thư mục để truy cập và lưu trữ data.

# Bài 29: DevOps Docker 5 / Docker Volume, Networks docker nâng cao

🏃🏼‍♀️ Lab document
http://tinhocthatladongian.com/downlo...

1. Docker volume
Là một nơi lưu trữ data nằm ngoài container, mục đích không làm mất data khi xóa container

Sử dụng Volume khi nào 
- Khi chia sẻ dữ liệu giữa nhiều container đang chạy.
- Lưu dữ liệu tới một server remote hoặc cloud.
- Khi cần backup, restore hoặc migrate dữ liệu từ Docker Host này sang Docker Host khác.

Lệnh liên quan tới volume
```bash
# docker volume create        Tạo mạng mới
# docker volume inspect       Xem chi tiết mạng
# docker volume ls             Hiển thị những mạng đang có
# docker volume rm             Xóa volume
# docker volume prune         Xóa toàn bộ volume
```
2. Docker networks
Để kết nối các container trong cùng mạng hoặc khác mạng với nhau.

Các câu lệnh thao tác với mạng
```bash
# docker network create        Tạo mạng mới
# docker network inspect       Xem chi tiết mạng
# docker network ls             Hiển thị những mạng đang có
# docker network rm            Xóa mạng
# docker network prune         Xóa đồng loạt các mạng không sử dụng
# docker network connect       Tạo kết nối mạng
# docker network disconnect    Ngắt kết nối mạng
```
# Bài 30: AWS ECS ECR / DevOps triển khai docker image lên AWS ECS

🏃🏼‍♀️ Lab document
http://tinhocthatladongian.com/downlo...
1. Tạo IAM user
AmazonEC2ContainerRegistryFullAccess
AmazonECS_FullAccess
2. Cài EC2
3. Cấu hình AWS CLI
aws configure
4. Tạo docker image
AMAZON ECR
5. Tạo repository
6. Push Image lên ECR
7. Tạo ECS Task, Cluster
8. Chạy và kiểm tra thành quả

# Bài 31: AWS VPC 1 Thiết kế mạng riêng bảo mật cho doanh nghiệp trên AWS/Cloud



# Bài 32: AWS VPC 2 Nat instance & Nat gateway Thiết kế mạng riêng bảo mật cho doanh nghiệp trên AWS

🏃🏼‍♀️ Lab document
1. Tạo Security Group cho DB
2. Làm thế nào để truy cập vào EC2 trong private subnet
2. Làm thế nào để cài đặt package trong những service nằm trong private subnet
3. cài đặt Nat instance
4. Cài đặt Nat gateway

# Bài 33: AWS VPC 3 ACL Access Control List Quản lý traffic vào ra services cho doanh nghiệp trên AWS

# Bài 34: AWS VPC 4 Bastion Host Bảo vệ mạng dịch vụ cho doanh nghiệp trên AWS Cloud bằng Bastion host

🏃🏼‍♀️ Lab document
Bastion host là cái gì?
Khi kết nối đến dịch vụ cloud trực tiếp thì về cơ bản là không tốt và dễ bị tấn công, chính vì vậy để kết nối đến các dịch vụ trong VPS của mình chúng ta sẽ thông qua 1 server chuyên dụng khác. Những server như thế được gọi là Bastion Server. Những server này thường tắt khi không sử dụng nên hoàn toàn có thể an tâm người bên ngoài xâm nhập là rất khó, nên sẽ làm tăng an toàn bảo mật lên.

Internet Gateway =＞ Route Table =＞ Network ACL =＞ Security Group =＞ Bastion host =＞ Service 

Thực hành
Tạo customize VPC
Tạo WebServer và DB Server trên trong lớp customize VPC
Tạo Bastion host
Kết nối và test thử

# Bài 35: AWS VPC 4 Endpoint Tối ưu hóa chi phí sử dụng AWS Cloud cho doanh nghiệp bằng VPC Endpoint

🏃🏼‍♀️ Lab document
1. VPC Endpoint là cái gì?
VPC Endpoint đóng vai trò là cầu nối, và là kết nối riêng biệt để các instance trong VPC có thể giao tiếp với các dịch vụ như S3, Dynamodb ... khác trên AWS mà không cần đi qua Internet, NAT, VPN, hay AWS Direct Connect

Tất cả các traffic đi từ VPC đến AWS Service đều không đi ra khỏi AWS network.

2. Các dịch vụ support Endpoint
https://docs.aws.amazon.com/vpc/lates...

Lợi ích khi sử dụng VPC Endpoint
Giảm chi phí sử dụng AWS vì chỉ kết nối trong AWS Cloud mà ko ra ngoài internet
Vì không ra ngoài internet nên đảm bảo tính bảo mật

3.  Lab Steps
Tạo customize VPC
Tạo EC2 public & private
Tạo IAM Role cho EC2 có thể access tới S3
Test kết nối tới S3
Xem danh sách s3
aws s3 ls --region=eu-west-3

# Bài 36: AWS Elastic Load Balancing 1/Giới thiệu về cân bằng tải giải pháp tuyệt vời cho doanh nghiệp

🏃🏼‍♀️ Lab document
1. Cân bằng tải là gì
Elastic Load Balancing tự động phân phối lưu lượng truy cập ứng dụng đến nhiều mục tiêu(target),  chẳng hạn như instance của Amazon EC2/Web Server.  Nó có thể xử lý tải khác nhau của lưu lượng truy cập ứng dụng của bạn trong một Availability Zones(Data center) duy nhất hoặc qua nhiều Availability Zones(Data center) .

2. Lợi ích
Triển khai và mở rộng quy mô nhanh
Đảm bảo dịch vụ được hoạt động liên tục khi 1 service(ví dụ 1 web chết)
Đảm bảo tốc đột truy cập đến dịch vụ

3. Các loại cân bằng tải
Application Load Balancer
Phù hợp nhất cho cân bằng tải lưu lượng HTTP và HTTPS, hoạt động ở Layer 7
https://docs.aws.amazon.com/elasticlo...

Network Load Balancer
Phù hợp nhất cho việc cân bằng tải lưu lượng TCP, nơi đòi hỏi hiệu năng cực cao. Hoạt động ở mức kết nối (Layer 4) nó có khả năng xử lý hàng triệu yêu cầu mỗi giây đồng thời duy trì độ trễ cực thấp
https://docs.aws.amazon.com/elasticlo...

Classic Load Balancer
Cung cấp cân bằng tải cơ bản trên nhiều trường hợp Amazon EC2 và hoạt động ở cả cấp yêu cầu và cấp kết nối. Bộ cân bằng tải cổ điển dành cho các ứng dụng được xây dựng trong mạng EC2-Classic
https://docs.aws.amazon.com/elasticlo...

# Bài 37:AWS Elastic Load Balancing 2/Application Load Balancer Lab Cân bằng tải cho hệ thống lớn

# Bài 38:AWS Elastic Load Balancing 3/ Network Load Balancer Cân bằng tải cho hệ thống lớn

# Bài 39: AWS Elastic Load Balancing 4/ Classic Load Balancer Giải pháp cân bằng tải cho hệ thống lớn
Giải pháp cân bằng tải tuyệt vời ông mặt trời cho các dự án chạy với quy mô lớn nhiều người dùng

# Bài 40: AWS Elastic Load Balancing 5/ Lập trình 1 giải pháp cho session với ElastiCache/memcached

🏃🏼‍♀️ Lab document
http://tinhocthatladongian.com/downlo...
1. Tạo ElastiCache/memcached
2. Tạo Load Balancer
3. Viết code php để test session

Bài 41: AWS Elastic Load Balancing 6/ Lập trình 2 session với RDS/MySQL


🏃🏼‍♀️ Lab document
1. Tạo RDS/MySQL
2. Tạo Load Balancer
3. Viết code php để test session

# Bài 46: CI/CD How to install Jenkins on AWS EC2 Linux

🏃🏼‍♀️ Lab document
1. Tạo EC2
2. Java installation
$ yum install -y java-1.8.0-openjdk-devel.x86_64
$ alternatives --config java 
$ java -version
(yum install fontconfig java-11-openjdk )

3. Jenkins install
$ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
$ yum install -y jenkins

4. Start Jenkins
$ systemctl start jenkins

5. Setting password
$ less /var/lib/jenkins/secrets/initialAdminPassword

Vieets code chinh xung hclk la 24 mghz
(((((Không chỉ xem những video hiện tại của mình mà hãy mở rộng mình ra )))))

>>> Đã sửa đổi và kiếm được code 

# Bài 47: Continuous Integration for PHP projects using Jenkins/GitHub

1 Web server installtion
#!/bin/bash
yum update -y
yum install httpd -y 
amazon-linux-extras install epel 
yum install epel-release
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum install -y php70 php70-php php70-php-fpm php70-php-pecl-memcached php70-php-mysqlnd php70-php-xml
ln -s /usr/bin/php70 /usr/bin/php70
yum install git -y 
service httpd start
chkconfig httpd on

Câu lệnh trên dùng để cài đặt các gói cần thiết để sử dụng PHP trên hệ điều hành CentOS. Cụ thể:

php70: phiên bản PHP 7.0 trên CentOS
php70-php: các module và thư viện cho PHP 7.0
php70-php-fpm: FastCGI Process Manager cho PHP 7.0
php70-php-pecl-memcached: gói mở rộng Memcached cho PHP 7.0
php70-php-mysqlnd: MySQL Native Driver cho PHP 7.0
php70-php-xml: các thư viện XML cho PHP 7.0
Với -y được sử dụng để đồng ý với tất cả các yêu cầu của gói cài đặt, do đó quá trình cài đặt sẽ không yêu cầu sự tương tác của người dùng để xác nhận các yêu cầu này.

>>> ((( Vẫn là nếu tận dụng được AWS document thì tận dụng , lát nữa làm theo document xem đc k , nếu vậy đỡ tốn thời gian để fix lỗi , chỉ cần xem video và hiểu nguyên tắc là được rồi )))

( Đổi thành lệnh trong ubuntu )
#!/bin/bash
sudo apt update
sudo apt install -y apache2
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:ondrej/php
sudo apt update
sudo apt install -y php7.0 libapache2-mod-php7.0 php7.0-fpm php7.0-mysql php7.0-memcached php7.0-xml
sudo a2enmod php7.0
sudo service apache2 restart
sudo apt install -y git


2. Configure Apache
useradd www-user

cd /var/www/html/
mkdir tinhocthatladongian
chown -Rf www-user:apache tinhocthatladongian
vi /etc/httpd/conf/httpd.conf

Listen 81
NameVirtualHost *:81
<VirtualHost *:81>
   DocumentRoot /var/www/html/tinhocthatladongian/
	<Directory "/var/www/html/tinhocthatladongian">
		Order deny,allow
		Allow from all
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>

service httpd restart

http://18.182.38.76:81/

3. Creating ssh key
sudo su - www-user
ssh-keygen -t rsa
cd /home/www-user/.ssh/
mv id_rsa.pub authorized_keys
( đổi quyền liền ngay khi tạo )
chown 600 authorized_keys

4. Copy private key to Jenkins server
cd /home
mkdir jenkins
vi web-server.pem
chown -Rf jenkins:root
chown 400 web-server.pem
ssh www-user@/ip/ -i /home/jenkins/web-server.pem

( vi chua cau hinh eslatic ip nen sau khi start lai , ip bi thay doi )

////////////////////////////////////////////
rm -Rf tinhocthatladongian/
( xóa thư mục có file )


- Đổi lại ip cho slave 1 ( xem đã được chưa ), đổi thì phải netplan apply
- vim bach >> source
- đổi minh chau >> tanhoang
- phần test có dấu -
- phần source có dâu - phải dính liền
- 

// stop-yarn.sh
// stop-dfs.sh
////////////////////////////////////////////


################################################
// thêm thư mục vào
git status
git add 
git commit -m""
git push
################################################

5. Github configuration
Webhook
http://[Jenkin URL]:8080/github-webhook/

6. Jenkins configuration
6.1 Git configuration

6.2 Build Triggers
Check on " Github hook trigger for GITScm polling"

6.3 Build
Execute shell
```bash
#!/bin/bash
ssh -T -i /home/jenkins/web-server.pem www-user@18.182.38.76 << EOF
cd /var/www/html/tinhocthatladongian
git pull
EOF
```
( Cài phần mềm >> nên cài bằng tay thôi ,chứ mỗi lần nó chạy nó lại cài thì k tốt lắm )

