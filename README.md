# AZTECH-SEQUENCER-NODE-RUN-GUIDE 

<div align="center">

</div>

# 🖥️ Device/System Requirements 

![image](https://github.com/user-attachments/assets/9e7e78a8-ddeb-4e0b-90a8-46f2ab5886b3)



# Pre-Requirements 🛠

- Docker & Docker Compose

(Let run your docker in background if u are using local Device)

- Aztec Tool

- Sepolia Rpc URL



# Install All Require Dependecies

```
sudo apt-get update && sudo apt-get upgrade -y
```

* Install Node.js 

```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt update && sudo apt install -y nodejs
```

* Other Packages

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```


# Install Docker & Docker Compose


```
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```
sudo apt update && sudo apt install -y docker-ce && sudo systemctl enable --now docker
```

```
sudo usermod -aG docker $USER && newgrp docker
```


```
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose
```


*  Verify installation

```
docker --version && docker-compose --version
```



# Install the Aztec CLI

```
bash -i <(curl -s https://install.aztec.network)
```


* Lets Config it to your corrent Shell/Path

```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
```

```
source ~/.bashrc
```

* Verify the Installation with-

```
aztec -h
```


* Set the correct version for the testnet

```
aztec-up alpha-testnet
```


# Load your wallet with Sepolia Faucet 

https://testnetbridge.com/sepolia

https://sepolia-faucet.pk910.de/

https://www.alchemy.com/faucets/ethereum-sepolia



# Allow Incoming connections on Ports 

```
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw enable
```

```
sudo ufw allow 40400
sudo ufw allow 8080
```

   
#  Start Your Sequencer 

</div>

* Create a Screen Session

```
screen -S aztec
```

 ## Execute below given command to Start Your node & Dont forget to make changes in it-

```

aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls SepRPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKey YourPrivateKey \
  --sequencer.coinbase YourMMAddress \
  --p2p.p2pIp YourIP

```


# Detached and Attached From the Screen

* For detached from screen session - `ctrl` , `a` + `d`

* For Attach - 

```
screen -r aztec
```
   
# Get Apprentice Role In dc- 


## Step 1: Get the latest proven block number**

```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

## Step 2: Generate your sync proof**

```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK_NUMBER","BLOCK_NUMBER"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```


## Step 3: Register with Discord**

## join dc- https://discord.gg/aztec 


### 🔍 Find Peer ID :

```bash
sudo docker logs $(docker ps -q --filter ancestor=aztecprotocol/aztec:alpha-testnet | head -n 1) 2>&1 | grep -i "peerId" | grep -o '"peerId":"[^"]*"' | cut -d'"' -f4 | head -n 1
```

## Monitor Logs in Real-Time

```bash
sudo docker logs -f --tail 100 $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1)
```

## Join TG for more Updates: https://t.me/ntekearning2

