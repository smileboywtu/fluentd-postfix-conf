# fluentd postfix parser

fluentd(td-agent) config file for parse postfix mail log.
there are many way to parse postfix mail log, eg rsyslog, logstash,
but it's not easy to config with rsyslog and it's large memory usage 
of logstash.

# feature

support two kind of messages:

- inqueue message
- send out message
- json style result
- add tag and timestamp for result
- file buffer config

# requirement

you need to install fluentd and it's plugin to run this config

- td-agent
- fluentd-plugin-parser

run follow to install all:

``` shell
# unbuntu 14.04
curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-trusty-td-agent2.sh | sh
td-agent-gem  install fluent-plugin-parser
```

# keep care

if you want to run customer script or app, you need to care about the file privilege, usually
the file should be read by user `td-agent`.

# example

```json
{
    "host": "web-a",
    "process": "postfix/qmgr",
    "pid": "3366",
    "queue_id": "4B86560A5F",
    "from_address": "Anonym@ns.com",
    "size": "1980",
    "nrcpt": "1",
    "message": "queue active",
    "tag": "postfix.result",
    "timestamp": "1498626866"
} 
{
    "host": "web-a",
    "process": "postfix/smtp",
    "pid": "16673",
    "queue_id": "4B86560A5F",
    "to_address": "294789842@qq.com",
    "relay": "mx3.qq.com[112.90.83.115]:25",
    "delay": "1.8",
    "delays": "0.03/0/0.13/1.6",
    "dns": "2.0.0",
    "status": "sent",
    "message": "250 Ok: queued as ",
    "tag": "postfix.result",
    "timestamp": "1498626868"
}
```
