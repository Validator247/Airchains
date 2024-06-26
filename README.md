# Airchains

<img width="789" alt="Ảnh màn hình 2024-05-10 lúc 14 04 51" src="https://github.com/Validator247/Airchains/assets/148058353/0f602ac7-f691-4c52-b5f1-5533c961aab3">

# Installation guide

Install required packages

    sudo apt update && \
    sudo apt install curl git jq build-essential gcc unzip wget lz4 -y

Install Go

    cd $HOME && \
    ver="1.21.3" && \
    wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
    sudo rm -rf /usr/local/go && \
    sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
    rm "go$ver.linux-amd64.tar.gz" && \
    echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
    source $HOME/.bash_profile && \
    go version

Download the source files:

    wget https://github.com/ignite/cli/releases/download/v0.27.1/ignite_0.27.1_linux_amd64.tar.gz

Modify file permissions:

    chmod +x ignite_0.27.1_linux_amd64.tar.gz

Extract the TAR file:

    tar -xvf ignite_0.27.1_linux_amd64.tar.gz

Move the binary:

    sudo mv ignite /usr/local/bin

Verify installation:

    ignite version

Set up environment variables:

    export PATH=$PATH:/usr/local/go/bin

# Running a Full Node

Download binary

    wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond

Make the binary executable

    chmod +x junctiond

Move the binary to a system-wide directory

    sudo mv junctiond /usr/local/bin

Initialize the Node with the Moniker

    junctiond init <moniker>

Update Genesis Configuration

    wget https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json

Replace the Existing Genesis File

    cp genesis.json ~/.junction/config/genesis.json

Update Configuration: 

Run the open configuration command

	nano ~/.junction/config/config.toml 	

 Replace peer

    persistent_peers = "de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656,aaf73849a5edb61ad5690d97c9e111520994c378@159.69.216.77:26656,84b6ccf69680c9459b3b78ca4ba80313fa9b315a@159.69.208.30:26656"

Set Mini Gas

	sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.00025amf"|g' $HOME/.junction//config/app.toml
 
create service

    sudo tee /etc/systemd/system/junctiond.service > /dev/null << EOF
    [Unit]
    Description=airchains node service
    After=network-online.target
    [Service]
    User=$USER
    ExecStart=/usr/local/bin/junctiond start
    Restart=on-failure
    RestartSec=10
    LimitNOFILE=65535
    [Install]
    WantedBy=multi-user.target
    EOF

    sudo systemctl daemon-reload
    sudo systemctl enable junctiond

Launch Node


    sudo systemctl restart junctiond && journalctl -u junctiond -f -o cat		
	

# Creat Wallet  ( Faucet token at the project discord channel)

    junctiond keys add wallet

To obtain the pubkey:

    junctiond comet show-validator


# Creat file Validator.json  ( nano validator.json )

        {
	"pubkey": <validator-pub-key>,
	"amount": "1000000amf",
	"moniker": "<validator-name>",
	"identity": "optional identity signature (ex. UPort or Keybase)",
	"website": "validator's (optional) website",
	"security": "validator's (optional) security contact email",
	"details": "validator's (optional) details",
	"commission-rate": "0.1",
	"commission-max-rate": "0.2",
	"commission-max-change-rate": "0.01",
	"min-self-delegation": "1"
    }

Finally, create the validator:

    junctiond tx staking create-validator path/to/validator.json --from wallet --chain-id junction --fees 500amf

        
# Done !                
