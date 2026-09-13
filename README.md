# FreeProxyHub 🚀

> 免费代理 IP 聚合平台
> 自动采集公开代理节点，统一整理、去重并生成多种订阅格式。


\

**FreeProxyHub** 是一个基于 GitHub Actions + Python + 静态页面构建的免费代理节点聚合项目。

项目会定期从 **VPN Gate** 以及多个 GitHub 开源代理项目中采集公开节点，经过基础清洗、去重和整理后，生成：

* Clash / Mihomo 订阅
* v2rayNG / Shadowrocket 等客户端可使用的 Base64 订阅
* HTTP / SOCKS 纯文本代理列表
* JSON 原始数据
* 节点统计信息

项目无需独立数据库，生成的数据直接存放在仓库 `data/` 目录中，前端页面读取静态数据即可展示。

---

## ✨ 项目特点

### 🌐 多来源聚合

目前主要从以下公开来源获取节点：

* [VPN Gate](https://www.vpngate.net/)
* `gfpcom/free-proxy-list`
* `monosans/proxy-list`
* `roosterkid/openproxylist`

采集器同时支持多种代理协议及节点格式，包括：

* HTTP
* SOCKS4
* SOCKS5
* VMess
* VLESS
* Trojan
* Shadowsocks
* VPN Gate / OpenVPN 数据

具体来源和协议以当前采集器配置为准。

---

### 🔄 自动采集与更新

项目通过自动化任务运行 Python 采集器：

```text
公开代理源
    ↓
数据采集
    ↓
格式解析
    ↓
节点清洗
    ↓
去重
    ↓
数据统计
    ↓
生成订阅
    ↓
写入 data/
    ↓
静态页面展示
```

采集器会将所有来源的数据统一转换成内部结构，并使用：

```text
IP + Port + Protocol
```

作为去重依据，减少重复节点。

---

## 📦 输出内容

运行采集器后，数据会输出到：

```text
data/
├── proxies.json
├── stats.json
├── meta.json
├── clash.yaml
├── subscribe.txt
├── subscribe_base64.txt
└── proxies.txt
```

### `proxies.json`

完整代理节点数据，适合开发者进行二次处理。

示例：

```json
{
  "ip": "1.2.3.4",
  "port": 443,
  "protocol": "vless",
  "source": "gfpcom"
}
```

---

### `clash.yaml`

Clash / Mihomo 格式订阅。

当前生成逻辑默认最多输出前 **300 个节点**，并根据节点信息生成 HTTP、SOCKS、VMess、VLESS、Trojan、SS 等配置。

同时内置：

* 自动选择
* 故障切换
* 手动选择

等策略组。

---

### `subscribe.txt`

节点 URI 列表，包含支持生成的：

```text
vmess://
vless://
trojan://
ss://
```

等格式。

---

### `subscribe_base64.txt`

对 `subscribe.txt` 进行 Base64 编码后的订阅内容，可用于支持 Base64 订阅格式的客户端。

---

### `proxies.txt`

纯文本代理列表，主要包含：

```text
http://IP:PORT
https://IP:PORT
socks4://IP:PORT
socks5://IP:PORT
```

当前默认最多输出前 **300 个节点**。

---

### `stats.json`

节点统计数据，包括：

* 节点总数
* 更新时间
* 协议分布
* 数据来源分布
* 抗干扰等级统计

例如：

```json
{
  "total": 1234,
  "updated_at": "2026-01-01T00:00:00Z",
  "protocols": {
    "vless": 300,
    "vmess": 200,
    "socks5": 500
  }
}
```

---

## 🖥️ Web 页面

项目自带一个纯静态 Web 页面：

* 仪表盘
* 代理列表
* 订阅中心
* 协议统计
* 来源统计
* 抗干扰能力统计
* 节点筛选
* 一键复制订阅地址
* 深色 / 浅色主题

页面本身不需要后端服务，直接读取仓库中的静态 JSON 数据即可。

---

## 📡 订阅中心

> 点击对应订阅地址即可复制，支持 Clash / Mihomo、v2rayNG、Shadowrocket 等客户端。

| 客户端                           | 订阅格式   | 订阅地址                                                                                                  |
| :---------------------------- | :----- | :---------------------------------------------------------------------------------------------------- |
| 🟢 **Clash / Mihomo**         | YAML   | [`订阅地址`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/clash.yaml)           |
| 🔵 **v2rayNG / Shadowrocket** | Base64 | [`订阅地址`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe_base64.txt) |
| 🟣 **通用客户端**                  | URI    | [`订阅地址`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe.txt)        |
| 🟠 **HTTP / SOCKS**           | TXT    | [`代理列表`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/proxies.txt)          |
| ⚪ **开发者**                     | JSON   | [`JSON 数据`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/proxies.json)      |

### 🚀 一键订阅

**Clash / Mihomo**

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/clash.yaml
```

**v2rayNG / Shadowrocket**

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe_base64.txt
```

**通用 URI 订阅**

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe.txt
```

### 📋 在线数据

| 数据      | 地址                                                                                                    |
| :------ | :---------------------------------------------------------------------------------------------------- |
| 📊 节点统计 | [`stats.json`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/stats.json)     |
| 📦 完整节点 | [`proxies.json`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/proxies.json) |
| 📝 文本代理 | [`proxies.txt`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/proxies.txt)   |
| ℹ️ 元数据  | [`meta.json`](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/meta.json)       |

---

## 🖼️ 订阅卡片

如果仓库主页需要更醒目的效果，可以使用 HTML 卡片：

<div align="center">

<table> <tr> <td align="center" width="300">

### 🟢 Clash / Mihomo

**YAML 订阅**

[📋 获取订阅](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/clash.yaml)

</td>

<td align="center" width="300">

### 🔵 v2rayNG / Shadowrocket

**Base64 订阅**

[📋 获取订阅](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe_base64.txt)

</td> </tr>

<tr> <td align="center">

### 🟣 通用订阅

**URI 节点列表**

[📋 获取订阅](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe.txt)

</td>

<td align="center">

### 🟠 HTTP / SOCKS

**纯文本代理**

[📋 获取列表](https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/proxies.txt)

</td> </tr> </table>

</div>

---

## 📱 使用方式

### Clash / Mihomo

打开客户端的 **订阅管理**，添加：

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/clash.yaml
```

### v2rayNG / Shadowrocket

使用：

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe_base64.txt
```

作为远程订阅地址。

### 其他客户端

对于支持标准 URI 节点导入的客户端，可使用：

```text
https://raw.githubusercontent.com/xiaolaodi0719/FreeProxyHub/main/data/subscribe.txt
```

导入节点。

> ⚠️ GitHub Raw 属于静态文件地址。如果所在网络访问 GitHub 较慢，可以通过项目部署的 Web 页面获取对应订阅地址。
## 🛠️ 本地运行

### 环境要求

推荐：

```text
Python 3.9+
```

项目当前采集器基于 Python 标准库实现，核心流程通过 `collector.py` 执行。

### 运行

克隆项目：

```bash
git clone https://github.com/xiaolaodi0719/FreeProxyHub.git
cd FreeProxyHub
```

执行：

```bash
python collector.py
```

运行完成后即可在：

```text
data/
```

目录中看到生成的数据文件。采集器入口会依次执行 VPN Gate 采集、GitHub 数据采集、去重、统计和订阅生成。

---

## 📁 项目结构

```text
FreeProxyHub/
├── .github/
│   └── workflows/       # 自动化任务
│
├── data/                # 代理数据及订阅文件
│   ├── proxies.json
│   ├── stats.json
│   ├── meta.json
│   ├── clash.yaml
│   ├── subscribe.txt
│   ├── subscribe_base64.txt
│   └── proxies.txt
│
├── collector.py         # 代理采集、解析、去重、订阅生成
├── index.html           # Web 前端页面
└── README.md
```

---

## 🔧 数据处理流程

FreeProxyHub 的数据处理主要分为以下几个阶段：

### 1. 采集

从多个公开代理源获取原始节点数据。

### 2. 解析

将不同来源的代理格式统一转换成标准数据结构。

### 3. 清洗

过滤空数据、异常数据以及不符合格式的节点。

VPN Gate 数据还会根据延迟进行基础过滤，目前采集器会跳过 Ping 大于 `500ms` 的节点。

### 4. 去重

根据：

```text
IP:Port:Protocol
```

进行去重。

### 5. 节点筛选

生成订阅时会优先处理标记为 `strong` / `extreme` 的节点，再补充普通节点。

### 6. 生成订阅

最终生成：

```text
Clash YAML
Base64
URI
Plain Text
JSON
```

等多种数据格式。

---

## ⚠️ 重要说明

### 免费节点不保证稳定性

本项目中的代理节点均来自公开互联网来源，节点可能随时：

* 失效
* 超时
* 更换 IP
* 速度下降
* 无法连接
* 被服务商封禁

因此：

> **订阅中的节点仅供测试和学习使用，请勿将免费节点视为稳定的生产级代理服务。**

---

### 不保证匿名性

免费代理节点来源复杂，无法保证：

* 节点运营者可信
* 流量不会被记录
* 节点不会修改请求
* 节点不会进行流量分析

因此，**不要通过未知免费代理传输密码、私钥、银行卡信息或其他敏感数据。**

---

### 节点来源说明

FreeProxyHub 本身主要负责：

```text
采集 → 整理 → 去重 → 格式化 → 发布
```

并不拥有或运营所有代理服务器。

节点来自公开互联网资源，其真实性、稳定性和安全性由对应节点提供者决定。

---

## 📊 节点数量

项目不会对节点总数做固定承诺。

由于代理源本身具有动态变化特征，每次更新后的：

* 节点数量
* 协议数量
* 国家 / 地区分布
* 节点可用性

都可能不同。

实际数据请以仓库最新生成的 `data/stats.json` 为准。

---

## 🤝 贡献

欢迎提交：

* 新的免费代理数据源
* 新的协议解析支持
* 节点过滤优化
* 订阅格式优化
* Web UI 改进
* Bug 修复
* 文档完善

提交 Pull Request 前建议先确认：

```text
数据源稳定
解析逻辑正确
不会大量产生重复节点
不会明显增加 GitHub Actions 资源消耗
```

---

## ⭐ Star

觉得项目有帮助，可以给仓库点个 ⭐ Star。

GitHub：

https://github.com/xiaolaodi0719/FreeProxyHub

---

## 📜 License

本项目遵循仓库当前采用的许可证。

对于第三方代理节点及数据源，其版权、使用条款和法律责任请以对应来源的许可协议及当地法律法规为准。

---

## ⚠️ 免责声明

本项目仅用于：

* 网络编程学习
* 代理协议研究
* 自动化数据处理
* 开源项目研究
* 软件测试

使用者应自行遵守所在国家或地区的法律法规，以及相关网络服务的服务条款。

**因使用本项目或项目提供的数据产生的任何直接或间接后果，由使用者自行承担。**
