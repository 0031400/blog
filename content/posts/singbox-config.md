+++
date = '2025-05-14'
draft = false
title = 'singbox-config'
slug = '46e97813-059f-4cf1-bdd6-c4c3424d6e83'
cover = 'https://i.withme.top/2025/05/14/46e97813-059f-4cf1-bdd6-c4c3424d6e83.webp'
+++
The below config is base on singbox `1.11.10`.I will use the latest featrues and abandon the outdated.  
The base config file is like this.
```json
{
  "log": {},
  "dns": {},
  "ntp": {},
  "certificate": {},
  "endpoints": [],
  "inbounds": [],
  "outbounds": [],
  "route": {},
  "experimental": {}
}
```
Now,we write the `log`.  
```json
{
    "disabled": false,
    "level": "info",
    "timestamp": true
}
```
When I write the config file,I use the `debug` level.When I use the application,I use the `warn` level.
Now we write the `inbounds`
```json
[
    {
        "type": "tun",
        "tag": "tun-in",
        "address": [
            "172.18.0.1/30",
            "fdfe:dcba:9876::1/126"
        ],
        "interface_name": "sbox",
        "auto_route": true,
        "strict_route": true
    }
]
```
I like to use the tun.And when the tun does't work,I will use the system proxy.Now I write the tun config,and the proxy config will be later.  
The `address` is the ip which singbox virtual network card gives you.`auto_route` is responsible for change the system route table.If the does't write the field,the virtual network card is just set up but don't change your network.`strict_route` can block the network traffic which the singbox can't solve.I think it is useful.  
Now let us write the `outbounds`
```json
[
    {
        "type": "direct",
        "tag": "direct-out"
    },
    {
        "type": "vless",
        "tag": "vless-tls",
        "server": "666.666.666.666",
        "server_port": 6666,
        "uuid": "66666666-6666-6666-6666-666666666666",
        "tls": {
            "enabled": true,
            "server_name": "666.com"
        },
        "transport": {
            "type": "ws",
            "path": "/66666666"
        }
    },
    {
        "type": "urltest",
        "tag": "urltest",
        "outbounds": [
            "vless-tls"
        ],
        "interval": "1m",
        "interrupt_exist_connections": true
    }
]
```
I am customed to using many proxy nodes and gather all of them to a `urltest` node.And I add a `direct` outbound additionally for the direct out.  
Now we write the `route`  
I firstly write the `rules`  
We need to resolve the dns query specially so we need the below.
```json
{
    "protocol": "dns",
    "action": "hijack-dns"
}
```
We need to distinguish the dns traffic from all of the network traffic so we need the sniff.
```json
{
    "inbound": [
        "tun-in"
    ],
    "action": "sniff"
}
```
And it should be the first.  
Then I will guide all of the network to my proxy node temporarily before I solve the traffic division.
```json
{
    "inbound": [
        "tun-in"
    ],
    "outbound": "urltest"
}
```
The `rules` is finished temporarily.Now we write other field of the `route`
```json
{
    "auto_detect_interface": true,
    "rules": []
}
```
The official guide says `auto_detect_interface` is useful so I add it.  
I need to set the traffic to China is direct out.So I need to use some data to kown where the domains and ip are.So I write the `rule_set`
```json
[
    {
        "tag": "geosite-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/SagerNet/sing-geosite/rule-set/geosite-cn.srs"
    },
    {
        "tag": "geoip-cn",
        "type": "remote",
        "format": "binary",
        "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/SagerNet/sing-geoip/rule-set/geoip-cn.srs"
    },
    {
        "tag": "geosite-ad",
        "type": "remote",
        "format": "binary",
        "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/REIJI007/AdBlock_Rule_For_Sing-box/main/adblock_reject.srs"
    }
]
```
I use one proxydown website.But it may not work always so you should deal with it yourself.  
Now I complete the `rules`
```json
[
    {
        "ip_is_private": true,
        "outbound": "direct-out"
    },
    {
        "rule_set": "geosite-ad",
        "action": "reject"
    },
    {
        "rule_set": "geoip-cn",
        "outbound": "direct-out"
    },
    {
        "rule_set": "geosite-cn",
        "outbound": "direct-out"
    }
]
```
Now I will write the `dns`  
I love the use the dns server the dhcp gives me.But it may give me wrong dns of some websites like google.So I need some tls dns server.  
`servers`
```json
[
    {
        "tag": "cf",
        "address": "tls://1.1.1.1"
    },
    {
        "tag": "dhcp",
        "address": "dhcp://auto"
    }
]
```
`rules`
```json
[
    {
        "outbound": "any",
        "server": "dhcp"
    },
    {
        "rule_set": "geosite-ad",
        "action": "reject"
    },
    {
        "rule_set": [
            "geosite-cn"
        ],
        "server": "dhcp"
    },
    {
        "inbound": [
            "tun-in"
        ],
        "server": "cf"
    }
]
```
I prefer the ipv6 so I set this
```json
{
    "strategy": "prefer_ipv6"
}
```
The rest is the `experimental` ,they are easy to understand.
```json
{
    "cache_file": {
        "enabled": true
    },
    "clash_api": {
        "external_controller": "127.0.0.1:9090",
        "external_ui_download_url": "https://proxydown.0031400.xyz?url=https://github.com/MetaCubeX/Yacd-meta/archive/gh-pages.zip"
    }
}
```
So all of the config is below.
```json
{
    "log": {
        "disabled": false,
        "level": "info",
        "timestamp": true
    },
    "dns": {
        "strategy": "prefer_ipv6",
        "servers": [
            {
                "tag": "cf",
                "address": "tls://1.1.1.1"
            },
            {
                "tag": "dhcp",
                "address": "dhcp://auto"
            }
        ],
        "rules": [
            {
                "outbound": "any",
                "server": "dhcp"
            },
            {
                "rule_set": "geosite-ad",
                "action": "reject"
            },
            {
                "rule_set": [
                    "geosite-cn"
                ],
                "server": "dhcp"
            },
            {
                "inbound": [
                    "tun-in"
                ],
                "server": "cf"
            }
        ]
    },
    "inbounds": [
        {
            "type": "tun",
            "tag": "tun-in",
            "address": [
                "172.18.0.1/30",
                "fdfe:dcba:9876::1/126"
            ],
            "interface_name": "sbox",
            "auto_route": true,
            "strict_route": true
        }
    ],
    "outbounds": [
        {
            "type": "direct",
            "tag": "direct-out"
        },
        {
            "type": "vless",
            "tag": "vless-tls",
            "server": "666.666.666.666",
            "server_port": 6666,
            "uuid": "66666666-6666-6666-6666-666666666666",
            "tls": {
                "enabled": true,
                "server_name": "666.com"
            },
            "transport": {
                "type": "ws",
                "path": "/66666666"
            }
        },
        {
            "type": "urltest",
            "tag": "urltest",
            "outbounds": [
                "vless-tls"
            ],
            "interval": "1m",
            "interrupt_exist_connections": true
        }
    ],
    "route": {
        "auto_detect_interface": true,
        "rules": [
            {
                "inbound": [
                    "tun-in"
                ],
                "action": "sniff"
            },
            {
                "protocol": "dns",
                "action": "hijack-dns"
            },
            {
                "ip_is_private": true,
                "outbound": "direct-out"
            },
            {
                "rule_set": "geosite-ad",
                "action": "reject"
            },
            {
                "rule_set": "geoip-cn",
                "outbound": "direct-out"
            },
            {
                "rule_set": "geosite-cn",
                "outbound": "direct-out"
            },
            {
                "inbound": [
                    "tun-in"
                ],
                "outbound": "urltest"
            }
        ],
        "rule_set": [
            {
                "tag": "geosite-cn",
                "type": "remote",
                "format": "binary",
                "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/SagerNet/sing-geosite/rule-set/geosite-cn.srs"
            },
            {
                "tag": "geoip-cn",
                "type": "remote",
                "format": "binary",
                "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/SagerNet/sing-geoip/rule-set/geoip-cn.srs"
            },
            {
                "tag": "geosite-ad",
                "type": "remote",
                "format": "binary",
                "url": "https://proxydown.0031400.xyz?url=https://raw.githubusercontent.com/REIJI007/AdBlock_Rule_For_Sing-box/main/adblock_reject.srs"
            }
        ]
    },
    "experimental": {
        "cache_file": {
            "enabled": true
        },
        "clash_api": {
            "external_controller": "127.0.0.1:9090",
            "external_ui_download_url": "https://proxydown.0031400.xyz?url=https://github.com/MetaCubeX/Yacd-meta/archive/gh-pages.zip"
        }
    }
}
```