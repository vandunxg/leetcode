---
comments: true
difficulty: Medium
rating: 1418
source: Weekly Contest 306 Q2
tags:
    - Graph
    - Hash Table
---

<!-- problem:start -->

# [2374. Node With Highest Edge Score](https://leetcode.com/problems/node-with-highest-edge-score)

[中文文档](/solution/2300-2399/2374.Node%20With%20Highest%20Edge%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị có hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, trong đó mỗi node có <strong>đúng một</strong> cạnh đi ra.</p>

<p>Đồ thị được biểu diễn bằng một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>edges</code> có độ dài <code>n</code>, trong đó <code>edges[i]</code> cho biết có một cạnh <strong>có hướng</strong> từ node <code>i</code> đến node <code>edges[i]</code>.</p>

<p><strong>Điểm cạnh</strong> của node <code>i</code> được định nghĩa là tổng các <strong>nhãn</strong> của tất cả node có cạnh trỏ đến <code>i</code>.</p>

<p>Hãy trả về <em>node có <strong>điểm cạnh</strong> cao nhất</em>. Nếu có nhiều node có <strong>điểm cạnh</strong> bằng nhau, hãy trả về node có <strong>chỉ số</strong> nhỏ nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2374.Node%20With%20Highest%20Edge%20Score/images/image-20220620195403-1.png" style="width: 450px; height: 260px;" />
<pre>
<strong>Đầu vào:</strong> edges = [1,0,0,0,0,7,7,5]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
- Các node 1, 2, 3 và 4 có cạnh trỏ đến node 0. Điểm cạnh của node 0 là 1 + 2 + 3 + 4 = 10.
- Node 0 có cạnh trỏ đến node 1. Điểm cạnh của node 1 là 0.
- Node 7 có cạnh trỏ đến node 5. Điểm cạnh của node 5 là 7.
- Các node 5 và 6 có cạnh trỏ đến node 7. Điểm cạnh của node 7 là 5 + 6 = 11.
Node 7 có điểm cạnh cao nhất, nên ta trả về 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2374.Node%20With%20Highest%20Edge%20Score/images/image-20220620200212-3.png" style="width: 150px; height: 155px;" />
<pre>
<strong>Đầu vào:</strong> edges = [2,0,0,2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
- Các node 1 và 2 có cạnh trỏ đến node 0. Điểm cạnh của node 0 là 1 + 2 = 3.
- Các node 0 và 3 có cạnh trỏ đến node 2. Điểm cạnh của node 2 là 0 + 3 = 3.
Node 0 và node 2 đều có điểm cạnh bằng 3. Vì node 0 có chỉ số nhỏ hơn, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= edges[i] &lt; n</code></li>
	<li><code>edges[i] != i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Điểm cạnh của một node là tổng các chỉ số trỏ đến nó. Vì $n \le 10^5$, chỉ cần cộng dồn một lần; khi hòa, chọn chỉ số nhỏ hơn.
>
> Cộng node nguồn $i$ vào $cnt[j]$ và so sánh với đáp án hiện tại ngay trong quá trình duyệt, tránh phải duyệt lần hai.

<!-- thinking:end -->

Ta định nghĩa một mảng $\textit{cnt}$ có độ dài $n$, trong đó $\textit{cnt}[i]$ biểu thị điểm cạnh của node $i$. Ban đầu, tất cả phần tử đều bằng $0$. Ta cũng định nghĩa biến kết quả $\textit{ans}$, ban đầu bằng $0$.

Tiếp theo, ta duyệt mảng $\textit{edges}$. Với mỗi node $i$ và node $j$ ở đầu cạnh đi ra của nó, ta cập nhật $\textit{cnt}[j]$ thành $\textit{cnt}[j] + i$. Nếu $\textit{cnt}[\textit{ans}] < \textit{cnt}[j]$ hoặc $\textit{cnt}[\textit{ans}] = \textit{cnt}[j]$ và $j < \textit{ans}$, ta cập nhật $\textit{ans}$ thành $j$.

Cuối cùng, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{edges}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def edgeScore(self, edges: List[int]) -> int:
        ans = 0
        cnt = [0] * len(edges)
        for i, j in enumerate(edges):
            cnt[j] += i
            if cnt[ans] < cnt[j] or (cnt[ans] == cnt[j] and j < ans):
                ans = j
        return ans
```

#### Java

```java
class Solution {
    public int edgeScore(int[] edges) {
        int n = edges.length;
        long[] cnt = new long[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int j = edges[i];
            cnt[j] += i;
            if (cnt[ans] < cnt[j] || (cnt[ans] == cnt[j] && j < ans)) {
                ans = j;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int edgeScore(vector<int>& edges) {
        int n = edges.size();
        vector<long long> cnt(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int j = edges[i];
            cnt[j] += i;
            if (cnt[ans] < cnt[j] || (cnt[ans] == cnt[j] && j < ans)) {
                ans = j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func edgeScore(edges []int) (ans int) {
	cnt := make([]int, len(edges))
	for i, j := range edges {
		cnt[j] += i
		if cnt[ans] < cnt[j] || (cnt[ans] == cnt[j] && j < ans) {
			ans = j
		}
	}
	return
}
```

#### TypeScript

```ts
function edgeScore(edges: number[]): number {
    const n = edges.length;
    const cnt: number[] = Array(n).fill(0);
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        const j = edges[i];
        cnt[j] += i;
        if (cnt[ans] < cnt[j] || (cnt[ans] === cnt[j] && j < ans)) {
            ans = j;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn edge_score(edges: Vec<i32>) -> i32 {
        let n = edges.len();
        let mut cnt = vec![0_i64; n];
        let mut ans = 0;

        for (i, &j) in edges.iter().enumerate() {
            let j = j as usize;
            cnt[j] += i as i64;
            if cnt[ans] < cnt[j] || (cnt[ans] == cnt[j] && j < ans) {
                ans = j;
            }
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
