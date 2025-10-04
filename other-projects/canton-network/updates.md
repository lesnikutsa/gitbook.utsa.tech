# ⌚ Updates

{% hint style="warning" %}
Important:

\
Be sure to make backups before updating
{% endhint %}

```shell
# stop the node
cd ~/.canton/0.4.18/splice-node/docker-compose/validator
./stop.sh

# create a separate directory
mkdir -p ~/.canton/0.4.19
cd ~/.canton/0.4.19

# download the archive
wget https://github.com/digital-asset/decentralized-canton-sync/releases/download/v0.4.19/0.4.19_splice-node.tar.gz
tar xzvf 0.4.19_splice-node.tar.gz
cd ~/.canton/0.4.19/splice-node/docker-compose/validator

# export version!!!
export IMAGE_TAG=0.4.19

# запускаем - -o "" оставляем пустыми (без токена), так как он вводится только 1 раз
# ./start.sh -s "<SPONSOR_SV_URL>" -o "<ONBOARDING_SECRET>" -p "<party_hint>" -m "<MIGRATION_ID>" -w

# view logs
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
docker compose logs -f validator
```

