## setup JAVA_HOME
```
export JAVA_HOME=/usr/lib/jvm/jdk-17
```

## renew certbot
```
certbot certonly --force-renew -d deepcode.co.jp
```

## update cert
```
cd ~/tomcat/conf
cp /etc/letsencrypt/live/deepcode.co.jp/cert.pem ./
cp /etc/letsencrypt/live/deepcode.co.jp/chain.pem ./
cp /etc/letsencrypt/live/deepcode.co.jp/fullchain.pem ./
cp /etc/letsencrypt/live/deepcode.co.jp/privkey.pem ./
chown root:root *.pem
```

## restart
```
sh bin/startup.sh
```
