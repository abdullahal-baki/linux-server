### ---install packages---
python3 -m venv venv  <br>
source venv/bin/activate

pip3 install -r requirements.txt

chmod +x /path/to/your_script.py

### ---prepare app---
sudo nano /etc/systemd/system/[serviceName].service

```
[Unit]
Description=Telegram bot Service
After=network.target

[Service]
Environment="PATH=/home/harry/Desktop/bot/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sb>
ExecStart=/home/harry/Desktop/bot/venv/bin/python /home/harry/Desktop/bot/main.py
WorkingDirectory=/home/harry/Desktop/bot
StandardOutput=inherit
StandardError=inherit
Restart=always
User=harry

[Install]
WantedBy=multi-user.target

```
sudo systemctl daemon-reload <br>

sudo systemctl start my_python_service.service <br>

sudo systemctl enable my_python_service.service <br>

sudo systemctl restart my_python_service.service <br>

sudo systemctl status my_python_service.service <br>
