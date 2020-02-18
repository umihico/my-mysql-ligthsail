# my-mysql-ligthsail

SSH and run
```
sudo yum update -y

sudo yum install -y docker
sudo service docker start
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo service docker restart
docker version

sudo [ ! -f /usr/local/bin/docker-compose ] && sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
```

send env variables thru ssh
```
mysqlenv=$(aws ssm get-parameters --profile umihico --names "MYSQL_USER_STG" "MYSQL_ROOT_PASSWORD_STG" "MYSQL_PORT_STG" "MYSQL_USER_PROD" "MYSQL_ROOT_PASSWORD_PROD" "MYSQL_PORT_PROD"|python -c 'import sys, json;print("\n".join([d["Name"]+"="+d["Value"] for d in json.load(sys.stdin)["Parameters"]]))')
ssh mysql "cat > .env << EOS
$mysqlenv
EOS"
```

create swapfile if you need (`free`)
```
sudo dd if=/dev/zero of=/swapfile1 bs=1M count=1024
sudo chmod 600 /swapfile1
sudo mkswap /swapfile1
sudo swapon /swapfile1
```

place `docker-compose.yml` and `docker-compose up -d`
