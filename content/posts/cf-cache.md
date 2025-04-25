---
date: '2025-04-20'
draft: false
title: cf cache 配置
slug: cf-cache
---
### 概述
cloudflare是全球最大的网络公司，提供cdn等服务，吸引用户的一大亮点是它提供不限流量的cdn。和其他先用后收费的公司相比，使用它的cdn不会造成天价账单。凭借它遍布全球的cdn节点，全球到它的距离都很短。我们部署网站可以使用cf的cdn。可以加快用户访问，减轻源服务器压力。cf可以配置强制缓存，我们可以通过这个方式实现静态全站缓存。我的博客网站为了实现稳定性，已经部署到了cf的pages上面，更方便使用它的cdn。配置缓存规则时，我喜欢用主机名来筛选，配置边缘缓存时间为1年，实现某个站点全站缓存，但是为了更新网站时可以清除缓存，我需要点击清除缓存，实在时不方便，于是，我看了cf api的文档，它提供了清除缓存的api，我们可以通过python和cmd写个脚本，更新网站后运行这个脚本就可以清除缓存了。
### 脚本内容
python负责主要逻辑。
```python
import os
from cloudflare import Cloudflare

client = Cloudflare(
    api_email="<账户邮箱>",
    api_key="<全局api>",
)
response = client.cache.purge(
    zone_id="<地区id>",
    hosts=["<域名>"]
)
print(response.id)
```
cmd负责点击运行
```cmd
python <python脚本位置>
pause
```