

![image](https://github.com/user-attachments/assets/8dbb0d99-6c88-4180-afe1-701bde8b7aa3)



### Download binary,genesis and system link
```
cd
mkdir -p $HOME/uomi-node
wget -O $HOME/uomi-node/uomi https://github.com/Uomi-network/uomi-node/releases/download/v0.1.7/uomi_0.1.7_ubuntu_22
wget -O $HOME/uomi-node/genesis.json https://github.com/Uomi-network/uomi-node/releases/download/v0.1.7/genesis.json
chmod +x $HOME/uomi-node/uomi
```
```
sudo ln -s $HOME/uomi-node/uomi /usr/local/bin/uomi -f
sudo ln -s $HOME/uomi-node/genesis.json /usr/local/bin/genesis.json -f
```
### Create validator account
```
uomi key generate --scheme Sr25519
```

![image](https://github.com/user-attachments/assets/74dad38d-b5b4-4bbd-a585-3a347e6d37b5)

### Generate Ed25519 key for GRANDPA
```
uomi key inspect --scheme Ed25519 "YOUR_SEED_PHRASE"
```


### Service
NOT: başka bir polkadat yada port değiştirmek isterseniz ekli değiştirebilirsiniz ancak daha ileriki kodlarda portlarla işimiz var ona göre uyarlamanız gerekir.
```
sudo tee /etc/systemd/system/uomid.service > /dev/null <<'EOF'
[Unit]
Description=Uomi Validator
After=network.target
StartLimitIntervalSec=0

[Service]
User=root
Type=simple
Restart=always
RestartSec=120
ExecStart=$HOME/uomi-node/uomi \
    --validator \
    --name "YOUR_NODE_NAME" \
    --chain "$HOME/uomi-node/genesis.json" \
    --base-path "$HOME/uomi-node" \
    --state-pruning 1000 \
    --blocks-pruning 1000 \
    --enable-evm-rpc \
    --rpc-cors all \
    --prometheus-external \
    --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
    --rpc-port 9944 \
    --port 30333  \
    --reserved-nodes "/ip4/204.12.199.4/tcp/30333/p2p/12D3KooWPDoa2JKhgRneM5eUktHEio4qqA7kMyfruzkwBhnbe2VX" \
    --reserved-nodes "/ip4/65.109.142.4/tcp/30333/p2p/12D3KooWSGp485hkoGXUucjWRDKf9AXaYQy9aLzMWpqhPFw816rZ" \
    --reserved-nodes "/ip4/138.199.152.43/tcp/30333/p2p/12D3KooWAz8V3VCrW2RwoyLRMSeoHhqcAHJtEFi4vtFhpWART5Ya"


[Install]
WantedBy=multi-user.target
EOF
```

### Service enable and run
```
sudo systemctl daemon-reload
sudo systemctl enable uomid.service
sudo systemctl restart uomid.service
```
### Service Log
```
journalctl -u uomi -fo cat
```

###  insert your keys
NOT: SECRET_PHRASE yerine kelimelerimizi yazacağız. PUBLIC_KEY yerine public hex yazacağız
```
for key_type in babe imon uomi ipfs; do
    curl -H "Content-Type: application/json" \
        -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["'$key_type'", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
        http://localhost:9944
done
```

```
curl -H "Content-Type: application/json" \
    -d '{"id":1, "jsonrpc":"2.0", "method":"author_insertKey", "params":["gran", "SECRET_PHRASE", "PUBLIC_KEY"]}' \
    http://localhost:9944
```
### Sezon key
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```
NOT: aldığımız keyle explore gidiyoruz ve validatore tıklayıp. keyimizi yazarak validator oluyoruz.

![image](https://github.com/user-attachments/assets/4df8e7b7-34b3-4005-9c81-e62b5a5521e7)



