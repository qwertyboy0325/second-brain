**CRDT（Conflict-Free Replicated Data Types）** 是一種特殊的資料結構，主要用於 **分散式系統**，特別是在**多人協作編輯（如 Figma）、即時同步（如 Google Docs）、區塊鏈、分散式資料庫（如 Redis、Cassandra）**等場景。

CRDT 允許多個節點（clients）在**沒有中央協調**的情況下，**獨立更新數據**，並能夠**自動合併變更**，確保最終一致性（Eventually Consistency）且**無需鎖機制（No Locking）**。

## **🎯 CRDT 的核心特性**

1. **最終一致性（Eventually Consistent）**
    - 任何節點上的變更都能最終達成一致，而不需要中心化控制。
2. **無衝突（Conflict-Free）**
    - 透過數學性質，確保在合併數據時不會發生衝突，即使不同用戶同時修改數據。
3. **可合併（Mergeable）**
    - CRDTs 設計成能夠透過特定規則來合併來自不同節點的變更。
4. **無需鎖（Lock-Free）**
    - CRDTs 允許離線操作（Offline Editing），當重新連接時，系統能夠自動合併變更。
5. **支援分散式系統（Distributed & Decentralized）**
    - 適用於 **P2P（點對點）、區塊鏈、多人協作應用、分散式資料庫** 等系統。

- [[CvRDT]]
- [[CmRDT]]