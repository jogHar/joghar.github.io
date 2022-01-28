---
layout: default
title: Usefull commnad-Ubuntu | Hardik Jogani
permalink: /doc/ubuntu
---

- [Port Kill](#port-kill)
- [Wi-Fi Driver](#wifi-driver)
- [SSH Generate](#ssh-generate)
- [Desktop Shortcut](#desktop-shortcut)

## Port Kill {#port-kill}
```bash
sudo kill -9 $(sudo lsof -t -i:9001)
```
here '9001' is port number

---

## Wi-Fi Driver {#wifi-driver}
```bash
sudo apt update

sudo apt install bcmwl-kernel-source
```

---

## SSH Generate {#ssh-generate}
```bash
ssh-keygen -t rsa -b 4096 -C "joganihardik57@gmail.com"

eval $(ssh-agent -s)

ssh-add ~/.ssh/id_rsa
```
copy your public key from '~/.ssh/id_rsa.pub' and use it. 

---

## Desktop Shortcut {#desktop-shortcut}
```bash
[Desktop Entry]
Version=1.0
Name=Eclipse
GenericName=Eclipse
Comment=Java IDE
Exec=/home/xyz/abc/eclipse/eclipse
Path=/home/xyz/abc/eclipse
Icon=/home/xyz/abc/eclipse/icon.xpm
Terminal=false
Type=Application
Categories=Application
```