# 配置片段

这里存放了一些从 `list.yml` 中拆分出的配置片段，用于将本项目提供的一些配置整合到其它配置中。

# 文件说明

## Proxy Providers 规则集

- [nodes.yml](./nodes.yml)：节点列表，注意**不要**和下文的 `proxy.yml` 搞混了。
- [nodes_redir.yml](./nodes_redir.yml)：中转节点列表。
- nodes_地区码.yml：相应地区的节点列表，根据名称识别，不保证准确性，也不保证使用第三方服务时是否会被判断为国区。

## Rule Providers 规则集

- [adblock.yml](./adblock.yml)：广告屏蔽域名列表。
- [proxy.yml](./proxy.yml)：需要走代理的域名列表。
- [direct.yml](./direct.yml)：需要直连的域名列表。
- [region.yml](./region.yml)：存在锁区的域名列表。

注意：广告拦截列表中的域名不会出现在需要走代理的域名列表中，因此即使您没有使用广告屏蔽规则，仍有一些广告会无法加载。

# 配置示例

```yaml
proxy-providers:
  订阅地址:
    type: http
    url: "https://ghproxy.com/https://raw.githubusercontent.com/peasoft/NoMoreWalls/master/snippets/nodes.yml"
    interval: 3600
    path: ./proxy_providers/NoMoreWalls.yml
    health-check:
      enable: true
      interval: 600
      url: https://www.google.com/

rule-providers:
  adblock:
    type: http
    behavior: classical
    path: ./rule_providers/adblock.yml
    url: "https://ghproxy.com/https://raw.githubusercontent.com/peasoft/NoMoreWalls/master/snippets/adblock.yml"
    interval: 21600 #6h
    format: yaml
  proxy:
    type: http
    behavior: classical
    path: ./rule_providers/proxy.yml
    url: "https://ghproxy.com/https://raw.githubusercontent.com/peasoft/NoMoreWalls/master/snippets/proxy.yml"
    interval: 86400 #24h
    format: yaml
  direct:
    type: http
    behavior: classical
    path: ./rule_providers/direct.yml
    url: "https://ghproxy.com/https://raw.githubusercontent.com/peasoft/NoMoreWalls/master/snippets/direct.yml"
    interval: 86400 #24h
    format: yaml
  region:
    type: http
    behavior: classical
    path: ./rule_providers/region.yml
    url: "https://ghproxy.com/https://raw.githubusercontent.com/peasoft/NoMoreWalls/master/snippets/region.yml"
    interval: 86400 #24h
    format: yaml

rules:
  - DOMAIN-SUFFIX,googleapis.cn,🚀 选择代理 # 代理会自动切到国外，详情请见 README
  - DOMAIN-SUFFIX,xn--ngstr-lra8j.com,DIRECT # Google Play 国外/国内 服务器
  - DOMAIN-SUFFIX,xn--ngstr-cn-8za9o.com,DIRECT # Google Play 纯国内 服务器，尚未完成部署
  - DOMAIN-KEYWORD,kgithub,DIRECT
  - DOMAIN-KEYWORD,fastgit,DIRECT
  - DOMAIN-KEYWORD,ghproxy,DIRECT
  - RULE-SET,adblock,⛔ 广告拦截
  - DOMAIN-SUFFIX,cn,DIRECT
  - DOMAIN-KEYWORD,-cn,DIRECT
  - RULE-SET,region,🌐 突破锁区
  - RULE-SET,direct,DIRECT
  - GEOIP,CN,DIRECT
  - RULE-SET,proxy,🚀 选择代理
  - MATCH,🐟 漏网之鱼
```
