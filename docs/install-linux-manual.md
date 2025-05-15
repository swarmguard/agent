# Linux Manual Setup Guide
The manual setup guide explains the steps to install SwarmGuard binaries manually. Basic technical knowledge and administrator access to the target system is needed. If you are looking for the standard industrial setup guide or the setup guide for industrial app stores, please follow the [industrial setup guide](https://swarmguard.com/industrial/setup). 

### Sign up and download the app
- [Sign up](https://swarmguard.io/industrial/sign-up/) for a SwarmGuard account.
- Download the SwarmGuard Industrial app from the [Google Play Store](https://play.google.com/store/apps/details?id=com.inalp.swarmguard_admin.industrial) or [Apple App Store](https://apps.apple.com/ch/app/swarmguard-industrial/id6478231418).

### Installation
Download the binary for your architecture from the official [release page](https://github.com/swarmguard/agent/releases) and copy it to your target system to `/usr/bin`. 

Create two links, `swarm` and  `swarmd`, both linking to the binary you just installed to `/usr/bin`. Keep in mind that the name of your binary might differ depending on the architecture of your system.
```
sudo ln -s /usr/bin/swarmd-linux-amd64 /usr/bin/swarmd
sudo ln -s /usr/bin/swarmd-linux-amd64 /usr/bin/swarm
```

Create a systemd service file `/etc/systemd/system/swarmguard.service` with the following content:
```
[Unit]
Description=SwarmGuard Agent
After=network-online.target
StartLimitIntervalSec=0

[Service]
Type=simple
User=root
ExecStart=/usr/bin/swarmd
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

### Start the SwarmGuard agent
To start the SwarmGuard agent service, run the following command:
```
sudo systemctl start swarmguard
```
To enable the SwarmGuard agent service to be started by default on boot, run the following command:
```
sudo systemctl enable swarmguard
```
Run the command `sudo swarm status` to see the status of the SwarmGuard agent. Run the command `sudo systemctl status swarmguard` to see the status of the systemd service.

### Register your device
To register your device, scan the QR code with the SwarmGuard Industrial mobile app. The QR code can be displayed by running the command `sudo swarm register`.

