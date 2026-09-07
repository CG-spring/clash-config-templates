# Clash Config Templates

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/CG-spring/clash-config-templates.svg?style=flat-square)](https://github.com/CG-spring/clash-config-templates/stargazers)

> Pre-built Clash configuration templates with rules and proxy groups — copy, paste and ready to use.

**中文** | **[English](README_EN.md)**

---

## Table of Contents

- [Basic Configuration](#basic-configuration)
- [Proxy Group Templates](#proxy-group-templates)
- [Routing Rule Templates](#routing-rule-templates)
- [DNS Configuration](#dns-configuration)
- [Complete Template](#complete-template)
- [Recommended Airports](#recommended-airports)
- [License](#license)

---

## Basic Configuration

### Minimal config

```yaml
mixed-port: 7890
allow-lan: false
mode: rule
log-level: info

proxies:
  # add your nodes here

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

## Proxy Group Templates

### Auto-select fastest node (url-test)

```yaml
proxy-groups:
  - name: Auto
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - Node-1
      - Node-2
      - Node-3
```

### Manual node selection (select)

```yaml
proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - Auto
      - HongKong
      - Japan
      - USA
      - DIRECT
```

### Load balancing (load-balance)

```yaml
proxy-groups:
  - name: LoadBalance
    type: load-balance
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - Node-1
      - Node-2
      - Node-3
```

### Group by region

```yaml
proxy-groups:
  - name: HongKong
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - HK-1
      - HK-2

  - name: Japan
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - JP-1
      - JP-2

  - name: USA
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      - US-1
      - US-2
```

---

## Routing Rule Templates

### Ad blocking

```yaml
rules:
  # ad domains
  - DOMAIN-SUFFIX,ad.com,REJECT
  - DOMAIN-SUFFIX,ads.com,REJECT
  - DOMAIN-KEYWORD,analytics,REJECT

  # common ad networks
  - DOMAIN-SUFFIX,doubleclick.net,REJECT
  - DOMAIN-SUFFIX,googlesyndication.com,REJECT
  - DOMAIN-SUFFIX,googleadservices.com,REJECT
```

### Streaming unblock

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

### AI tools

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

### Mainland China direct

```yaml
rules:
  # common domestic services
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  - DOMAIN-SUFFIX,weixin.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,jd.com,DIRECT

  # domestic IP
  - GEOIP,CN,DIRECT
```

---

## DNS Configuration

### Anti-pollution DNS (fake-ip)

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

## Complete Template

```yaml
# Full Clash configuration template
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
  # add your nodes here

proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - Auto
      - HongKong
      - Japan
      - USA
      - DIRECT

  - name: Auto
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    proxies:
      # add nodes

  - name: Streaming
    type: select
    proxies:
      - Proxy
      - HongKong
      - USA

rules:
  # streaming
  - DOMAIN-SUFFIX,netflix.com,Streaming
  - DOMAIN-SUFFIX,youtube.com,Streaming

  # AI tools
  - DOMAIN-SUFFIX,openai.com,Proxy
  - DOMAIN-SUFFIX,anthropic.com,Proxy

  # ad blocking
  - DOMAIN-KEYWORD,ads,REJECT

  # mainland direct
  - GEOIP,CN,DIRECT

  # default via proxy
  - MATCH,Proxy
```

---

## Recommended Airports

| Airport | Feature | Price | Link |
|---------|---------|-------|------|
| **ClashVIP** | Best value | from ¥15/mo | [Site](https://clashvip.net) |
| **ClashHub** | Dedicated lines | from ¥20/mo | [Site](https://clashhub.net) |
| **Airport Nav** | Multi-airport compare | Free | [Nav](https://nav.clashvip.net) |

---

## License

MIT License - 2026

<p align="center">
  <a href="https://clashvip.net">ClashVIP</a> |
  <a href="https://clashhub.net">ClashHub</a>
</p>
