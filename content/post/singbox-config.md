+++
date = '2025-05-14'
draft = false
title = 'singbox-config'
slug = '46e97813-059f-4cf1-bdd6-c4c3424d6e83'
+++
The below config is base on singbox `1.11.10`.I will use the latest featrues and abandon the outdated.
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
                "tag": "edu",
                "address": "udp://202.112.128.50"
            }
        ],
        "rules": [
            {
                "outbound": "any",
                "server": "edu"
            },
            {
                "rule_set": "geosite-ad",
                "action": "reject"
            },
            {
                "rule_set": [
                    "geosite-cn"
                ],
                "server": "edu"
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