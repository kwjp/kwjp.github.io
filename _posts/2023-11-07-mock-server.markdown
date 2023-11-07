## 必要パッケージインストール
```
sudo dnf install python3 augeas-libs cronie
```

## venv作成・pipからcertbotをインストール
```
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
sudo /opt/certbot/bin/pip install certbot certbot-nginx
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

## certbot
```
sudo certbot certonly --standalone -d mock.run5.jp -d tl.mock.run5.jp --preferred-challenges dns
sudo cp /etc/letsencrypt/live/mock.run5.jp/fullchain.pem ngrok/assets/server/tls/snakeoil.crt
sudo cp /etc/letsencrypt/live/mock.run5.jp/privkey.pem ngrok/assets/server/tls/snakeoil.key
sudo cp ngrok/assets/server/tls/snakeoil.crt ngrok/assets/client/tls/ngrokroot.crt
```
