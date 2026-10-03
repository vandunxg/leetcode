---
comments: true
difficulty: Medium
rating: 1360
source: Weekly Contest 235 Q2
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1817. Finding the Users Active Minutes](https://leetcode.com/problems/finding-the-users-active-minutes)

[中文文档](/solution/1800-1899/1817.Finding%20the%20Users%20Active%20Minutes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho log các thao tác của người dùng trên LeetCode và một số nguyên <code>k</code>. Log được biểu diễn bằng mảng số nguyên 2D <code>logs</code>, trong đó mỗi <code>logs[i] = [ID<sub>i</sub>, time<sub>i</sub>]</code> cho biết người dùng có <code>ID<sub>i</sub></code> đã thực hiện một thao tác tại phút <code>time<sub>i</sub></code>.</p>

<p><strong>Nhiều người dùng</strong> có thể thực hiện thao tác đồng thời và một người dùng có thể thực hiện <strong>nhiều thao tác</strong> trong cùng một phút.</p>

<p><strong>Số phút hoạt động của người dùng (UAM)</strong> được định nghĩa là <strong>số phút khác nhau</strong> mà người dùng đã thực hiện thao tác trên LeetCode. Một phút chỉ được tính một lần, kể cả khi trong phút đó có nhiều thao tác.</p>

<p>Hãy tính mảng <code>answer</code> đánh chỉ số từ <strong>1</strong>, có kích thước <code>k</code>, sao cho với mỗi <code>j</code> (<code>1 &lt;= j &lt;= k</code>), <code>answer[j]</code> là <strong>số người dùng</strong> có <strong>UAM</strong> bằng <code>j</code>.</p>

<p>Trả về <i>mảng </i><code>answer</code><i> như mô tả ở trên</i>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [[0,5],[1,2],[0,2],[0,5],[1,3]], k = 5
<strong>Đầu ra:</strong> [0,2,0,0,0]
<strong>Giải thích:</strong>
Người dùng có ID=0 đã thực hiện thao tác ở các phút 5, 2 và 5 một lần nữa. Vì vậy, UAM của họ là 2 (phút 5 chỉ được tính một lần).
Người dùng có ID=1 đã thực hiện thao tác ở các phút 2 và 3. Vì vậy, UAM của họ là 2.
Vì cả hai người dùng đều có UAM bằng 2, answer[2] bằng 2 và các giá trị answer[j] còn lại bằng 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [[1,1],[2,2],[2,3]], k = 4
<strong>Đầu ra:</strong> [1,1,0,0]
<strong>Giải thích:</strong>
Người dùng có ID=1 đã thực hiện một thao tác tại phút 1. Vì vậy, UAM của họ là 1.
Người dùng có ID=2 đã thực hiện thao tác tại các phút 2 và 3. Vì vậy, UAM của họ là 2.
Có một người dùng có UAM bằng 1 và một người dùng có UAM bằng 2.
Do đó, answer[1] = 1, answer[2] = 1 và các giá trị còn lại bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= logs.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= ID<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= time<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>k</code> nằm trong khoảng <code>[The maximum <strong>UAM</strong> for a user, 10<sup>5</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> UAM của một người dùng là số phút khác nhau mà họ hoạt động; ta cần đếm số người dùng có từng UAM. Các cặp $(user,time)$ trùng nhau không được làm tăng kết quả.
>
> Ánh xạ mỗi người dùng tới một set các mốc thời gian; kích thước set chính là UAM của người dùng đó. Tăng $\textit{ans}[UAM-1]$ trong mảng có độ dài $k$. Chỉ cần một lượt duyệt qua log.

<!-- thinking:end -->

Ta dùng hash table $d$ để ghi nhận tất cả thời điểm thao tác khác nhau của mỗi người dùng, sau đó duyệt hash table để đếm số phút hoạt động của từng người dùng. Cuối cùng, ta đếm phân bố số phút hoạt động của các người dùng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $logs$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findingUsersActiveMinutes(self, logs: List[List[int]], k: int) -> List[int]:
        d = defaultdict(set)
        for i, t in logs:
            d[i].add(t)
        ans = [0] * k
        for ts in d.values():
            ans[len(ts) - 1] += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] findingUsersActiveMinutes(int[][] logs, int k) {
        Map<Integer, Set<Integer>> d = new HashMap<>();
        for (var log : logs) {
            int i = log[0], t = log[1];
            d.computeIfAbsent(i, key -> new HashSet<>()).add(t);
        }
        int[] ans = new int[k];
        for (var ts : d.values()) {
            ++ans[ts.size() - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findingUsersActiveMinutes(vector<vector<int>>& logs, int k) {
        unordered_map<int, unordered_set<int>> d;
        for (auto& log : logs) {
            int i = log[0], t = log[1];
            d[i].insert(t);
        }
        vector<int> ans(k);
        for (auto& [_, ts] : d) {
            ++ans[ts.size() - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func findingUsersActiveMinutes(logs [][]int, k int) []int {
	d := map[int]map[int]bool{}
	for _, log := range logs {
		i, t := log[0], log[1]
		if _, ok := d[i]; !ok {
			d[i] = make(map[int]bool)
		}
		d[i][t] = true
	}
	ans := make([]int, k)
	for _, ts := range d {
		ans[len(ts)-1]++
	}
	return ans
}
```

#### TypeScript

```ts
function findingUsersActiveMinutes(logs: number[][], k: number): number[] {
    const d: Map<number, Set<number>> = new Map();
    for (const [i, t] of logs) {
        if (!d.has(i)) {
            d.set(i, new Set<number>());
        }
        d.get(i)!.add(t);
    }
    const ans: number[] = Array(k).fill(0);
    for (const [_, ts] of d) {
        ++ans[ts.size - 1];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
