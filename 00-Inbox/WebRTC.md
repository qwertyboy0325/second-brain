WebRTC（Web Real-Time Communication）是一種**開源技術**，可用於瀏覽器或應用程式之間進行**即時視訊、語音和數據通訊**，**不需要中間伺服器**，直接點對點（P2P）傳輸資料。

✅ **核心特點**

- **低延遲（Real-time）**：支援即時視訊、語音、數據同步
- **點對點（P2P）**：可直接在裝置之間傳輸資料，減少伺服器負擔
- **跨平台**：支援 **Web 瀏覽器（Chrome, Firefox, Edge, Safari）、行動裝置、原生應用**
- **開源標準**：由 **Google、Mozilla、Opera、W3C** 共同開發
## **1️⃣ WebRTC 的核心組件**

WebRTC 主要有 3 個核心 API： 1️⃣ **RTCPeerConnection**（點對點連線）  
2️⃣ **RTCDataChannel**（傳輸非媒體數據）  
3️⃣ **MediaStream API**（音訊 & 視訊處理）

**👉 這 3 個 API 組成了一個完整的 WebRTC 通訊架構，支援點對點音訊、視訊與數據傳輸。**

---

## **2️⃣ WebRTC 連線建立流程**

WebRTC 的核心機制是 **NAT 穿透（NAT Traversal）**，確保不同網絡環境下的設備可以直接連線。其主要流程如下：

### **📡 1. 訊號交換（Signaling）**

- 兩台裝置（用戶 A 和用戶 B）需要透過 **訊號伺服器（Signaling Server）** 交換連線資訊（ICE 候選者、SDP）。
- 訊號伺服器**不負責傳輸音訊或視訊，只用來協商連線**。
```
[Alice] → Signal Server → [Bob]
```
🔹 **技術選擇**：

- 訊號伺服器可用 **WebSocket、Socket.IO、HTTP polling**
- WebRTC 本身**不內建 Signaling**，開發者需自行實作
### **📡 2. 連線建立（ICE、STUN、TURN）**

當雙方交換了 SDP（Session Description Protocol）後，開始使用 ICE（Interactive Connectivity Establishment）進行 P2P 連線。

1. **STUN（Session Traversal Utilities for NAT）**：
    - 嘗試建立 **直接 P2P 連線**，讓設備得知自己的公網 IP。
2. **TURN（Traversal Using Relays around NAT）**：
    - 如果 STUN 失敗（如企業防火牆封鎖 P2P），則使用 TURN 伺服器中繼數據。
```
[Alice] ←(STUN)→ [Bob] # 直接連線（最理想） [Alice] ←(TURN)→ [TURN Server] ←(TURN)→ [Bob] # 透過 TURN 轉發數據
```
**技術選擇**：

- **開源 STUN/TURN 伺服器**：[`coturn`](https://github.com/coturn/coturn)
- Google 提供 **免費 STUN 伺服器**：
```
{ "iceServers": [{ "urls": "stun:stun.l.google.com:19302" }] }
```
### **📡 3. 音訊 & 視訊 & 資料傳輸**

- 當 P2P 連線建立後，雙方開始交換音訊、視訊、或文字數據：
    - **MediaStream API** 傳輸音訊與視訊
    - **RTCDataChannel** 傳輸文字訊息或其他非媒體數據（類似 WebSocket）