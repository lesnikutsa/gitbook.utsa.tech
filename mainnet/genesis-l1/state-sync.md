# ⌚ State sync

{% hint style="info" %}
State sync allows a new node to join a network by fetching a snapshot of the network state at a recent height, instead of fetching and replaying all historical blocks
{% endhint %}

{% hint style="info" %}
Some blockchains do not yet know how to transfer wasm along with State sync. Download wasm if necessary
{% endhint %}

{% hint style="info" %}
Important - different blockchains need a different amount of RAM to successfully start with State sync. Average 4 GB RAM
{% endhint %}

## State sync

<mark style="color:red;">Currently, State sync on this node is not stable!!!</mark>

```shell
# stop the service and clear the database

```

```shell
# download wasm if necessary

```

<pre class="language-shell"><code class="lang-shell"># add peer
<strong>
</strong></code></pre>

```shell
```

{% hint style="info" %}
after echo <mark style="color:blue;">$LATEST\_HEIGHT $BLOCK\_HEIGHT $TRUST\_HASH</mark> you should see something like this

![](<../../.gitbook/assets/image (29).png>)
{% endhint %}

```shell
# restart node

```



