# ⚙️ Useful commands

**View logs**

```bash
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
docker compose logs -f validator
```

**Stop the node**

```bash
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
./stop.sh
```

**Restart the node**

```bash
cd ~/.canton/0.4.19/splice-node/docker-compose/validator
./start.sh -s "https://sv.sv-1.dev.global.canton.network.sync.global" -o "" -p "
```

**Check if IP supervalidators have accepted Devnet**

```bash
(set -o pipefail
CURL='curl -fsS -m 5 --connect-timeout 5'
for url in $($CURL https://scan.sv-1.dev.global.canton.network.sync.global/api/scan/v0/scans | jq -r '.scans[].scans[].publicUrl'); do
  echo -n "$url: "
  $CURL "$url"/api/scan/version | jq -r '.version'
done)
```

**Check if IP supervalidators have accepted Mainnet**

```bash
(set -o pipefail
CURL='curl -fsS -m 5 --connect-timeout 5'
for url in $($CURL https://scan.sv-1.global.canton.network.sync.global/api/scan/v0/scans | jq -r '.scans[].scans[].publicUrl'); do
  echo -n "$url: "
  $CURL "$url"/api/scan/version | jq -r '.version'
done)
```

