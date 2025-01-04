# ⚙️ Validator migration

* Launch the node on the new server as usual and fully synchronize
* After synchronization on the new server, run the command and copy the new key `curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}'` [`http://localhost:9933`](http://localhost:9953)
* Go to Network - Accounts - Change session keys and change our key. Sign the transaction
* If you are in an active set, you must wait until 2 full epochs have passed. Only then will the keys change. If you are not in an active set, the validator keys will change immediately
* Make sure that the keys have changed on the Network - Accounts page and only then disconnect the old server

{% hint style="warning" %}
For a quick transfer, you can transfer the keys created on the old server to the new one. BUT this is associated with the risks of double signing and is not recommended to do without the need and knowledge of what you are doing
{% endhint %}
