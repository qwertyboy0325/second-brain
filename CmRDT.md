- 也叫做 **Commutative Replicated Data Type（CmRDT）**
- 透過**廣播操作（Broadcast Operations）**來同步狀態，而不是直接同步整個狀態。
- 每個節點都會將自身的操作記錄下來，**並透過事件傳播給其他節點來更新狀態**。
- **適用場景**：多人協作工具，如 **Figma、Google Docs、Yjs、Automerge**

**🔹 例子：LWW-Register（最新寫入勝出，Last-Write-Wins Register）**

``` rust
struct LWWRegister<T> { value: T, timestamp: u64, } impl<T: Clone> LWWRegister<T> { fn write(&mut self, new_value: T, new_timestamp: u64) { if new_timestamp > self.timestamp { self.value = new_value; self.timestamp = new_timestamp; } } fn merge(&mut self, other: &LWWRegister<T>) { if other.timestamp > self.timestamp { self.value = other.value.clone(); self.timestamp = other.timestamp; } } }
```