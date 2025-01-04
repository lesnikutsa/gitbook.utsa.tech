---
description: >-
  Story is the World’s IP Blockchain, onramping Programmable IP to power the
  next generation of AI, DeFi, and consumer applications
---

# Story Protocol

## Links

* **Web**: [https://www.story.foundation/](https://www.story.foundation/)
* **Discord**: [https://discord.gg/storyprotocol](https://discord.gg/storyprotocol)
* **Github**: [https://github.com/storyprotocol](https://github.com/storyprotocol)

## **Details**

* **Network Chain ID**: `odyssey` 1516
* **Binary**: `story` `story-geth`
* **Denom**: IP
* **Working directory**: .story

## Public services

* **Guide (RU)**: [https://teletype.in/@lesnik13utsa/QvzjUnVAn\_K](https://teletype.in/@lesnik13utsa/QvzjUnVAn_K)
* **RPC:** [https://t-story.archive.rpc.utsa.tech/](https://t-story.archive.rpc.utsa.tech/)
* **API:** [https://t-story.archive.api.utsa.tech/](https://t-story.archive.api.utsa.tech/)
* **EVM:** [https://t-story.archive.evm.utsa.tech:443](https://t-story.archive.evm.utsa.tech)
* **WSS:** [https://t-story.archive.wss.utsa.tech:443](https://t-story.archive.wss.utsa.tech)

## Peering

You can use peer **lesnik | UTSA** for fast connection or state sync

```shell
peers="90161a7f82ce5dbfbed1a2a9d40d4103730cff0f@5.9.87.231:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$peers\"|" $HOME/.story/story/config/config.toml
```

The address book is updated once an hour. You can use it for quick launch

```shell
wget -O $HOME/.story/story/config/addrbook.json "https://share102.utsa.tech/story/addrbook.json"
```
