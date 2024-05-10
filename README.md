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

Update Configuration 9 Edit ~/.junction/config/config.toml to set persistent_peers)

    persistent_peers = "de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656"

# Next (optional) set up systemD and launch. Create a wallet, get the public key file, create a validator.json file and set up the validator like other cosmos chains. Good luck        
