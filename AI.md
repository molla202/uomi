
```
sudo apt-get update && sudo apt-get upgrade
apt-get install build-essential screen git nano
```
```
wget https://repo.anaconda.com/archive/Anaconda3-2023.07-1-Linux-x86_64.sh
bash Anaconda3-2023.07-1-Linux-x86_64.sh
```
```
source ~/.bashrc
conda list
```
```
sudo apt update && sudo apt install python3.10-venv python3-venv python3-pip
```
```
git clone https://github.com/Uomi-network/uomi-node-ai/
cd uomi-node-ai
```
```
conda create -n uomi-ai python=3.10 -y
```
```
conda activate uomi-ai
```
```
conda install pytorch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 pytorch-cuda=12.1 -c pytorch -c nvidia 
conda install -c nvidia/label/cuda-12.1.0 cuda-nvcc=12.1 cuda-toolkit=12.1
pip install -U "optimum>=1.20.0"
pip install auto-gptq --no-build-isolation
pip install transformers
conda install numpy=1.24
pip install flash-attn --no-build-isolation
pip install flask
```
```
nvcc --version
```
```
screen -S ai
```
```
cd
cd uomi-node-ai
conda activate uomi-ai
python3 /root/uomi-node-ai/uomi-ai.py
```
NOT: hata verirse alttakini dneyin
```
pip uninstall flash-attn
pip install flash-attn --no-binary :all:
```


### Service
```
sudo tee /etc/systemd/system/uomiaid.service > /dev/null << EOF
[Unit]
Description=Uomi AI Service
After=network.target

[Service]
User=root
WorkingDirectory=/root/uomi-node-ai
Environment="PATH=/root/anaconda3/envs/uomi-ai/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
ExecStart=/bin/bash -c "source /root/anaconda3/bin/activate uomi-ai && exec python3 /root/uomi-node-ai/uomi-ai.py"
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable uomiaid
sudo systemctl start uomiaid
```
### log komutu
```
sudo journalctl -u uomiaid -f
```
