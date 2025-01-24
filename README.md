<p align="center">
  <img height="100" height="auto" src="https://github.com/user-attachments/assets/1c6038d3-8db6-4e45-ac1a-e1b509e4ad52">
</p>

# Hemi Testnet

Official documentation:
>- [Validator setup instructions](https://docs.hemi.xyz/how-to-tutorials/using-hemi/pop-mining/setup-part-1)

Explorer:
>- [Explorer](https://testnet.explorer.hemi.xyz/)

### Minimum Hardware Requirements
 - 2x CPUs; the faster clock speed the better
 - 4GB RAM
 - 60GB of storage (SSD or NVME)

### Recommended Hardware Requirements 
 - 4x CPUs; the faster clock speed the better
 - 8GB RAM
 - 100GB of storage (SSD or NVME)

 - Ubuntu 22.04

Переходим на [сайт](https://points.absinthe.network/hemi/d/connections) и подключаем кошелек Metamask, код для доп. поинтов - 65323a9d

Привязываем Discord и Twiiter это можно сделать нажав "Connections" в Primary Wallet.

Ставим ноду:

``sudo apt update && sudo apt upgrade -y``

``sudo apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev tar clang bsdmainutils ncdu unzip libleveldb-dev lz4 screen bc fail2ban -y``

``curl -L -O https://github.com/hemilabs/heminetwork/releases/download/v0.11.1/heminetwork_v0.11.1_linux_amd64.tar.gz``

``mkdir -p hemi``

``tar --strip-components=1 -xzvf heminetwork_v0.11.1_linux_amd64.tar.gz -C hemi``

``cd hemi``

``./keygen -secp256k1 -json -net="testnet" > ~/popm-address.json``

Сохраняем данные кошелька. pubkey_hash — это Ваш tBTC адрес, на который нужно запросить тестовые токены в [Discord](https://discord.gg/hemixyz).

``cat ~/popm-address.json``

``nano popmd.env``

Вписываем свои значения:

POPM_BTC_PRIVKEY=Вписываем private_key

POPM_STATIC_FEE=50

POPM_BFG_URL=wss://testnet.rpc.hemi.network/v1/ws/public

CTRL + O Enter CRTL + X

cat <<EOT | sudo tee /etc/systemd/system/hemi.service > /dev/null

[Unit]

Description=PopMD Service

After=network.target

[Service]

User=root

EnvironmentFile=/root/hemi/popmd.env

ExecStart=/root/hemi/popmd

WorkingDirectory=/root/hemi/

Restart=on-failure

[Install]

WantedBy=multi-user.target

EOF

``sudo systemctl daemon-reload``

``sudo systemctl restart systemd-journald``

``sudo systemctl enable hemi``

``sudo systemctl start hemi``

Полезные команды:

Просмотр логов:

``sudo journalctl -u hemi -f``


Кран:

https://coinfaucet.eu/en/btc-testnet/

https://testnet.help/en/btcfaucet/testnet

Переходим на [сайт](https://pop-miner.hemi.xyz/). Нажимаем "Open web PoP miner" - "Continue a mining session" и вписываем private_key.
