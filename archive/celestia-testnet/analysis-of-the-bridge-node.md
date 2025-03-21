# 🔎 Analysis of the Bridge Node

This guide will analyze the load of servers on which Bridge Node Celestia and Consensus Node will be installed. In order to run a bridge node, you need to use public RPCs, or run Consensus Node yourself to receive RPCs. It’s worth mentioning right away that the team recommended using **pruning = "nothing"** for Consensus Node , but we decided to test various configurations&#x20;

For our test, we took 4 VPS on Hetzner in the same location (Finland) - CPX41 8/16GB/240GB Accordingly, we will ru 4 Bridge nodes and 4 Consensus Nodes on 4 servers. Basic settings and installation are described in this article. The settings of each server will be slightly different and in our case have the following config:

{% hint style="info" %}


**Server1:**

* pruning = "default"
* indexer = "null"

**Server2:**

* pruning = "custom" 1000/10
* indexer = "null"

**Server3:**

* pruning = "nothing"
* indexer = "null"

**Server4:**

* pruning = "default"
* indexer = "kv"
{% endhint %}

**We will measure the following parameters:**

* Uptime at https://tiascan.com/bridge-nodes
* Used hard disk space with `df -m /dev/sda1`
* Network traffic
* Load average with htop

Measurements will be taken immediately after synchronization, 2 hours after synchronization, 24 hours after synchronization, and 48 hours after synchronization. All measurements will be reflected in this Google [spreadsheet](https://docs.google.com/spreadsheets/d/1tPVw8N5t8tGc-GcevYaCds-b7v-frQ_LFtO_iBd9FGM/edit#gid=0) and also in the screenshots

**Main results**

![](<../../.gitbook/assets/image (37).png>)

**Network traffic results during Bridge run**

![](<../../.gitbook/assets/image (26).png>)

**Results of used hard disk space**

![](<../../.gitbook/assets/image (36).png>)

**Size comparison of .celestia-app and .celestia-bridge-blockspacerace-0 directories**

![](<../../.gitbook/assets/image (30).png>)

### **Conclusions**

* **The network load** on all 4 servers is approximately the same. And the difference that we see does not give us reason to pay attention to it
* **Uptime** on all 4 nodes also does not vary much and we see that all 4 nodes with different configurations stay within **99.34% - 99.36%**, which is quite normal for these VPS
* **Load average** showed us the main load during the node synchronization period. But with a synchronized node, we see almost the same load on the servers. Only the server with Pruning=custom 1000/10 has a higher load compared to the other 3 servers
* **The main difference is when measuring the occupied space on hard drives**. Server3 with Pruning=nothing showed great results in saving hard disk space, although before the test I thought it would be the other way around. So, the difference between server3 occupied space was more than 2 times compared to other servers on which Pruning was configured differently. So, from the screenshots above, we see that the .celestia-bridge-blockspacerace-0 directory on server 3 takes only **6+GB** instead of **80+** on other servers

Summing up, we can conclude that during installation we do not have to pay attention to indexing and network consumption - and most importantly, set **Pruning=nothing**
