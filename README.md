# rasp-pi-ubuntu-kiosk
Make Kiosk with a Raspberry Pi 4 and Ubuntu 2022.04 LTS

### Install Ubuntu & Necessary Packages:
If you haven't already, install Ubuntu. Once that's done, make sure everything's up-to-date:
``` bash
sudo apt update && sudo apt upgrade
```
Then, install Chromium:
``` bash
sudo apt install chromium-browser
```

### Configure Chromium in Kiosk Mode:
You'll want to start Chromium in kiosk mode and without any UI. You can achieve this by using the --kiosk and --app flags:
bash
``` bash
chromium-browser --kiosk --noerrdialogs --disable-session-crashed-bubble --disable-infobars --app=https://www.killerwarehouse.com
```
### Automate the Launching Process:
There are a few methods to auto-launch Chromium upon boot, but given your expertise, we'll focus on a systemd service:
Create a new service file:
``` bash
sudo nano /etc/systemd/system/chromium-kiosk.service
```
Paste the following content:
``` ini
[Unit]
Description=Chromium Kiosk
Wants=graphical.target
After=graphical.target

[Service]
Environment=DISPLAY=:0.0
Environment=XAUTHORITY=/home/your_username/.Xauthority
Type=simple
ExecStart=/usr/bin/chromium-browser --kiosk --noerrdialogs --disable-session-crashed-bubble --disable-infobars --app=https://www.killerwarehouse.com
Restart=on-failure
User=your_username
Group=your_username

[Install]
WantedBy=graphical.target
Replace your_username with your actual username.
```

Enable and start the service:
``` bash
sudo systemctl enable chromium-kiosk
sudo systemctl start chromium-kiosk
```

### Disable Shortcuts and UI Interactions:
To ensure the user can't exit kiosk mode, you'll want to disable certain key combinations (like Alt+F4 or Ctrl+Alt+Delete). This requires setting up a custom window manager or using tools like xmodmap to remap/disable keys. The specifics can vary, so you may want to dive deeper into the tool you prefer.

### Optimization:
If this kiosk will be running 24/7, you might also consider optimizations such as disabling screen blanking, ensuring there's an auto-reboot schedule (in case of any crashes), and monitoring the system for any unforeseen issues.
