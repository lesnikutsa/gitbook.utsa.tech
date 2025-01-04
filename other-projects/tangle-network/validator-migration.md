# 📬 Validator migration

{% hint style="info" %}
Unlike standard Polkadot nodes in Tangle, the validator is also a DKG center - this means that you will have to continue to use the same keys that were previously created on the first server, otherwise the validator will not be able to sign blocks
{% endhint %}

1. We cool down the validator in polkadot js and stop the node on the old server. If you haven’t copied your keys before, then it’s time to do it now
2. We launch the node on the new server as usual and fully synchronize
3. After synchronization on the new server, stop the node, replace the keys in $HOME/.tangle/data/chains/tangle-standalone-testnet/keystore/ and turn on the node
