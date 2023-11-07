https://www.ywbj.cc/?p=965

## 必要パッケージインストール
```
sudo dnf install python3 augeas-libs cronie
```

## 下载获取 Ngrok 源码
```
git clone https://github.com/tutumcloud/ngrok.git ngrok
```

## venv作成・pipからcertbotをインストール
```
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
sudo /opt/certbot/bin/pip install certbot certbot-nginx
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

## renew certbot
```
sudo certbot certonly --force-renew --manual --preferred-challenges=dns -d mock.run5.jp -d tl.mock.run5.jp
```

## setup certbot
```
sudo certbot certonly --standalone -d mock.run5.jp -d tl.mock.run5.jp --preferred-challenges dns
```

## config cert
```
sudo cp /etc/letsencrypt/live/mock.run5.jp/fullchain.pem ngrok/assets/server/tls/snakeoil.crt
sudo cp /etc/letsencrypt/live/mock.run5.jp/privkey.pem ngrok/assets/server/tls/snakeoil.key
sudo cp ngrok/assets/server/tls/snakeoil.crt ngrok/assets/client/tls/ngrokroot.crt
```

## 编译生成服务端和客户端
```
sudo rm -rf ngrok/bin/darwin_amd64/ ngrok/bin/go-bindata ngrok/bin/ngrokd
sudo GOOS=linux GOARCH=amd64 make release-server
sudo GOOS=darwin GOARCH=amd64 make release-client
```

## start server
```
sudo ./bin/ngrokd -domain="mock.run5.jp" -httpAddr=":80" -httpsAddr=":443" -tunnelAddr=":4443"
```

## copy client
```
scp -r -i "mock.pem" ec2-user@ec2-54-95-221-150.ap-northeast-1.compute.amazonaws.com:/home/ec2-user/ngrok/bin/darwin_amd64/ ./
```

## config macos client
```
touch ngrok.cfg
vim ngrok.cfg
server_addr: "mock.run5.jp:4443"
trust_host_root_cert: false
```

## start macos client
```
./ngrok -subdomain tl -config=ngrok.cfg 80
```
