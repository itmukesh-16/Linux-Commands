# Auto Start a Python Application on Linux Using Systemd

This guide explains how to run a Python application as a non-root user and automatically start it whenever the server reboots.

## Step 1: Create a Non-Root User

Create a dedicated user for running the application.

```bash
sudo useradd --system --no-create-home appuser
```

## Step 2: Place the Application

Create an application directory:

```bash
sudo mkdir -p /home/appuser/myapp
```

Copy your application files into the directory and assign ownership:

```bash
sudo chown -R appuser:appuser /home/appuser/myapp
```

Example:

```text
/home/appuser/myapp/
├── app.py
├── requirements.txt
└── config.json
```

---

## Step 3: Create a Systemd Service

Create a service file:

```bash
sudo nano /etc/systemd/system/myapp.service
```

Add the following configuration:

```ini
[Unit]
Description=My Python Application
After=network.target

[Service]
Type=simple
User=appuser
Group=appuser
WorkingDirectory=/home/appuser/myapp
ExecStart=/usr/bin/python3 /home/appuser/myapp/app.py

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Service Configuration Explained

- `User` : Runs the application as a non-root user.
- `Group` : Runs under the specified group.
- `WorkingDirectory` : Folder where the application executes.
- `ExecStart` : Command used to start the application.
- `Restart=always` : Restarts the application if it crashes.
- `WantedBy=multi-user.target` : Starts the service during system boot.

---

## Step 4: Reload Systemd

Reload systemd to recognize the new service:

```bash
sudo systemctl daemon-reload
```

---

## Step 5: Enable Auto-Start on Reboot

Enable the service:

```bash
sudo systemctl enable myapp.service
```

This ensures the application starts automatically after every reboot.

---

## Step 6: Start the Service

Start the application immediately:

```bash
sudo systemctl start myapp.service
```

---

## Step 7: Verify the Service

Check service status:

```bash
sudo systemctl status myapp.service
```

Expected output:

```text
Active: active (running)
```

---

## View Application Logs

Follow logs in real time:

```bash
sudo journalctl -u myapp.service -f
```

View recent logs:

```bash
sudo journalctl -u myapp.service -n 50
```

---

## Useful Commands

Start service:

```bash
sudo systemctl start myapp.service
```

Stop service:

```bash
sudo systemctl stop myapp.service
```

Restart service:

```bash
sudo systemctl restart myapp.service
```

Check status:

```bash
sudo systemctl status myapp.service
```

Disable auto-start:

```bash
sudo systemctl disable myapp.service
```

Enable auto-start:

```bash
sudo systemctl enable myapp.service
```

---

## Troubleshooting

Verify Python path:

```bash
which python3
```

Check service logs:

```bash
sudo journalctl -xeu myapp.service
```

Verify file permissions:

```bash
ls -l /home/appuser/myapp
```

Ensure the application user owns the files:

```bash
sudo chown -R appuser:appuser /home/appuser/myapp
```

---

## Summary

1. Create a non-root user.
2. Store application files in a user-accessible location.
3. Create a systemd service.
4. Reload systemd.
5. Enable the service.
6. Start the service.
7. Validate status and logs.

Your Python application will now start automatically whenever the server boots.
