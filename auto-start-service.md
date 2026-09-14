sudo useradd --system --no-create-home appuser

sudo chown -R appuser:appuser /opt/myapp

sudo vi /etc/systemd/system/myapp.service


[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
User=appuser
Group=appuser
WorkingDirectory=/opt/aws-ecomerce-Application-Multiple-services/backend
ExecStart=/opt/aws-ecomerce-Application-Multiple-services/backend/venv/bin/python3 /opt/aws-ecomerce-Application-Multiple-services/backend/app.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target


sudo systemctl daemon-reload
sudo systemctl enable myapp
sudo systemctl start myapp
sudo systemctl status myapp
sudo journalctl -u myapp -f
