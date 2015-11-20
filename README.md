# 21 Bitcoin Computer Demos #
https://21.co/learn/

### Setting up VM ######
    # Install dependencies
    sudo apt-get -y update
    sudo apt-get -y upgrade
    sudo apt-get install -y git-core
    sudo apt-get install -y build-essential libtool autotools-dev autoconf pkg-config libssl-dev
    sudo apt-get install -y libboost-all-dev

    # Install the special Berkeley DB dependency (and optionally MiniUPNPC)
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install -y libdb4.8-dev libdb4.8++-dev
    sudo apt-get install -y libminiupnpc-dev

    # Build bitcoind from source
    export CORES=$( nproc ) # Linux or Mac
    cd $HOME
    git clone https://github.com/bitcoin/bitcoin.git
    cd bitcoin
    git checkout v0.11.1
    ./autogen.sh
    ./configure --enable-hardening
    make -j $CORES

    # Add bitcoind, bitcoin-cli, bitcoin-tx to your path
    mkdir -p $HOME/bin
    cd $HOME/bin
    ln -s ../bitcoin/src/bitcoind .
    ln -s ../bitcoin/src/bitcoin-cli .
    ln -s ../bitcoin/src/bitcoin-tx .
    export PATH=$PATH:$HOME/bin

    # Set up bitcoin.conf
    mkdir -p $HOME/.bitcoin
    echo -e "rpcuser=bitcoinrpc\\nrpcpassword=MAKE_UP_A_PASSWORD\\n" > $HOME/.bitcoin/bitcoin.conf

    # Launch bitcoind
    bitcoind -daemon -txindex -reindex -server
