# Langkah langkah deploy aplikasi psat2425

## Siapkan File
1. download filenya di https://github.com/paknux/psat2425 dan estrak filenya
2. buka vscode kemudian buka folder yang sudah didownload dan diestrak tadi
3. edit pada bagian file readme.md,diganti dengan langkah langkah deploy aplikasi psat2425
4. buka github dan buat repository baru dengan nama psat2425
5. push semua file tadi ke github

## Membuat Security Group
1. sebelum membuat instance ec2 dan rds kita harus membuat securiti group terlebih dahulu jika belum memiliki security group
2. buat dua security group yaitu SG-Serverweb dan SG-ServerDB
3. Buat security group di VPC
### SG-ServerWeb
allow inbound MySQL (3306) from anywhereIPv4 (0.0.0.0/0)
### SG-ServerDB
allow inbound SSH (22) from anywhereIPv4 (0.0.0.0/0)  
allow inbound HTTP (80) from anywhereIPv4 (0.0.0.0/0)  
allow inbound HTTPS (443) from anywhereIPv4 (0.0.0.0/0)

## Membuat Instance EC2 dan rds
setelah kita sudah membuat security group maka kita sudah bisa membuat instance EC2 dan rds
### Membuat rds
1. Masuk ke menu RDS kemudian buat database baru
2. kemudian pilih standard lalu pilih yang mysql
3. untuk templates kita pilih yang free tier lalu DB cluster identifier diisi bebas untuk namanya sesuai keinginan
4. kemdian untuk Master Username : (biarkan admin ) Master Password : isi misal P4ssw0rd123 
5. untuk public akses = no ya masa db
6. Untuk Security group pastikan pilih security group yang sudah kita buat tadi(SG-ServerDB)
7. Klik Create Database
8. Tunggu sampai EndPoint muncul
### Membuat Instance EC2
1. Masuk ke menu EC2 kemudian launch Instance
2. Buat nama sesuai keinginan
3. pilih ubuntu dan pilih nano/micro
4. pilih vockey pada bagian key pair
5. pilih security group yang sudah kita buat sebelumnya (SG-serverWeb) kemudian launch instance

## Deploy aplikasi psat2425
1. Masuk ke menu instance yang sudah kita buat sebelumnya 
2. kemudian kita connectkan dan jika sudah masuk ke ubuntu servernya kita tinggal masukan perintah scripting bash
### bash scripting
sudo apt update -y

sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client

sudo rm -rf /var/www/html/{_,._}

sudo git clone https://github.com/januarElfndy/psat2425.git /var/www/html >>

sudo chmod -R 777 /var/www/html

echo DB_USER=admin > /var/www/html/.env >> biarkan admin
echo DB_PASS=P4ssw0rd123 >> /var/www/html/.env >> ini kita masukan passwondr seperti di database
echo DB_NAME=psat2425 >> /var/www/html/.env >> 
echo DB_HOST=rdsku.czt6n8ylfvyb.us-east-1.rds.amazonaws.com >> /var/www/html/.env >> diisi dengan endpoint yang sudah kita dapatkan tadi

sudo apt install openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2

jika sudah berhasil dideploy maka kita tinggal salin ip public dan buka dichrome kemudian masukan username dan password yang tadi, dan kita sudah bisa menambahkan data kita.

## TERIMAKASIH