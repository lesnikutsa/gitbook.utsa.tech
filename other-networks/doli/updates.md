# 📬 Updates

##



**UPD 🕊**

**Height 🕊**

```shell
systemctl stop doli

cd $HOME/doli
git fetch --all --tags
git checkout v6.43.0

cargo build --release -p doli-node -p doli-cli

cp $HOME/doli/target/release/doli-node /usr/local/bin/doli-node
cp $HOME/doli/target/release/doli /usr/local/bin/doli
chmod +x /usr/local/bin/doli-node /usr/local/bin/doli

doli-node --version
doli --version

systemctl restart doli && journalctl -u doli -f -o cat
```

