# EMQX

## Some useful HTTP API

```bash
# To publish a topic
# You can give more option like client ID or...
curl -i --basic -u admin:public -X POST "http://194.5.193.113:8081/api/v4/mqtt/publish" -d '{"topic":"led_right","payload":"Hello World"}'
```