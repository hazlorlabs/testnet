# Hazlor - Planet Hatlas Testnet 

<div align="center">
  <h1> </h1>
</div>

<!-- TODO: add banner -->
<![banner](https://pbs.twimg.com/media/FHpGU6LVkAEu8fn?format=jpg&name=small)

Planned Start Time: 2/01/2022, 16:00 PST;

**This testnet will not be incentivized.**



### Install and build latest Binary:

**Prerequisites:** If you want to install Starport locally, make sure to have [Golang >=1.17](https://golang.org/). The latest version of Starport also requires [Protocol Buffer compiler](https://grpc.io/docs/protoc-installation/) to be installed. [Node.js >=12.19.0](https://nodejs.org/) is used to build the welcome screen, block explorer and to run the web scaffold.

#### Build from source

Starport uses [Git LFS](https://git-lfs.github.com/). **Please make sure that it is installed before cloning Starport.**

If you have installed Git LFS after cloning Starport, checkout to your preferred branch to trigger a pull for large files or run `git lfs pull`.

You need to ensure your gopath configuration is correct. If the following **'make'** step does not work then you might have to add these lines to your .profile or .zshrc in the users home folder:

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```


Note: When building from source, it is important to have your `$GOPATH` set correctly. When in doubt, the following should do:

```sh
mkdir ~/go
export GOPATH=~/go
```

##### Building Hazlor

Be sure to use the latest version.

```sh
git clone https://github.com/hazlorlabs/blockchain.git
cd blockchain
Make Build
then
Make Install
```

### Minimum hardware requirements

- 4GB RAM
- 50GB of disk space
- 1.4 GHz amd64 CPU


## Setup validator node

Below are the instructions to generate & submit your genesis transaction

### Generate genesis transaction (gentx)

1. Initialize the Hazlor directories and create the local genesis file with the correct
   chain-id

   ```bash
   hazlord init <moniker-name> --chain-id hazlor_7878-1
   ```

2. Create a local key pair

   ```sh
   > hazlord keys add <key-name>
   ```

3. Add your account to your local genesis file with a given amount and the key you
   just created. Use only `100000000tscas`, other amounts will be ignored.

   ```bash
   hazlord add-genesis-account $(hazlord keys show <key-name> -a) 100000000tscas
   ```

4. Create the gentx

   ```bash
   hazlord gentx <key-name> 90000000tscas --chain-id=hazlor_7878-1
   ```

   If all goes well, you will see a message similar to the following:

   ```bash
   Genesis transaction written to "/home/user/.hazlor/config/gentx/gentx-******.json"
   ```

### Submit genesis transaction

- Fork [the testnets repo](https://github.com/hazlorlabs/testnet) into your Github account

- Clone your repo using

  ```bash
  git clone https://github.com/<your-github-username>/testnet
  ```

- Copy the generated gentx json file to `<repo_path>/plant_hatlas/gentx/`

  ```sh
  > cd testnet
  > cp ~/.hazlord/config/gentx/gentx*.json ./hazlor_7878-1/gentx/
  ```

- Commit and push to your repo
- Create a PR onto https://github.com/hazlorlabs/testnet
- Only PRs from individuals / groups with a history successfully running nodes will be accepted. This is to ensure the network successfully starts on time.

#### Running in production

Download Genesis file when the time is right. Put it in your `/home/user/.hazlor` folder.

Create a systemd file for your Hazlord service:

```sh
sudo vi /etc/systemd/system/hazlor.service
```

Copy and paste the following and update `<YOUR_USERNAME>`, `<GO_WORKSPACE>`, and `<CHAIN_ID>`:

```sh
Description=hazlord daemon
After=network-online.target
[Service]
User=<YOUR_USERNAME>
ExecStart=/home/<YOUR_USERNAME>/<GO_WORKSPACE>/go/bin/hazlord start --
home=/home/<YOUR_USERNAME>/.hazlord
WorkingDirectory=/home/<YOUR_USERNAME>/go/bin
StandardOutput=file:/var/log/hazlord/digitaloceand.log
StandardError=file:/var/log/hazlord/digitaloceand_error.log
Restart=always
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
```

2
**This assumes `$HOME/go_workspace` to be your Go workspace, and `$HOME/.hazlor` to be your directory for config and data. Your actual directory locations may vary.**

Enable and start the new service:

```sh
sudo systemctl enable hazlord
sudo systemctl start hazlord
```

Check status:

```sh
hazlord status
```

Check logs:

```sh
journalctl -u hazlord -f
```

### Learn more


- [Hazlor Community Telegran](https://t.me/hazlorlabs)
- [Hazlor Community Discord](https://discord.gg/X6ZjdB4BEJ)


[Credit to the Juno Network for this guide](https://github.com/CosmosContracts/testnets) 
