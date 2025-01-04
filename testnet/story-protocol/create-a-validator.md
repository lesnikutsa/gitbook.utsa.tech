# 💻 Create a validator

#### First, let's look at the public validator keys

```bash
story validator export
```

#### Export EVM validator key to default data configuration directory

```bash
story validator export --export-evm-key
```

#### Now we can see our private\_key

```bash
cat /root/.story/story/config/private_key.txt
```

Now we need to import our private\_key into the EVM wallet and request tokens from the faucet

#### Everything is ready to create a validator

```bash
story validator create --stake 1234000000000000000000 --moniker "<moniker>"  --private-key "<your_private_key>" --chain-id 1516
```

{% hint style="info" %}
This command will create a validator that matches the validator key stored in priv\_validator\_key.json. Note that at least $1024 IP IP (equivalent to 1024000000000000000000 wei) must be staked to participate in the consensus.
{% endhint %}

{% hint style="warning" %}
**Don't forget to save priv\_validator\_key.json**&#x20;

**Don't forget to save private\_key.txt**
{% endhint %}

