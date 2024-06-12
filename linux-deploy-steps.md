
### ---add user---

adduser harry

usermod -aG sudo harry


### ---install packages---
sudo apt install python3-pip python3-dev nginx -y

python3 -m venv venv <br>
source venv/bin/activate

pip3 install -r requirements.txt

pip3 install gunicorn

### ---prepare app---

app.py >>> ```host="0.0.0.0"```

nano wsgi.py >>>
```
from app import app

if __name__ == '__main__':
app.run()
```
### ---test app with gunicorn---
gunicorn --bind 0.0.0.0:5000 wsgi:app

### ---setup service---
##### create a system service
sudo nano /etc/systemd/system/app.service

```

[Unit]
Description=Gunicorn instance to serve myproject
After=network.target
[Service]
# add user
User=harry
Group=www-data
# add project directory [pwd]
WorkingDirectory=/home/harry/myApp/
# replace project directory
Environment="PATH=/home/harry/myApp/venv/bin"
# replace project directory
ExecStart=/home/harry/myApp/env/bin/gunicorn --workers 3 --bind unix:app.sock -m 007 wsgi:app
[Install]
WantedBy=multi-user.target

```
<br>

##### sudo systemctl start app [need this command after every restart] <br>
sudo systemctl enable app   [ this will enable service for every boot] <br>
sudo systemctl status app   [ check status] <br>
sudo systemctl restart app


### ---NGINX setup---

sudo nano /etc/nginx/sites-available/app


```
server {
listen 80;
server_name IP ADDRESS;

location / {
  include proxy_params;
  proxy_pass http://unix:[PROJECT DIRECTORY]/app.sock;
    }

location /static  {
    include  /etc/nginx/mime.types;
    root [PROJECT DIRECTORY];
  }
}


```

sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled

### --- PERMISSIONS---
sudo chmod 775 -R /home/harry/myFlaskApp
sudo chmod 775 -R /home/harry
sudo systemctl restart nginx
sudo ufw allow 'Nginx Full'
