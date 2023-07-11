# LinuxHomeMediaServer

# Install Jackett
## Create Jackett user
`sudo adduser --system --group jackett`

## Download Jackett and install using Servarr script
`cd /opt && f=Jackett.Binaries.LinuxAMDx64.tar.gz && release=$(wget -q https://github.com/Jackett/Jackett/releases/latest -O - | grep "title>Release" | cut -d " " -f 4) && sudo wget -Nc https://github.com/Jackett/Jackett/releases/download/$release/"$f" && sudo tar -xzf "$f" && sudo rm -f "$f" && cd Jackett* && sudo ./install_service_systemd.sh && systemctl status jackett.service && cd - && echo -e "\nVisit http://127.0.0.1:9117"`

## Stop Jacket service to make changes to systemd service user and group
`sudo systemctl stop jackett.service`

`sudo mkdir /etc/systemd/system/jackett.service.d`
```
cat << EOF | sudo tee /etc/systemd/system/jackett.service.d/user.conf > /dev/null
# Override service user
[Service]
User=jackett
Group=jackett
EOF
```

`sudo systemctl -q daemon-reload`

`sudo systemctl enable --now -q jackett`

# Uninstall
```
sudo systemctl daemon-reload
sudo systemctl stop jackett.service
sudo systemctl disable jackett.service
sudo rm -rf /etc/systemd/system/jackett.service*
sudo rm -rf /opt/Jackett
sudo userdel jackett
```
