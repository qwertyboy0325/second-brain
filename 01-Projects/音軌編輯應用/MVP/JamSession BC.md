## JamSession BC — 模組設計手冊

> **定位**：管理房間內的 Jam 流程──玩家角色配置、Ready 倒數、回合控制，並透過 IntegrationEvent 驅動前端 UI 與其它 BC。

---

### 1. 聚合與模型

| 類型 / 分類                       | 定位 / 角色                  | 關鍵屬性                                                                                                                 | 主要行為                                                             | 關鍵不變式 ✔                                     |
| ----------------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------- |
| **Session**<br>Aggregate Root | 一場 JamSession（對應一個 Room） | `sessionId` · `roomId` · `status: pending｜inProgress｜ended` · `players: Map<PeerId,PlayerState>` · `rounds: Round[]` | `startSession()`, `endSession()`<br>`startRound()`, `endRound()` | 只能在 `status=pending` 時啟動<br>回合序號不可跳號        |
| **PlayerState**<br>Entity     | 房間中某位玩家於本場 Session 的狀態   | `peerId` · `role?: RoleVO` · `isReady: boolean` · `joinedAt: Timestamp`                                              | `setRole(roleVO)`, `setReady(isReady)`                           | 角色設定僅限 `status=pending`<br>Ready 需在角色設定後才允許 |
| **RoleVO**<br>Value Object    | 玩家在 Jam 中的角色             | `id: string` · `name: string` · `color: string`                                                                      | `equals(other: RoleVO)`                                          | 同一角色 ID 只能被一位玩家持有                           |
| **Round**<br>Value Object     | 一回合的元資料                  | `roundId` · `startedAt: Timestamp` · `endedAt?: Timestamp`                                                           | `duration()`                                                     | 完成後 `startedAt < endedAt`                   |

---

### 2. Domain Events

| 事件名稱               | 觸發方法                     | 典型 Payload                             |
|------------------------|------------------------------|------------------------------------------|
| `PlayerRoleSet`        | `PlayerState.setRole()`      | `{ sessionId, peerId, role: RoleVO }`    |
| `PlayerReadyToggled`   | `PlayerState.setReady()`     | `{ sessionId, peerId, isReady }`         |
| `SessionStarted`       | `Session.startSession()`     | `{ sessionId, startTime }`               |
| `RoundStarted`         | `Session.startRound()`       | `{ sessionId, roundId, startTime }`      |
| `RoundEnded`           | `Session.endRound()`         | `{ sessionId, roundId, endTime }`        |
| `SessionEnded`         | `Session.endSession()`       | `{ sessionId, endTime }`                 |

> 聚合內 `raise()` → Application 層 `pullDomainEvents()` → 轉成 Integration Events

---

### 3. Integration Events

| 事件名稱                | Publisher               | Subscriber                         | Payload                         |
|-------------------------|-------------------------|------------------------------------|---------------------------------|
| `jam.player-role-set`   | JamSession App          | UI / CollaborationBridge           | `{ sessionId, peerId, role }`   |
| `jam.player-ready`      | JamSession App          | UI / CollaborationBridge           | `{ sessionId, peerId, isReady }`|
| `jam.session-started`   | JamSession App          | UI / MusicArrangement / Collaboration | `{ sessionId, startTime }`  |
| `jam.round-started`     | JamSession App          | UI / MusicArrangement              | `{ sessionId, roundId, startTime }` |
| `jam.round-ended`       | JamSession App          | UI / MusicArrangement              | `{ sessionId, roundId, endTime }`   |
| `jam.session-ended`     | JamSession App          | UI / CollaborationBridge           | `{ sessionId, endTime }`        |

---

### 4. 輔助元件

| 模式               | 範例                                    |
|--------------------|-----------------------------------------|
| **Specification**  | `AllPlayersReadySpec`                   |
| **Domain Service** | `CountdownService`                      |
| **Factory**        | `SessionFactory.createFromRoom(roomVO)` |
| **Repository**     | `SessionRepository.load/save`           |
| **Domain Error**   | `DomainError('NOT_ALL_READY')`          |

---

### 5. ID 生成策略

| 物件        | 格式                 | 生成器 (DI)             |
| --------- | ------------------ | -------------------- |
| SessionId | `nanoid(8)`        | `RandomIdGen`        |
| RoundId   | 自增數字               | `RoundIdSequenceGen` |
| PeerId    | 與 Collaboration 共用 | `PeerIdGen`          |

---

### 6. 模組關係圖

```mermaid
flowchart LR
  subgraph UI_Block["UI Components"]
    UIComp["JamSession UI<br/>(選角色、Ready、倒數、回合檢視)"]
  end

  subgraph App["Application (Client Layer)"]
    CMD["CmdHandlers<br/>(SetRole, ToggleReady)"]
    APP["SessionAppService<br/>(start/end session, rounds)"]
    BUS["EventBus"]
    IB["IntegrationBridge"]
  end

  subgraph Domain["Domain (Client-Domain)"]
    AGG["Session Aggregate"]
    PS["PlayerState Entity"]
    RV["RoleVO"]
    RND["Round VO"]
  end

  subgraph Infra["Infrastructure (Browser APIs)"]
    CD["RTCPeerConnection / DataChannel"]
  end

  UIComp --> CMD
  CMD --> AGG
  AGG --> BUS
  BUS --> IB
  IB --> CD
  CD --> IB
  IB --> BUS
  BUS --> APP
  APP --> BUS
  BUS --> UIComp
  ```

---
### 7. 目錄建議（pnpm workspace）
```
jam-session-domain/
├─ aggregates/
│   └─ session.ts
├─ entities/
│   └─ player-state.ts
├─ value-objects/
│   ├─ role.ts
│   └─ round.ts
├─ events/
│   └─ session-events.ts
├─ specs/
│   └─ all-players-ready.spec.ts
├─ services/
│   └─ countdown-service.ts
├─ factories/
│   └─ session-factory.ts
├─ interfaces/
│   └─ session-repository.ts
├─ errors/
│   └─ domain-error.ts
└─ index.ts
```

---
### 8. 近期落地里程碑
1. **SetRole / ToggleReady 單元測試**
2. **AllPlayersReadySpec + CountdownService** 倒數流程
3. **SessionStarted / RoundStarted / RoundEnded** 全事件鏈接
4. **IntegrationBridge**：`jam.*` 事件透過 DataChannel 廣播
5. **E2E Demo**：4 人選角 → 全部 Ready → 倒數 → 兩輪 Jam
  