# 💻 Installation

{% hint style="info" %}
### **Features of Kira**

* Validators do not save the entire block history in order to save disk space. This task should only be performed by sentinel and archive nodes
* To participate in the test network, you must pass KYC and fill out a form. To join the set of validators, you need to install a node and find out the public address of the validator, which you will need to indicate in the form to get on the white list
* In KIRA, double signature is the only violation that is punishable. More details [here](https://ipfs.kira.network/ipfs/bafybeigwa4mtipb2kpncdd4rbumvgakdbdua74klwvodd2ft5dbldd3viq/kira-decentralized-slash-free.html) and [here](https://ipfs.kira.network/ipfs/bafybeigwa4mtipb2kpncdd4rbumvgakdbdua74klwvodd2ft5dbldd3viq/bc4f9a208d934a55b0fa0d092c97b561.html)
* There are no severe penalties for validators being disabled or temporarily unavailable. Nodes that go offline for an extended period of time will be deactivated and automatically removed from the consensus by the state machine. The network is designed to stop if ≥ 1/3 of the consensus nodes suddenly stop responding, but there should be no interruption to the network if individual nodes randomly leave and enter the consensus. As long as the number of validators does not fall below the min\_validators defined in the [network properties](https://docs.kira.network/), the chain will continue to generate new blocks
{% endhint %}

{% hint style="warning" %}
**IMPORTANT - install on a new server without other nodes. Automatic installation closes all ports for other nodes in Docker and reboots the server**
{% endhint %}

## Node installation

```shell
adduser kira
usermod -aG sudo kira
su kira
sudo -s
```

We enter the command - after which we are prompted to enter the current version. We can find the current version on github - [https://github.com/KiraCore/kira](https://github.com/KiraCore/kira)

```shell
cd /tmp && apt-get install -y wget && \
UBUNTU_AGENT="Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.85 Safari/537.36" && \
echo -ne "\033[1;32m\nPlease enter version of KM to install: \033[0m" && read V && \
wget --user-agent="$UBUNTU_AGENT" https://github.com/KiraCore/kira/releases/download/${V}/init.sh -O ./i.sh && \
chmod +x -v ./i.sh && ./i.sh --infra-src="$V" --init-mode="interactive" || echo -e "\033[1;31mERROR: KM setup failed, error code '$?'\033[0m"
```

![](<../../.gitbook/assets/image (44).png>)

**We agree to the terms and conditions - Y**

![](<../../.gitbook/assets/image (45).png>)

**After the initial installation we see a similar window. It provides a launch menu that allows you to configure the node, manage network parameters, upload snapshots and view the hash of the genesis file**

![](<../../.gitbook/assets/image (50).png>)

## Saving keys

During automatic installation, a mnemonic phrase is generated. Keys are saved along the path /home/kira/.secrets/

{% hint style="warning" %}
**IMPORTANT** - be sure to select the **\[M]** “View or Modify Mnemonic” option before exiting the “Node Launcher” menu and then press **\[V]** to display the main mnemonic. Write down your mnemonic phrase in a safe place
{% endhint %}

**MASTER\_MNEMONIC** is the only secret you will need to keep, since all other keys in KIRA are derived from it. More details [here](https://ipfs.kira.network/ipfs/bafybeihne7fpjrx2x7ofgu2nbfnyzsht4vnjgxhnzmv3pxxmuzllsn3nfa/9237f07cc51b48729299d00f5383901c.html)

## Setting network configuration

Setting up sshd\_config. If you are using a non-standard port for ssh, then select the **\[N]** option and then press **\[M]**. Skip this step if your ssh port is 22 (default)

## Adding Seed

It is extremely important to add the correct Peers to synchronize with the network. You can request an IP address from any of the other validators or use the addresses specified in the corresponding section of the test network - [https://ipfs.kira.network/ipfs/bafybeihne7fpjrx2x7ofgu2nbfnyzsht4vnjgxhnzmv3pxxmuzllsn3nfa/chaosnet-1.html](https://ipfs.kira.network/ipfs/bafybeihne7fpjrx2x7ofgu2nbfnyzsht4vnjgxhnzmv3pxxmuzllsn3nfa/chaosnet-1.html)

To add Peers, select option **\[A]**

![](<../../.gitbook/assets/image (47).png>)

Once we have received our Peers we can continue with the installation. To do this, select the **\[S]** option

You can follow the process by pressing **\[V]**

![](<../../.gitbook/assets/image (48).png>)

At the end of the node installation, your server will be automatically rebooted!!!

```shell
sudo -s
kira
```

You can follow the process by pressing **\[V]**. At the end of the installation you should see the following

![](<../../.gitbook/assets/image (49).png>)

Press "Q" or "Ctrl+C" to exit, then type `kira` to enter KIRA Manager

![](<../../.gitbook/assets/image (51).png>)

## Joining a validator set

To join the validator set, you first need to become a KIRA tester. Your initial goal will be to determine your public validator address, which will need to be provided on the [**FORM**](https://docs.google.com/forms/d/e/1FAIpQLScnCJcjlzIZwJ9ilts0u1lXSAZt5X9dplcsq8F9BrMkJgNVCA/viewform) in order to be whitelisted. Once the installation process is complete, you can easily find your validator address by entering kira in the terminal to open KIRA Manager. Then select option **\[0]** (\[0] by default) and copy your validator address

![](<../../.gitbook/assets/image (52).png>)

After filling out the form, you can write in telegram to speed up the process of adding to the whitelist Once the team whitelists you, you will be given the opportunity to select option **\[J]** to join the validator set in the KIRA Manager main menu. After executing this option, the node will begin to create and sign the proposed blocks

![](<../../.gitbook/assets/image (53).png>)

{% hint style="info" %}
NOTE: One of the most useful features is the maintenance mode. Whenever you need to update your node or move it or perform any other operations that may cause the validator to sign the process to stop, you can select option \[E] to enter maintenance mode. This will prevent the validator statistics from being reset and going into an inactive state. After completing the maintenance, selecting option **\[D]** will disable the maintenance mode
{% endhint %}
