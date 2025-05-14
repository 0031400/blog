+++
date = '2025-05-10'
draft = false
title = 'mihomo-config'
slug = '5d5659f9-b709-4179-8a9a-b00ce89aa9c7'
cover = 'https://i.withme.top/2025/05/10/5d5659f9-b709-4179-8a9a-b00ce89aa9c7.webp'
+++
```yaml
log-level: debug
tun:
  enable: true
  auto-route: true
  auto-detect-interface: true
  strict-route: true
profile:
  store-selected: true
external-controller: 127.0.0.1:9090
external-ui-url: "https://proxydown.0031400.xyz?url=https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
geodata-mode: true
geox-url:
  geoip: "https://proxydown.0031400.xyz?url=https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.dat"
  geosite: "https//proxydown.0031400.xyz?url=https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geosite.dat"
dns:
  enable: true
  ipv6: true
  nameserver:
  - dhcp://system
proxies:
# your proxy nodes
proxy-groups:
- name: fallback
  type: fallback
  proxies:
# your proxy nodes
rules:
    - IP-CIDR,10.0.0.0/8,DIRECT
    - IP-CIDR,172.16.0.0/12,DIRECT
    - IP-CIDR,192.168.0.0/16,DIRECT
    - GEOSITE,category-ads-all,REJECT
    - GEOSITE,CN,DIRECT
    - GEOIP,CN,DIRECT
    - MATCH,fallback
```