```
cd
systemctl stop uomid
wget -O $HOME/uomi-node/uomi https://github.com/Uomi-network/uomi-node/releases/download/v0.3.5/uomi_ubuntu_22_v0.3.5
systemctl restart uomid && journalctl -u uomid -fo cat
```
