# Clash 配置模板合集

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/CG-spring/clash-config-templates.svg?style=flat-square)](https://github.com/CG-spring/clash-config-templates/stargazers)

> Clash 配置模板合集 - 预置规则、代理组、分流策略的完整配置模板
> 
> 直接复制使用，开箱即用

**中文** | **[English](README_EN.md)**

---

## 目录

- [基础配置](#基础配置)
- [代理组模板](#代理组模板)
- [分流规则模板](#分流规则模板)
- [DNS 配置](#dns-配置)
- [完整模板](#完整模板)

---

## 基础配置

### 最小配置

```yaml
mixed-port: 7890
allow-lan: false
mode: rule
log-level: info

proxies:
  # 在此添加你的节点

proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - DIRECT

rules:
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

---

## 代理组模板

### 自动选择最快节点

```yaml
proxy-groups:
  - name: Auto
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - 节点1
      - 节点2
      - 节点3
```

### 手动选择节点

```yaml
proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - Auto
      - 香港节点
      - 日本节点
      - 美国节点
      - DIRECT
```

### 负载均衡

```yaml
proxy-groups:
  - name: LoadBalance
    type: load-balance
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - 节点1
      - 节点2
      - 节点3
```

### 按地区分组

```yaml
proxy-groups:
  - name: 香港
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - HK-1
      - HK-2

  - name: 日本
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - JP-1
      - JP-2

  - name: 美国
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - US-1
      - US-2
```

---

## 分流规则模板

### 广告拦截

```yaml
rules:
  # 广告域名
  - DOMAIN-SUFFIX,ad.com,REJECT
  - DOMAIN-SUFFIX,ads.com,REJECT
  - DOMAIN-KEYWORD,analytics,REJECT
  
  # 常见广告
  - DOMAIN-SUFFIX,doubleclick.net,REJECT
  - DOMAIN-SUFFIX,googlesyndication.com,REJECT
  - DOMAIN-SUFFIX,googleadservices.com,REJECT
```

### 流媒体解锁

```yaml
rules:
  # Netflix
  - DOMAIN-SUFFIX,netflix.com,Proxy
  - DOMAIN-SUFFIX,nflxvideo.net,Proxy
  - DOMAIN-SUFFIX,nflxso.net,Proxy
  
  # YouTube
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-SUFFIX,googlevideo.com,Proxy
  - DOMAIN-SUFFIX,ytimg.com,Proxy
  
  # Disney+
  - DOMAIN-SUFFIX,disneyplus.com,Proxy
  - DOMAIN-SUFFIX,disney-plus.net,Proxy
  
  # HBO Max
  - DOMAIN-SUFFIX,hbomax.com,Proxy
  - DOMAIN-SUFFIX,hbonow.com,Proxy
```

### AI 工具

```yaml
rules:
  # ChatGPT
  - DOMAIN-SUFFIX,openai.com,Proxy
  - DOMAIN-SUFFIX,chatgpt.com,Proxy
  - DOMAIN-SUFFIX,ai.com,Proxy
  
  # Claude
  - DOMAIN-SUFFIX,anthropic.com,Proxy
  
  # Gemini
  - DOMAIN-SUFFIX,gemini.google.com,Proxy
```

### 国内直连

```yaml
rules:
  # 常用国内服务
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  - DOMAIN-SUFFIX,weixin.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,jd.com,DIRECT
  
  # 国内 IP
  - GEOIP,CN,DIRECT
```

---

## DNS 配置

### 防污染 DNS

```yaml
dns:
  enable: true
  ipv6: false
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
    - '*.lan'
    - localhost.ptlogin2.qq.com
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1
  fallback-filter:
    geoip: true
    geoip-code: CN
```

---

## 完整模板

```yaml
# Clash 完整配置模板
mixed-port: 7890
allow-lan: false
mode: rule
log-level: info

dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1

proxies:
  # 添加你的节点

proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - Auto
      - 香港
      - 日本
      - 美国
      - DIRECT

  - name: Auto
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      # 添加节点

  - name: 流媒体
    type: select
    proxies:
      - Proxy
      - 香港
      - 美国

rules:
  # 流媒体
  - DOMAIN-SUFFIX,netflix.com,流媒体
  - DOMAIN-SUFFIX,youtube.com,流媒体
  
  # AI 工具
  - DOMAIN-SUFFIX,openai.com,Proxy
  - DOMAIN-SUFFIX,anthropic.com,Proxy
  
  # 广告拦截
  - DOMAIN-KEYWORD,ads,REJECT
  
  # 国内直连
  - GEOIP,CN,DIRECT
  
  # 其他走代理
  - MATCH,Proxy
```

---

## 推荐机场

| 机场 | 特点 | 价格 | 链接 |
|------|------|------|------|
| **ClashVIP** | 高性价比 | ¥15/月起 | [官网](https://clashvip.net) |
| **ClashHub** | 专线优化 | ¥20/月起 | [官网](https://clashhub.net) |
| **机场导航** | 多机场对比 | 免费 | [导航](https://nav.clashvip.net) |

---

## License

MIT License - 2026

<p align="center">
  <a href="https://clashvip.net">ClashVIP</a> |
  <a href="https://clashhub.net">ClashHub</a>
</p>