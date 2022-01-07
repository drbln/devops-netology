# Курсовая работа по итогам модуля "DevOps и системное администрирование"

## Разворачиваем ubuntu

## Настройка ubuntu

```
adduser rudnev
```

```
usermod -aG sudo rudnev
```

```
sudo apt update
```

```
sudo apt install cron
```

```
sudo apt install vim
```

```
sudo apt install curl
```

```
sudo apt install jq
```

```
sudo apt install software-properties-common
```

```
sudo apt install nginx
```

```
sudo apt install screen
```

```
sudo apt install ufw
```

```
sudo timedatectl set-timezone Europe/Moscow
```

```
mkdir ssl
```

```
cd ssl
```

## Настройка ufw

```
sudo ufw allow 80
```

```
sudo ufw allow 443
```

```
sudo ufw allow 22
```

```
sudo ufw allow 8200
```

```
sudo ufw enable
```

## Установка, настройка и работа с Hashicorp Vault

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```

```
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main
```

```
sudo apt-get update && sudo apt-get install vault
```

```
screen -d -m vault server -dev -dev-root-token-id root
```

```
export VAULT_ADDR=http://127.0.0.1:8200
```

```
export VAULT_TOKEN=root
```

```
vault secrets enable pki
```

```
vault secrets tune -max-lease-ttl=87600h pki
```

```
vault write -field=certificate pki/root/generate/internal \
     common_name="example.com" \
     ttl=87600h > CA_cert.crt
```

```
vault write pki/config/urls \
     issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
     crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
```

```
vault secrets enable -path=pki_int pki
```

```
vault secrets tune -max-lease-ttl=43800h pki_int
```

```
vault write -format=json pki_int/intermediate/generate/internal \
     common_name="example.com Intermediate Authority" \
     | jq -r '.data.csr' > pki_intermediate.csr
```

```
vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
     format=pem_bundle ttl="43800h" \
     | jq -r '.data.certificate' > intermediate.cert.pem
```

```
vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
```

```
vault write pki_int/roles/example-dot-com \
     allowed_domains="example.com" \
     allow_subdomains=true \
     max_ttl="720h"
```

```
vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="740h" > /home/rudnev/ssl//test.example1.com.crt
```

```
cat /home/rudnev/ssl/test.example1.com.crt | jq -r .data.certificate > /home/rudnev/ssl/test.example.com.crt
```

```
cat /home/rudnev/ssl/test.example1.com.crt | jq -r .data.private_key > /home/rudnev/ssl/test.example.com.key
```

## Настройка nginx на https, используя ранее подготовленный сертификат

```
sudo mkdir /etc/nginx/ssl
```

```
sudo cp test.example.com.crt /etc/nginx/ssl
```

```
sudo cp test.example.com.key /etc/nginx/ssl
```

```
sudo vim /etc/nginx/sites-available/default
```

```
listen 443 ssl default_server;
   listen [::]:443 ssl default_server;
   ssl_certificate     /etc/nginx/ssl/test.example.com.crt;
   ssl_certificate_key /etc/nginx/ssl/test.example.com.key;
   ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
   ssl_ciphers         HIGH:!aNULL:!MD5;
```

```
sudo nginx -s reload
```

## Установка корневого и промежуточного сертификата на хостовой машине и проверка (Скрин в приложении 1.png)

## Поместите скрипт в crontab, чтобы сертификат обновлялся какого-то числа каждого месяца в удобное для вас время.

```
touch kr.sh
```

```
vim kr.sh
```

```
sudo rm /etc/nginx/ssl/test.example.com.crt
sudo rm /etc/nginx/ssl/test.example.com.key
sudo rm /home/rudnev/ssl/test.example1.com.crt
sudo rm /home/rudnev/ssl/test.example.com.crt
sudo rm /home/rudnev/ssl/test.example.com.key

screen -d -m bash -c "vault server -dev -dev-root-token-id root"
export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=root

vault write -format=json pki_int/issue/example-dot-com common_name="test.example.com" ttl="740h" > /home/rudnev/ssl//test.example1.com.crt && cat /home/rudnev/ssl/test.example1.com.crt | jq -r .data.certificate > /home/rudnev/ssl/test.example.com.crt && cat /home/rudnev/ssl/test.example1.com.crt | jq -r .data.private_key > /home/rudnev/ssl/test.example.com.key

sudo cp /home/rudnev/ssl/test.example.com.crt /etc/nginx/ssl/test.example.com.crt
sudo cp /home/rudnev/ssl/test.example.com.key /etc/nginx/ssl/test.example.com.key

sudo nginx -s reload
```

## Поместить скрипт в crontab, чтобы сертификат обновлялся 1 числа каждого месяца.

```
sudo crontab -e
```

```
0 0 1 * * bash /home/rudnev/ssl/kr.sh
```

## Что бы убедится в работоспособности скрипта в определённое время установил выполнение каждую минуту (скрины 2.png и 3.png в приложении)



