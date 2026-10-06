# 💻 Useful commands



```shell
# chain check
doli --rpc http://127.0.0.1:8500 --network mainnet chain

# network check
curl -s -X POST http://127.0.0.1:8500 -H "Content-Type: application/json" -d '{"jsonrpc":"2.0","method":"getNetworkInfo","params":{},"id":1}' | jq

# wallet check
doli -w $HOME/.doli/mainnet/wallet.json --network mainnet info

# balance check
doli -w $HOME/.doli/mainnet/wallet.json --rpc http://127.0.0.1:8500 balance

# add bond
doli -w $HOME/.doli/mainnet/wallet.json \
  --rpc http://127.0.0.1:8500 \
  --network mainnet \
  producer add-bond --count 10

# send transaction
doli -w $HOME/.doli/mainnet/wallet.json \
  --rpc http://127.0.0.1:8500 \
  --network mainnet \
  send <ADDRESS> <AMOUNT>
```

```shell
# Delete node
systemctl stop doli
systemctl disable doli
rm -f /etc/systemd/system/doli.service
systemctl daemon-reload

cd
rm -rf doli .doli
rm -f /usr/local/bin/doli
rm -f /usr/local/bin/doli-node
```







