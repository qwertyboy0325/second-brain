## 📅 Echlub MVP — 任務導向排程（含 UI 實作，週末休息）

> **UI 設計外包**，此處僅排「實作 UI（React + PixiJS）」  
> **週六、週日休息**，不排工作

---

### Identity BC (5/2–5/6)  
- 5/2 (週五)  
  - [x] Domain & App：實作 `User` Aggregate + `EmailVO` + `PasswordPolicySpec` + `RegisterCmdHandler`/`LoginCmdHandler` + 單元測試  
- 5/5 (週一)  
  - [x] Backend Infra：`UserRepository` (DB schema + migration) & `JwtService` + persistence 測試  
  - [x] REST Controller `/auth/register`、`/auth/login` + 集成測試  
  - [x] UI 實作：React `LoginPage` & `RegisterPage` + 表單驗證 + E2E (註冊→登入→JWT)  
- 5/6 (週二)  
  - [x] Buffer & Review：修正遺留問題，合併 PR  

---

### Collaboration BC (5/6–5/12)  
- 5/6 (週二)  
  - [x] Domain & App：實作 `Room` Aggregate + `RoomRuleVO` + `Peer` + `CreateRoomCmdHandler`/`JoinRoomCmdHandler` + 單元測試  
- 5/7 (週三)  
  - [x] Backend Signaling：部署 NestJS WS Gateway + STUN/TURN 配置  
- 5/8 (週四)  
  - [x] Backend Connection：實作 `ConnectionManager` (Mesh & Relay) + `IntegrationBridge`  
- 5/9 (週五)  
  - [x] UI 實作：React `RoomListPage` & `PlayerStatusBadge` + WebRTC 連線測試  
- 5/12 (週一)  
  - [ ] LoggingAdapter：實作事件審計 Adapter + 單元測試  
  - [ ] Metrics API：實作 `GET /rooms/:id/metrics` + 集成測試  
  - [ ] Buffer & Review：端到端測試 & PR 合併  

---

### JamSession BC (5/12–5/16)  
- 5/12 (週一)  
  - [ ] Domain & Spec/Service：`Session` + `PlayerState` + `RoleVO` + `RoundVO` + `AllPlayersReadySpec` + `CountdownService` + 單元測試  
- 5/13 (週二)  
  - [ ] Application：`SetRoleCmd`/`ToggleReadyCmd`/`StartRoundCmd` Handler + 測試  
- 5/14 (週三)  
  - [ ] Infra：`IntegrationBridge` → DataChannel 廣播 `jam.*`  
- 5/15 (週四)  
  - [ ] UI 實作：React `SessionReadyBoard` + Ready 倒數動畫 + Round 控制  
- 5/16 (週五)  
  - [ ] LoggingAdapter：實作 JamSession 事件審計 Adapter + 單元測試  
  - [ ] Metrics API：實作 `GET /sessions/:id/metrics` + 集成測試  
  - [ ] Buffer & Review：整合測試 & PR 合併  

---

### MusicArrangement BC – Part 1 (5/16–5/23)  
- 5/16 (週五)  
  - [ ] Domain：`Track` + `AudioClipVO` + `TimeRangeVO` + `Bpm` + 單元測試  
- 5/19 (週一)  
  - [ ] Spec/Service：`NoOverlapSpec`、`InsideRoundSpec`、`QuantizeService` + 測試  
- 5/20 (週二)  
  - [ ] App & Playback Infra：`CreateClipCmd`/`MoveClipCmd`/`RemoveClipCmd` Handlers + Tone.js `TransportAdapter` stub  
- 5/21 (週三)  
  - [ ] Sync Infra：`IntegrationBridge` 同步 `music.clipop-broadcast` & `music.note-op`  
- 5/22 (週四)  
  - [ ] UI 實作：React + PixiJS `TimelinePage` & `Clip`/`Track` 拖拉交互  
- 5/23 (週五)  
  - [ ] Buffer & Review：E2E Demo & PR 合併  

---

### MusicArrangement BC – Part 2 (5/23–5/28)  
- 5/23 (週五)  
  - [ ] Domain：`AudioClip` Entity + `TakeRecorded`/`TakeSelected` Events + 單元測試  
- 5/26 (週一)  
  - [ ] App & Audio Infra：`RecordingSaga`/`UploadSampleSaga` + MediaRecorder → WebRTC Track + WS chunk 備份 Demo  
- 5/27 (週二)  
  - [ ] UI 實作：React + PixiJS 錄音控制 & waveform/take 選擇  
- 5/28 (週三)  
  - [ ] Buffer & Review：PR 合併  

---

### PublicShare BC (5/28–5/31)  
- 5/28 (週三)  
  - [ ] Domain & App：`ShareSession` + `ShareSettingsVO` + `MixProgressVO` + `LoadMixQueryHandler` + QR Service + 測試  
- 5/29 (週四)  
  - [ ] Infra：`MixService` + REST `/share/:id/mix` + IndexedDB/S3 Adapter  
- 5/30 (週五)  
  - [ ] UI 實作：React `SharePage` + 播放控制 & QR 顯示  
- 5/31 (週六) & 6/1 (週日)  
  - **休息**  

---

### Integration & Final Demo (6/2–6/4)  
- 6/2 (週一)  
  - [ ] Integration Testing：跨 BC IntegrationEvent 測試腳本  
- 6/3 (週二)  
  - [ ] Load Test：4 人並行 + ICE/Relay 模擬  
- 6/4 (週三)  
  - [ ] Final Demo：全流程演示 + 講解稿  
  - [ ] PR & Release  
