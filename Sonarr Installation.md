# Sonarr Installation

# Prerequisites
```
sudo addgroup media
sudo adduser jackett media
```

# Installation
Add Mono Repo (20.04)
```
sudo apt install gnupg ca-certificates
sudo gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mono-official-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb [signed-by=/usr/share/keyrings/mono-official-archive-keyring.gpg] https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
```

Get MediaInfo
```
wget https://mediaarea.net/repo/deb/repo-mediaarea_1.0-19_all.deb && sudo dpkg -i repo-mediaarea_1.0-19_all.deb
```

Add the Sonarr repository
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 2009837CBFFD68F45BC180471F4F90DE2A9B4BF8
echo "deb https://apt.sonarr.tv/debian buster main" | sudo tee /etc/apt/sources.list.d/sonarr.list
sudo apt update
```

Install Sonarr
```
sudo apt install sonarr
```

```
sudo mkdir /lib/systemd/system/sonarr.service.d
cat << EOF | sudo tee /lib/systemd/system/sonarr.service.d/user.conf > /dev/null
# Override service user
[Service]
Group=media
EOF
```
