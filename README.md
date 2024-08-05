# AirChains-Node-Guide

![image](https://github.com/user-attachments/assets/2ab19c2c-98cf-4c74-9ce8-699a0dd5a861)


# Run a Validator Node 🛠️🚀

This guide provides a comprehensive step-by-step approach to setting up and operating a full validator node on the Junction network. Follow these instructions to get started. 📜🔧

## Download the `junctiond` Binary 📥

Use the `wget` command to download the `junctiond` executable from the Airchains Network GitHub release page:

```bash
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond
```

## Make the Binary Executable ⚙️

Apply the `chmod +x` command to add executable permissions to the `junctiond` file:

```bash
chmod +x junctiond
```

## Move the Binary to a System-Wide Directory 📂

Transfer the `junctiond` binary to `/usr/local/bin` so it can be accessed from anywhere on the system:

```bash
sudo mv junctiond /usr/local/bin
```

## Initialize the Node with the Moniker 🆔

Initialize your node with your chosen moniker:

```bash
junctiond init <moniker>
```

## Update Genesis Configuration 🔄

### Download the Testnet Genesis File 🌐

Download the latest genesis file from the GitHub repository:

```bash
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json
```

### Replace the Existing Genesis File 🔄

Replace the existing genesis file in your Junction node configuration directory:

```bash
cp genesis.json ~/.junction/config/genesis.json
```

## Update Configuration 🛠️

Edit the `~/.junction/config/config.toml` file to set `persistent_peers`:

```toml
persistent_peers = "de2e7251667dee5de5eed98e54a58749fadd23d8@34.22.237.85:26656"
```

## Start the Node 🚀

Before starting the node, set the minimum gas price in your `app.toml` configuration file:

1. Locate `app.toml` at `~/.junction/config/app.toml`.
2. Set the `minimum-gas-prices` to `0.00025amf`.

Then, start your node:

```bash
junctiond start
```

## Wait for the Node to Sync ⏳

Check the status of the node:

```bash
junctiond status
```

If the `catching_up` field returns `true`, wait until the node completes synchronization. Do not proceed until this process is finished.

## Create a New Account for the Validator 👤

Generate your validator wallet's mnemonic and address:

```bash
junctiond keys add <validator-name>
```

**Important:** Write down and securely store the mnemonic and address.

## Fund Your Account 💰

Ensure your validator account holds a minimum of **58 tokens**. If your account lacks tokens, acquire testnet tokens from our [Discord faucet channel](https://discord.gg/airchains). The faucet is accessible at Airchains Faucet.

## Stake Tokens to Become a Validator 💼

Create a `validator.json` file with the following details:

### Example `validator.json`:

```json
{
    "pubkey": "<validator-pub-key>",
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
```

To obtain the pubkey, use:

```bash
junctiond comet show-validator
```

**Example Output:**

```json
{"@type":"/cosmos.crypto.ed25519.PubKey","key":"ZXONS7NNjLWH4HePBOoHKDAYeLXQO5iUwpCRQSi1poI="}
```

Replace `<validator-pub-key>` with the pubkey value from the command output.

Then, execute the staking command:

```bash
junctiond tx staking create-validator path/to/validator.json --from <key-name> --chain-id junction --fees 500amf
```

A prompt will appear in the CLI. To proceed, type `y` and press enter. The command will return a transaction hash:

```json
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 3068ED7C9867D9DC926A200363704715AE9470EE73452324A32C2583E62B1D79
```

## Query Validator Set 🔍

Check if you were accepted as a validator:

```bash
junctiond query tendermint-validator-set
```

If your address is visible, you've been successfully included in the validator set! 🎉
