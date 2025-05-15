## Collaboration BC — 模組設計手冊

> **定位**：負責「房間管理」與「P2P/WebRTC 信令＆事件轉發」，為 JamSession、MusicArrangement 提供穩定的多端連線與資料同步管道。

---

### 1. 聚合與模型

| 類型 / 分類              | 定位 / 角色             | 關鍵屬性 (※ = VO)                                                    | 主要行為                                          | 關鍵不變式 ✔                           |
|-------------------------|-------------------------|-----------------------------------------------------------------------|---------------------------------------------------|----------------------------------------|
| **Room**<br>Aggregate   | 房間核心                | `roomId` · `ownerId` · `players: Set<PeerId>` · `rules: RoomRuleVO*`   | `create(owner, rules)`, `join(peer)`, `leave(peer)`, `updateRule(rules)`, `close()` | `players.size ≤ rules.maxPlayers`       |
| **RoomRuleVO**<br>Value Object | 房規設定              | `maxPlayers` · `connectionMode` · `allowRelay` · `opusBitrate` · `latencyTargetMs` | `isValidFor(nPlayers)`                              | `maxPlayers ≥ currentPlayers.size`     |
| **Peer**<br>Entity      | 同房成員                | `peerId` · `joinedAt: Timestamp`                                       | —                                                 | —                                      |

---

### 2. Domain Events

| 事件名稱                   | 觸發方法                   | 典型 Payload                         |
|----------------------------|----------------------------|--------------------------------------|
| `RoomCreated`              | `Room.create()`            | `{ roomId, ownerId, rules }`         |
| `PlayerJoined`             | `Room.join()`              | `{ roomId, peerId }`                 |
| `PlayerLeft`               | `Room.leave()`             | `{ roomId, peerId }`                 |
| `RoomRuleChanged`          | `Room.updateRule()`        | `{ roomId, newRules }`               |
| `RoomClosed`               | `Room.close()`             | `{ roomId }`                         |
| `RoomOwnerChanged`         | (內部邏輯)                  | `{ roomId, newOwnerId }`             |

> Aggregate 內 `raise()` → Application 層 `pullDomainEvents()` → 轉成 Integration Events

---

### 3. Integration Events

| 事件名稱                     | Publisher              | Subscriber                       | Payload 範例                   |
|------------------------------|------------------------|----------------------------------|-------------------------------|
| `collab.room-created`        | Room App               | JamSession / UI                  | `{ roomId, ownerId, rules }`  |
| `collab.player-joined`       | Room App               | JamSession / UI                  | `{ roomId, peerId }`          |
| `collab.player-left`         | Room App               | JamSession / UI                  | `{ roomId, peerId }`          |
| `collab.room-rule-changed`   | Room App               | ConnectionManager / UI           | `{ roomId, newRules }`        |
| `collab.room-closed`         | Room App               | All BC / UI                      | `{ roomId }`                  |
| `collab.room-owner-changed`  | Room App               | JamSession / UI                  | `{ roomId, newOwnerId }`      |
| `music.clipop-broadcast`     | MusicArrangement App    | IntegrationBridge                | `{ ... }`                     |
| `music.note-op`              | MusicArrangement App    | IntegrationBridge                | `{ ... }`                     |

---

### 4. 輔助元件

| 模式               | 範例                                 |
|--------------------|--------------------------------------|
| **Specification**  | `MaxPlayersSpec`, `ValidRuleSpec`    |
| **Domain Service** | `RoomRuleValidationService`          |
| **Factory**        | `RoomFactory.createWithDefaults()`   |
| **Repository**     | `RoomRepository.load(roomId) / save(room)` |
| **Domain Error**   | `DomainError('ROOM_FULL')`, `DomainError('INVALID_RULE')` |

---

### 5. ID 生成策略

| 物件     | 格式                        | 生成器 (DI)      |
| ------ | ------------------------- | ------------- |
| RoomId | `nanoid(8)`               | `RandomIdGen` |
| PeerId | `UUID v4` or `nanoid(10)` | `PeerIdGen`   |

---

### 6. 模組關係圖

```mermaid
flowchart LR
  UI[Client UI] --> API[Collaboration Application Layer]
  API --> RM[Room Aggregate]
  RM --> EV[EventBus]
  EV --> IB[IntegrationBridge]
  IB --> CM[ConnectionManager]
  CM --> SH[SignalHubService]
  SH --> WS[WebSocket Server]
  SH --> STUN[STUN/TURN Servers]
  SH --> Relay[WS Relay Service]
  CM --> IB
  IB --> EV
  API --> UI
  ```
### 7.目錄建議（pnpm workspace）
  ```
collaboration-domain/
├─ aggregates/
│   └─ room.ts
├─ value-objects/
│   └─ room-rule-vo.ts
├─ entities/
│   └─ peer.ts
├─ events/
│   └─ room-events.ts
├─ specs/
│   ├─ max-players.spec.ts
│   └─ valid-rule.spec.ts
├─ services/
│   └─ room-rule-validation-service.ts
├─ factories/
│   └─ room-factory.ts
├─ interfaces/
│   └─ room-repository.ts
├─ errors/
│   └─ domain-error.ts
└─ index.ts
```
