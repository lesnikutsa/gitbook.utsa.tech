# ⚒️ TenderDuty v2 - monitoring and alerting

![](<../.gitbook/assets/image (27).png>)

Tenderduty is a comprehensive monitoring tool for Tendermint networks. More details can be found here - [https://github.com/blockpane/tenderduty](https://github.com/blockpane/tenderduty)

This monitoring of TenderDuty v2 allows you to control the nodes and, in particular, see the height of the network, the status of the validator, uptime, signed and skipped blocks. It is also possible to connect notifications to telegrams and discord

Installation is possible in various ways, but I will use installation via Docker, although there is no fundamental difference So, we need a separate server (which definitely gives a security plus) or a server with an already installed node (nodes). You will also need to find open RPCs or open your own on the main (not desirable) or backup node

## Install

```bash
apt update && sudo apt upgrade -y
apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

```bash
# install docker
apt update && \
apt install apt-transport-https ca-certificates curl software-properties-common -y && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
apt update && \
apt-cache policy docker-ce && \
sudo apt install docker-ce -y && \
docker --version
```

```bash
# install tenderduty
tmux new-session -s tenderduty

mkdir tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```

```bash
# Now you can download the config and edit it (Russian language)
wget -O $HOME/tenderduty/config.yml "https://raw.githubusercontent.com/lesnikutsa/lesnik_utsa/main/monitoring/TenderDuty(ru)/config.yml"
nano $HOME/tenderduty/config.yml
```

{% hint style="info" %}
For simple monitoring without notifications, just change in the config: network name; chain-id; valoper\_address; url
{% endhint %}

**Example config file**

```bash
---

# определяет, включена ли панель мониторинга
enable_dashboard: yes

# Какой TCP-порт будет прослушивать панель мониторинга
listen_port: 8888

# hide_log полезен, если панель мониторинга будет опубликована публично. Он отключает канал журнала,
# и скрывает большинство деталей, связанных с узлом. Имейте в виду, что это не полностью проверено на предмет предотвращения
# утечки информации об именах узлов и т.д.
hide_logs: no

# Сколько времени нужно подождать, прежде чем оповестить о том, что узел не работает
node_down_alert_minutes: 3

# Должен ли быть включен экспортер prometheus?
prometheus_enabled: yes
# Какой порт он должен прослушивать?
prometheus_listen_port: 28686

# Глобальная настройка
pagerduty:
  # Должны ли мы использовать PD? Имейте в виду, что если для этого параметра установлено значение no, это переопределяет отдельные настройки оповещения цепочки.
  enabled: no
  # Это ключ API, а не токен oauth, более подробная информация приведена ниже, но для получения дополнительной информации ознакомьтесь с документами v1.
  api_key: aaaaaaaaaaaabbbbbbbbbbbbbcccccccccccc
  # В настоящее время не используется, но скоро будет использоваться. Это позволяет устанавливать приоритеты эскалации и т.д.
  default_severity: alert

# Настройки Discord
discord:
  # Оповещения в discord?
  enabled: no
  # Webhook настраивается щелчком правой кнопки мыши на канале, редактированием настроек и настройкой webhook в разделе интеграции.
  webhook: https://discord.com/api/webhooks/999999999999999999/zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz

# Настройки Telegram
telegram:
  # Оповещение в Telegram? Примечание: также заменяет настройки, относящиеся к конкретной сети
  enabled: no
  # Ключ API ... поговорите с @BotFather
  api_key: '5555555555:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'
  # ID группы чата, в который будут отправляться сообщения
  channel: "-666666666"

# Различные цепочки, подлежащие мониторингу. Создайте новую запись для каждой сети
chains:

  # Удобное для пользователя имя, которое будет использоваться для меток. Настоятельно рекомендую заключить в кавычки
  "aura":
    # chain_id проверяется на соответствие при подключении к конечной точке RPC, также используется в качестве метки в нескольких местах
    chain_id: euphoria-1
    # Ура, в версии v2 мы выводим valcons из запросов abci, так что вам не придется прыгать через обручи, чтобы выяснить, как преобразовывать ключи ed25519 в соответствующий адрес bech32
    valoper_address: auravaloper10...
    # Должен ли мониторинг вернуться к использованию общедоступных конечных точек API, если все предоставленные узлы RCP выйдут из строя?
    # Это не всегда надежно, не все общедоступные узлы имеют правильную настройку прокси-сервера websocket.
    public_fallback: yes

    # Управляет различными настройками оповещений для каждой сети
    alerts:
      # Если сеть перестает видеть новые блоки, следует ли отправлять предупреждение?
      stalled_enabled: yes
      # Сколько времени требуется остановленной сети в минутах, чтобы сгенерировать сигнал тревоги
      stalled_minutes: 10

      # Самый простой сигнал тревоги, вы только что пропустили x блоков... хотели бы вы знать?
      consecutive_enabled: yes
      # Сколько пропущенных блоков должно вызвать уведомление
      consecutive_missed: 5
      # НЕ ИСПОЛЬЗУЕТСЯ: будущая подсказка для маршрутизации pagerduty
      consecutive_priority: critical

      # Для каждой сети существует определенное окно блоков и процент пропущенных блоков, которые приведут к тюрьме
      # Следует ли отправлять предупреждение, если превышен определенный процент этого окна?
      percentage_enabled: no
      # Какой процент должен вызвать оповещение
      percentage_missed: 10
      # Еще не используется, подсказка о маршрутизации pagerduty
      percentage_priority: warning

      # Должно ли быть отправлено предупреждение, если валидатор не находится в активном наборе, т.е. заключен в тюрьму, tombstoned, не привязан?
      alert_if_inactive: yes
      # Следует ли отправлять предупреждение, если ни один RPC-сервер не отвечает? (Обратите внимание, что этот сигнал тревоги подается мгновенно без задержки)
      alert_if_no_servers: yes

      # для этой *конкретной* сети можно переопределить настройки оповещений. Если адреса api_key или webhook пусты,
      # то будут использоваться глобальные настройки. Обратите внимание, что включено должно быть установлено как глобально, так и для каждой цепочки

      # Настройка для конкретной цепочки для pagerduty
      pagerduty:
        enabled: yes
        api_key: "" # используется по умолчанию, если пусто

      # Настройки Discord
      discord:
        enabled: yes
        webhook: "" # используется по умолчанию, если пусто

      # Настройки Telegram
      telegram:
        enabled: yes
        api_key: "" # используется по умолчанию, если пусто
        channel: "" # используется по умолчанию, если пусто

    # В этом разделе рассматриваются наши поставщики RPC. Конечные точки LCD (они же REST) не используются, только конечные точки RPC
    # Рекомендуется использовать несколько хостов, и они будут опробованы последовательно, пока не будет обнаружена рабочая RPC
    nodes:
      # URL-адрес конечной точки. Должен включать protocol://hostname:port
      - url: https://snapshot-1.euphoria.aura.network:443
        # Должны ли мы отправить предупреждение, если этот хост не отвечает?
        alert_if_down: yes
      # повторные хосты для контроля избыточности
#      - url: 
#        alert_if_down: no
#      - url: 
#        alert_if_down: no

################################################################################
# Далее можно добавить вторую и последующие сети... Раскомментируйте и измените#
################################################################################
#chains:
#  "L1":
#    chain_id: genesis_29-2
#    valoper_address: genesisvaloper1...
#    public_fallback: yes

#    alerts:
#      stalled_enabled: yes
#      stalled_minutes: 10
#      consecutive_enabled: yes
#      consecutive_missed: 5
#      consecutive_priority: critical
#      percentage_enabled: no
#      percentage_missed: 10
#      percentage_priority: warning
#      alert_if_inactive: yes
#      alert_if_no_servers: yes

#      pagerduty:
#        enabled: yes
#        api_key: "" # используется по умолчанию, если пусто

#      discord:
#        enabled: yes
#        webhook: "" # используется по умолчанию, если пусто

#      telegram:
#        enabled: yes
#        api_key: "" # используется по умолчанию, если пусто
#        channel: "" # используется по умолчанию, если пусто

#    nodes:
#      - url: 
#        alert_if_down: yes
#      - url: 
#        alert_if_down: no
#      - url: 
#        alert_if_down: no
#      - url: 
#        alert_if_down: no
```

**After setting up the config, run**

```bash
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest

# logs
docker logs -f --tail 20 tenderduty
```

![](<../.gitbook/assets/image (38).png>)

**Now is the time to check the information in the browser**

```bash
# find out the address and paste it into the browser
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"
# http://108.108.108.108:8888/
```

## Discord setup

It's pretty easy to turn on notifications for Discord. To do this, you need to perform just a few steps

![](<../.gitbook/assets/image (40).png>)

![](<../.gitbook/assets/image (22).png>)

![](<../.gitbook/assets/image (35).png>)

![](<../.gitbook/assets/image (34).png>)

![](<../.gitbook/assets/image (24).png>)

![](<../.gitbook/assets/image (42).png>)

![](<../.gitbook/assets/image (41).png>)

![](<../.gitbook/assets/image (25).png>)

After all these manipulations, we restart the monitoring and the alarms will come to the discord!

![](<../.gitbook/assets/image (33).png>)
