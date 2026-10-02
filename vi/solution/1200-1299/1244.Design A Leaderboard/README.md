---
comments: true
difficulty: Medium
rating: 1354
source: Biweekly Contest 12 Q1
tags:
    - Design
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [1244. Design A Leaderboard 🔒](https://leetcode.com/problems/design-a-leaderboard)

[中文文档](/solution/1200-1299/1244.Design%20A%20Leaderboard/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế class Leaderboard gồm 3 hàm:</p>

<ol>
	<li><code>addScore(playerId, score)</code>: Cập nhật leaderboard bằng cách cộng <code>score</code> vào điểm của người chơi tương ứng. Nếu leaderboard chưa có người chơi với id đó, thêm người chơi đó với điểm <code>score</code>.</li>
	<li><code>top(K)</code>: Trả về tổng điểm của <code>K</code> người chơi có điểm cao nhất.</li>
	<li><code>reset(playerId)</code>: Đặt điểm của người chơi có id đã cho về 0 (tức là xóa người đó khỏi leaderboard). Đảm bảo người chơi đã được thêm vào leaderboard trước khi gọi hàm này.</li>
</ol>

<p>Ban đầu, leaderboard rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<b>Đầu vào: </b>
[&quot;Leaderboard&quot;,&quot;addScore&quot;,&quot;addScore&quot;,&quot;addScore&quot;,&quot;addScore&quot;,&quot;addScore&quot;,&quot;top&quot;,&quot;reset&quot;,&quot;reset&quot;,&quot;addScore&quot;,&quot;top&quot;]
[[],[1,73],[2,56],[3,39],[4,51],[5,4],[1],[1],[2],[2,51],[3]]
<b>Đầu ra: </b>
[null,null,null,null,null,null,73,null,null,null,141]

<b>Giải thích: </b>
Leaderboard leaderboard = new Leaderboard ();
leaderboard.addScore(1,73);   // leaderboard = [[1,73]];
leaderboard.addScore(2,56);   // leaderboard = [[1,73],[2,56]];
leaderboard.addScore(3,39);   // leaderboard = [[1,73],[2,56],[3,39]];
leaderboard.addScore(4,51);   // leaderboard = [[1,73],[2,56],[3,39],[4,51]];
leaderboard.addScore(5,4);    // leaderboard = [[1,73],[2,56],[3,39],[4,51],[5,4]];
leaderboard.top(1);           // returns 73;
leaderboard.reset(1);         // leaderboard = [[2,56],[3,39],[4,51],[5,4]];
leaderboard.reset(2);         // leaderboard = [[3,39],[4,51],[5,4]];
leaderboard.addScore(2,51);   // leaderboard = [[2,51],[3,39],[4,51],[5,4]];
leaderboard.top(3);           // returns 141 = 51 + 51 + 39;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= playerId, K &lt;= 10000</code></li>
	<li>Đảm bảo <code>K</code> không lớn hơn số người chơi hiện tại.</li>
	<li><code>1 &lt;= score&nbsp;&lt;= 100</code></li>
	<li>Có tối đa <code>1000</code>&nbsp;lần gọi hàm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Ordered List

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần cập nhật hoặc reset điểm người chơi và tính tổng $K$ điểm cao nhất. Với tối đa một nghìn lần gọi, việc quét toàn bộ người chơi mỗi lần không hiệu quả. Hash map lưu điểm theo $playerId$; một danh sách đã sắp xếp lưu các điểm, kể cả những điểm trùng nhau.
>
> Khi cộng điểm, ta xóa điểm cũ rồi chèn điểm mới; thao tác reset cũng xóa điểm tương ứng; $top(K)$ tính tổng $K$ phần tử cuối danh sách. Hash map tìm điểm của người chơi trong $O(1)$, còn danh sách đã sắp xếp hỗ trợ cập nhật trong thời gian logarithmic và lấy một đoạn cuối theo thứ tự.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu điểm của từng người chơi và danh sách có thứ tự $rank$ để lưu điểm của tất cả người chơi.

Khi gọi hàm `addScore`, trước tiên ta kiểm tra người chơi đã có trong hash table $d$ chưa. Nếu chưa, ta thêm điểm của họ vào danh sách $rank$. Nếu đã có, ta xóa điểm cũ khỏi $rank$, thêm điểm mới vào $rank$, rồi cập nhật điểm trong $d$. Độ phức tạp thời gian là $O(\log n)$.

Khi gọi hàm `top`, ta trả về tổng điểm của $K$ người chơi đứng đầu bảng xếp hạng. Độ phức tạp thời gian là $O(K \times \log n)$.

Khi gọi hàm `reset`, ta xóa người chơi khỏi hash table $d$, rồi xóa điểm của họ khỏi danh sách $rank$. Độ phức tạp thời gian là $O(\log n)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số người chơi.

<!-- tabs:start -->

#### Python3

```python
class Leaderboard:
    def __init__(self):
        self.d = defaultdict(int)
        self.rank = SortedList()

    def addScore(self, playerId: int, score: int) -> None:
        if playerId not in self.d:
            self.d[playerId] = score
            self.rank.add(score)
        else:
            self.rank.remove(self.d[playerId])
            self.d[playerId] += score
            self.rank.add(self.d[playerId])

    def top(self, K: int) -> int:
        return sum(self.rank[-K:])

    def reset(self, playerId: int) -> None:
        self.rank.remove(self.d.pop(playerId))


# Your Leaderboard object will be instantiated and called as such:
# obj = Leaderboard()
# obj.addScore(playerId,score)
# param_2 = obj.top(K)
# obj.reset(playerId)
```

#### Java

```java
class Leaderboard {
    private Map<Integer, Integer> d = new HashMap<>();
    private TreeMap<Integer, Integer> rank = new TreeMap<>((a, b) -> b - a);

    public Leaderboard() {
    }

    public void addScore(int playerId, int score) {
        d.merge(playerId, score, Integer::sum);
        int newScore = d.get(playerId);
        if (newScore != score) {
            rank.merge(newScore - score, -1, Integer::sum);
        }
        rank.merge(newScore, 1, Integer::sum);
    }

    public int top(int K) {
        int ans = 0;
        for (var e : rank.entrySet()) {
            int score = e.getKey(), cnt = e.getValue();
            cnt = Math.min(cnt, K);
            ans += score * cnt;
            K -= cnt;
            if (K == 0) {
                break;
            }
        }
        return ans;
    }

    public void reset(int playerId) {
        int score = d.remove(playerId);
        if (rank.merge(score, -1, Integer::sum) == 0) {
            rank.remove(score);
        }
    }
}

/**
 * Your Leaderboard object will be instantiated and called as such:
 * Leaderboard obj = new Leaderboard();
 * obj.addScore(playerId,score);
 * int param_2 = obj.top(K);
 * obj.reset(playerId);
 */
```

#### C++

```cpp
class Leaderboard {
public:
    Leaderboard() {
    }

    void addScore(int playerId, int score) {
        d[playerId] += score;
        int newScore = d[playerId];
        if (newScore != score) {
            rank.erase(rank.find(newScore - score));
        }
        rank.insert(newScore);
    }

    int top(int K) {
        int ans = 0;
        for (auto& x : rank) {
            ans += x;
            if (--K == 0) {
                break;
            }
        }
        return ans;
    }

    void reset(int playerId) {
        int score = d[playerId];
        d.erase(playerId);
        rank.erase(rank.find(score));
    }

private:
    unordered_map<int, int> d;
    multiset<int, greater<int>> rank;
};

/**
 * Your Leaderboard object will be instantiated and called as such:
 * Leaderboard* obj = new Leaderboard();
 * obj->addScore(playerId,score);
 * int param_2 = obj->top(K);
 * obj->reset(playerId);
 */
```

#### Rust

```rust
use std::collections::BTreeMap;

#[allow(dead_code)]
struct Leaderboard {
    /// This also keeps track of the top K players since it's implicitly sorted
    record_map: BTreeMap<i32, i32>,
}

impl Leaderboard {
    #[allow(dead_code)]
    fn new() -> Self {
        Self {
            record_map: BTreeMap::new(),
        }
    }

    #[allow(dead_code)]
    fn add_score(&mut self, player_id: i32, score: i32) {
        if self.record_map.contains_key(&player_id) {
            // The player exists, just add the score
            self.record_map
                .insert(player_id, self.record_map.get(&player_id).unwrap() + score);
        } else {
            // Add the new player to the map
            self.record_map.insert(player_id, score);
        }
    }

    #[allow(dead_code)]
    fn top(&self, k: i32) -> i32 {
        let mut cur_vec: Vec<(i32, i32)> = self.record_map.iter().map(|(k, v)| (*k, *v)).collect();
        cur_vec.sort_by(|lhs, rhs| rhs.1.cmp(&lhs.1));
        // Iterate reversely for K
        let mut sum = 0;
        let mut i = 0;
        for (_, value) in &cur_vec {
            if i == k {
                break;
            }
            sum += value;
            i += 1;
        }

        sum
    }

    #[allow(dead_code)]
    fn reset(&mut self, player_id: i32) {
        // The player is ensured to exist in the board
        // Just set the score to 0
        self.record_map.insert(player_id, 0);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
