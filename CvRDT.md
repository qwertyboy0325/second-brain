### **狀態型（State-Based, CvRDT）**

- 也叫做 **Convergent Replicated Data Type（CvRDT）**
- 主要透過**合併（Merge）**不同的副本來同步資料
- 每個節點都能保持自己最新的狀態，然後與其他節點交換並合併狀態
- **適用場景**：分散式資料存儲，如 **Riak、Cassandra、AntidoteDB**

**🔹 例子：G-Counter（增量計數器）**

``` rust
struct GCounter { values: HashMap<NodeId, u64>, } impl GCounter { fn increment(&mut self, node: NodeId) { *self.values.entry(node).or_insert(0) += 1; } fn merge(&mut self, other: &GCounter) { for (node, count) in &other.values { self.values.insert(*node, self.values.get(node).cloned().unwrap_or(0).max(*count)); } } fn total(&self) -> u64 { self.values.values().sum() } }
```



💡 這種計數器能夠確保 **所有副本最終一致**，即使不同節點各自進行加法操作，最終結果仍然相同。