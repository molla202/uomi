```
cd
systemctl stop uomid
wget -O $HOME/uomi-node/uomi https://github.com/Uomi-network/uomi-node/releases/download/v0.1.9/uomi_0.1.9_ubuntu_22
systemctl restart uomid && journalctl -u uomid -fo cat
```
