---
layout: post
title: "Traffic Control Notes"
date: 2017-01-18
---

## ingress police rate example
```shell
tc qdisc add dev p4p1 handle ffff: ingress
tc filter add dev p4p1 parent ffff: protocol ip u32 match ip src 0.0.0.0/0 flowid :1 police rate 20.0mbit mtu 10000 burst 100k drop
tc qdisc del dev p4p1 ingress
```

## References

https://serverfault.com/a/386791/412589
