```
cd
systemctl stop uomid
wget -O $HOME/uomi-node/uomi https://github.com/Uomi-network/uomi-node/releases/download/v0.3.4/uomi_ubuntu_22_v0.3.4
systemctl restart uomid && journalctl -u uomid -fo cat
```
