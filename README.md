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

test postfix version is `2.9.6-1~12.04.3`

``` shell
apt-get install postfix=2.9.6-1~12.04.3
```

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
    "logtype": "postfix.result",
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
    "logtype": "postfix.result",
    "timestamp": "1498626868"
}
```

# DSN status Lookup

check DSN status:


The syntax of the new status codes is defined as:
status-code = class "." subject "." detail 
class = "2"/"4"/"5" 
subject = 1*3digit 
detail = 1*3digit

##Collected Status Codes

###First Digit

| status | description |
|---     |---          |
|2.X.X |Success|
|4.X.X |Persistent Transient Failure|
|5.X.X |Permanent Failure|


###Second and Third Digits

| status | description |
|---     |---          |
|X.1.0 |Other address status                           |
|X.1.1 |Bad destination mailbox address                |
|X.1.2 |Bad destination system address                 |
|X.1.3 |Bad destination mailbox address syntax         |
|X.1.4 |Destination mailbox address ambiguous          |
|X.1.5 |Destination mailbox address valid              |
|X.1.6 |Mailbox has moved                              |
|X.1.7 |Bad sender's mailbox address syntax            |
|X.1.8 |Bad sender's system address                    |
|X.2.0 |Other or undefined mailbox status              |
|X.2.1 |Mailbox disabled, not accepting messages       |
|X.2.2 |Mailbox full                                   |
|X.2.3 |Message length exceeds administrative limit    |
|X.2.4 |Mailing list expansion problem                 |
|X.3.0 |Other or undefined mail system status          |
|X.3.1 |Mail system full                               |
|X.3.2 |System not accepting network messages          |
|X.3.3 |System not capable of selected features        |
|X.3.4 |Message too big for system                     |
|X.4.0 |Other or undefined network or routing status   |
|X.4.1 |No answer from host                            |
|X.4.2 |Bad connection                                 |
|X.4.3 |Routing server failure                         |
|X.4.4 |Unable to route                                |
|X.4.5 |Network congestion                             |
|X.4.6 |Routing loop detected                          |
|X.4.7 |Delivery time expired                          |
|X.5.0 |Other or undefined protocol status             |
|X.5.1 |Invalid command                                |
|X.5.2 |Syntax error                                   |
|X.5.3 |Too many recipients                            |
|X.5.4 |Invalid command arguments                      |
|X.5.5 |Wrong protocol version                         |
|X.6.0 |Other or undefined media error                 |
|X.6.1 |Media not supported                            |
|X.6.2 |Conversion required and prohibited             |
|X.6.3 |Conversion required but not supported          |
|X.6.4 |Conversion with loss performed                 |
|X.6.5 |Conversion failed                              |
|X.7.0 |Other or undefined security status             |
|X.7.1 |Delivery not authorized, message refused       |
|X.7.2 |Mailing list expansion prohibited              |
|X.7.3 |Security conversion required but not possible  |
|X.7.4 |Security features not supported                |
|X.7.5 |Cryptographic failure                          |
|X.7.6 |Cryptographic algorithm not supported          |
|X.7.7 |Message integrity failure                      |

# about postfix log

more docs about log filed information:

- [uderstand delays](https://serverfault.com/questions/24121/understanding-a-postfix-log-file-entry)
- [DSN status](https://tools.ietf.org/html/rfc3463)
