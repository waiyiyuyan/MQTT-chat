# 💬 MQTT Web ChatRoom (网页实时聊天室)

[![MQTT](https://img.shields.io/badge/MQTT-5.0%20%2F%203.1.1-blue.svg)](https://mqtt.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Frontend](https://img.shields.io/badge/Frontend-HTML%20%2F%20CSS%20%2F%20JS-orange.svg)](#)

这是一个**纯前端、无后端服务器依赖**的轻量级 Web 实时聊天室项目。

项目基于 **MQTT 协议（通过 WebSocket 传输）** 搭建，仅靠**单个 HTML 文件**即可在浏览器中运行。它巧妙利用 MQTT 协议原生的 **Retain（保留消息）** 与 **Last Will（遗嘱消息）** 机制，实现了无数据库支撑下的“实时聊天”与“动态在线用户列表感知”。

---

## ✨ 核心特性

- 🚀 **零后端依赖**：直接连接公共 MQTT Broker（如 EMQX），无需搭建额外的 WebSocket / Node.js 服务。
- 👥 **全动态用户感知**：
  - **上线广播**：巧用 **Retain 机制**，新进房间的用户能瞬间获取当前所有在线人员列表。
  - **离线感知**：巧用 **Last Will（遗嘱）机制**，当用户异常断网或关闭网页时，Broker 会自动触发遗嘱广播并清除在线记录。
- 🎨 **Apple 极简风格 UI**：采用现代化毛玻璃（Backdrop Filter）视觉设计，响应式自适应布局。
- ✍️ **自适应文本框**：输入框支持高度根据内容自动扩展，优化交互体验。
- 📦 **单文件即开即用**：无需任何编译构建步骤（No npm / No Webpack），双击 HTML 即可体验。

---

## 🛠️ 核心原理说明

```text
[ 客户端 A ] --( 1. 发布上线消息 + Retain )--> [ MQTT Broker ] <--( 2. 订阅通配符 Topic )-- [ 客户端 B ]
[ 客户端 A ] --( 3. 异常断线/关闭页面 )---------> [ MQTT Broker ] --( 4. 触发遗嘱(Will)消息 )-> [ 客户端 B ]

1. **消息隔离**：
* 聊天消息：`chat/{ROOM_ID}/messages`
* 用户状态：`chat/{ROOM_ID}/users/{clientId}`（借助通配符 `chat/{ROOM_ID}/users/+` 进行监听）


2. **上线保持（Retain）**：上线时向个人 Topic 发布带 `retain: true` 的 JSON 数据，Broker 会为后来者保留当前最新的上线状态。
3. **掉线清除（Last Will）**：建立连接时预设遗嘱为`空 Payload`（MQTT 规范中，空 Payload 的 Retain 消息等于删除该保留记录）。一旦设备离线，Broker 自动广播空消息，各客户端收到后将该用户从本地列表移除。
```
---

## 🚀 快速开始

### 1. 本地运行

1. 克隆本项目或下载源码：
```bash
git clone https://github.com/waiyiyuyan/MQTT-chat.git
```


2. 直接使用浏览器双击打开 `index.html` 文件即可运行。

### 2. 体验多用户在线测试

* 尝试打开多个浏览器标签页（或使用无痕模式/不同浏览器）。
* 你会看到每个标签页会自动生成随机昵称，并实时出现在其他标签页的侧边“USERS”列表中。
* 尝试关闭其中一个标签页，其他页面会在数秒内感知到该用户下线并自动刷新列表。

---

## ⚙️ 自定义配置

在 `index.html` 的 `<script>` 标签顶部，你可以修改以下配置项：

```javascript
// Broker 接入点（默认使用 EMQX 官方公共测试节点）
const BROKER_URL = 'wss://broker.emqx.io:8084/mqtt';

// 房间 ID（可以修改为你独特的频道名称，防止冲突）
const ROOM_ID = 'my_web_chat_room_2026';

```

---

## 🛠️ 技术栈

* **HTML5 / CSS3**
* **JavaScript (ES6+)**
* **MQTT.js**（提供浏览器环境下的 WebSocket MQTT 客户端能力）

---

## 📄 开源协议

本项目采用 [MIT License](https://www.google.com/search?q=LICENSE) 协议开源，欢迎自由 fork 和二次修改。

```

```
