# Install Radarr

## Prerequisites
```
sudo adduser --system radarr
sudo mkdir /var/lib/radarr
sudo chown radarr:radarr /var/lib/radarr
```

Download binaries for x64
```
wget --content-disposition 'http://radarr.servarr.com/v1/update/master/updatefile?os=linux&runtime=netcore&arch=x64'
```

Uncompress the files:
```
tar -xvzf Radarr*.linux*.tar.gz
```

Move the files to /opt/
```
sudo mv Radarr /opt/
```

Ensure ownership of the binary directory.
```
sudo chown radarr:radarr -R /opt/Radarr
```

Configure systemd so Radarr can autostart at boot.
```
cat << EOF | sudo tee /etc/systemd/system/radarr.service > /dev/null
[Unit]
Description=Radarr Daemon
After=syslog.target network.target
[Service]
User=radarr
Group=media
Type=simple

ExecStart=/opt/Radarr/Radarr -nobrowser -data=/var/lib/radarr/
TimeoutStopSec=20
KillMode=process
Restart=on-failure
[Install]
WantedBy=multi-user.target
EOF
```

```
sudo mkdir /etc/systemd/system/radarr.service.d
cat << EOF | sudo tee /etc/systemd/system/radarr.service.d/user.conf > /dev/null
# Override service user
[Service]
Group=radarr
EOF
```

Reload systemd:
```
sudo systemctl -q daemon-reload
```
Enable the Radarr service:
```
sudo systemctl enable --now -q radarr
```