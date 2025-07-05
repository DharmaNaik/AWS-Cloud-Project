# Payment 
## python , version 3.9

```
sudo su -
```

``` 
dnf install python3 gcc python3-devel -y
```

``` 
useradd --system --home /app --shell /sbin/nologin --comment "roboshop system user" roboshop
```

```
curl -L -o /tmp/payment.zip https://roboshop-artifacts.s3.amazonaws.com/payment-v3.zip
```

```
mkdir /app 
```

```
cd /app
```

```
unzip /tmp/payment.zip
```

Lets download the dependencies.

``` 
pip3 install -r requirements.txt
```

```
vim /etc/systemd/system/payment.service
```

```
title=/etc/systemd/system/payment.service
[Unit]
Description=Payment Service

[Service]
User=root
WorkingDirectory=/app
// highlight-start
Environment=CART_HOST=<CART-SERVER-IPADDRESS>
Environment=CART_PORT=8080
Environment=USER_HOST=<USER-SERVER-IPADDRESS>
Environment=USER_PORT=8080
Environment=AMQP_HOST=<RABBITMQ-SERVER-IPADDRESS>
// highlight-end
Environment=AMQP_USER=roboshop
Environment=AMQP_PASS=roboshop123

ExecStart=/usr/local/bin/uwsgi --ini payment.ini
ExecStop=/bin/kill -9 $MAINPID
SyslogIdentifier=payment

[Install]
WantedBy=multi-user.target
```
Replace CART-SERVER-IPADDRESS with `cart server private IP`

Replace USER-SERVER-IPADDRESS with `user server private IP`

Replace RABBITMQ-SERVER-IPADDRESS with `rabbit server private IP`


``` 
systemctl daemon-reload
```


``` 
systemctl enable payment
```

```
systemctl start payment
```
Note:Update `Payment` server IP in frontend `config`file ,then `restart` frontend
