---
comments: true
difficulty: Hard
rating: 2476
source: Biweekly Contest 164 Q4
tags:
    - Breadth-First Search
    - Union Find
    - Math
    - String
    - Ordered Set
---

<!-- problem:start -->

# [3666. Minimum Operations to Equalize Binary String](https://leetcode.com/problems/minimum-operations-to-equalize-binary-string)

[中文文档](/solution/3600-3699/3666.Minimum%20Operations%20to%20Equalize%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn phải chọn <strong>chính xác</strong> <code>k</code> chỉ số <strong>khác nhau</strong> và <strong>flip</strong> từng ký tự <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code> và từng ký tự <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>.</p>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến tất cả ký tự trong chuỗi thành <code>&#39;1&#39;</code>. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;110&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Có một ký tự <code>&#39;0&#39;</code> trong <code>s</code>.</li>
	<li>Vì <code>k = 1</code>, ta có thể flip trực tiếp ký tự đó trong một thao tác.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0101&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập thao tác tối ưu, trong đó chọn <code>k = 3</code> chỉ số ở mỗi thao tác, là:</p>

<ul>
	<li><strong>Thao tác 1</strong>: Flip các chỉ số <code>[0, 1, 3]</code>. <code>s</code> thay đổi từ <code>&quot;0101&quot;</code> thành <code>&quot;1000&quot;</code>.</li>
	<li><strong>Thao tác 2</strong>: Flip các chỉ số <code>[1, 2, 3]</code>. <code>s</code> thay đổi từ <code>&quot;1000&quot;</code> thành <code>&quot;1111&quot;</code>.</li>
</ul>

<p>Vậy số thao tác ít nhất là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;101&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>k = 2</code> và <code>s</code> chỉ có một ký tự <code>&#39;0&#39;</code>, ta không thể flip chính xác <code>k</code> chỉ số để biến tất cả thành <code>&#39;1&#39;</code>. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>​​​​​​​5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác flip chính xác $k$ vị trí và mục tiêu là chuỗi toàn ký tự 1. Trạng thái có thể thu gọn thành số lượng số 0: vị trí cụ thể của các số 0 không làm thay đổi khoảng giá trị có thể đạt được.
>
> Với $\textit{cur}$ số 0, ta flip $x$ số 0, khi đó số lượng số 0 trở thành $\textit{cur}+k-2x$ với $x\in[\max(k-n+\textit{cur},0),\min(\textit{cur},k)]$. Các giá trị này tạo thành một khoảng các số có cùng parity.
>
> Lưu các số lượng chưa thăm trong hai ordered set, chia theo parity. BFS lấy $\textit{cur}$ ra và đưa vào queue mọi giá trị còn lại trong $[l,r]$. Tầng chạm tới $0$ là đáp án; nếu duyệt hết thì không thể thực hiện.

<!-- thinking:end -->

Ký hiệu độ dài của chuỗi $s$ là $n$, và số lượng ký tự '0' hiện tại trong chuỗi là $\textit{cur}$. Trong mỗi thao tác, ta chọn $k$ chỉ số để flip, trong đó có $x$ chỉ số flip từ '0' thành '1', và $k-x$ chỉ số flip từ '1' thành '0'. Khi đó số lượng ký tự '0' trong chuỗi sau khi flip là $\textit{cur} + k - 2x$.

Giá trị của $x$ cần thỏa mãn các điều kiện sau:

1. Có thể chọn nhiều nhất $\min(\textit{cur}, k)$ ký tự '0', vì không thể flip nhiều hơn $\textit{cur}$ ký tự '0', do đó $0 \leq x \leq \min(\textit{cur}, k)$.
2. Có thể chọn nhiều nhất $n - \textit{cur}$ ký tự '1', vì không thể flip nhiều hơn $n - \textit{cur}$ ký tự '1', do đó $k - x \leq n - \textit{cur}$, hay $x \geq k - n + \textit{cur}$.

Vì vậy, miền giá trị của $x$ là $[\max(k - n + \textit{cur}, 0), \min(\textit{cur}, k)]$, và miền số lượng ký tự '0' trong chuỗi sau khi flip là $[\textit{cur} + k - 2 \cdot \min(\textit{cur}, k), \textit{cur} + k - 2 \cdot \max(k - n + \textit{cur}, 0)]$.

Ta nhận thấy parity của số lượng ký tự '0' trong chuỗi sau khi flip giống với parity của số lượng ký tự '0' trước khi flip. Vì vậy, ta có thể dùng hai ordered set để lưu các trạng thái có số lượng ký tự '0' lần lượt là chẵn và lẻ.

Ta dùng BFS để tìm kiếm trên đồ thị chuyển trạng thái, trong đó trạng thái ban đầu là số lượng ký tự '0' trong chuỗi, còn trạng thái đích là 0. Mỗi lần lấy một trạng thái $\textit{cur}$ khỏi queue, ta tính miền $[l, r]$ của số lượng ký tự '0' sau khi flip, tìm tất cả trạng thái trong miền $[l, r]$ trong ordered set, thêm chúng vào queue, rồi xóa chúng khỏi ordered set.

Nếu gặp trạng thái 0 trong quá trình BFS, ta trả về số thao tác hiện tại; nếu BFS kết thúc mà chưa thăm trạng thái 0, ta trả về -1.

Độ phức tạp thời gian là $O(n \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $O(n)$ là số trạng thái có thể được thăm trong quá trình BFS, còn $O(\log n)$ là độ phức tạp thời gian để chèn và xóa phần tử trong ordered set.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s: str, k: int) -> int:
        n = len(s)
        ts = [SortedSet() for _ in range(2)]
        for i in range(n + 1):
            ts[i % 2].add(i)
        cnt0 = s.count('0')
        ts[cnt0 % 2].remove(cnt0)
        q = deque([cnt0])
        ans = 0
        while q:
            for _ in range(len(q)):
                cur = q.popleft()
                if cur == 0:
                    return ans
                l = cur + k - 2 * min(cur, k)
                r = cur + k - 2 * max(k - n + cur, 0)
                t = ts[l % 2]
                j = t.bisect_left(l)
                while j < len(t) and t[j] <= r:
                    q.append(t[j])
                    t.remove(t[j])
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int minOperations(String s, int k) {
        int n = s.length();

        TreeSet<Integer>[] ts = new TreeSet[2];
        Arrays.setAll(ts, i -> new TreeSet<>());

        for (int i = 0; i <= n; i++) {
            ts[i % 2].add(i);
        }

        int cnt0 = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') {
                cnt0++;
            }
        }

        ts[cnt0 % 2].remove(cnt0);

        Deque<Integer> q = new ArrayDeque<>();
        q.offer(cnt0);

        int ans = 0;
        while (!q.isEmpty()) {
            for (int size = q.size(); size > 0; --size) {
                int cur = q.poll();
                if (cur == 0) {
                    return ans;
                }

                int l = cur + k - 2 * Math.min(cur, k);
                int r = cur + k - 2 * Math.max(k - n + cur, 0);

                TreeSet<Integer> t = ts[l % 2];

                Integer next = t.ceiling(l);
                while (next != null && next <= r) {
                    q.offer(next);
                    t.remove(next);
                    next = t.ceiling(l);
                }
            }
            ans++;
        }

        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string s, int k) {
        int n = s.size();

        set<int> ts[2];
        for (int i = 0; i <= n; i++) {
            ts[i % 2].insert(i);
        }

        int cnt0 = count(s.begin(), s.end(), '0');
        ts[cnt0 % 2].erase(cnt0);

        queue<int> q;
        q.push(cnt0);

        int ans = 0;

        while (!q.empty()) {
            for (int size = q.size(); size > 0; --size) {
                int cur = q.front();
                q.pop();
                if (cur == 0) {
                    return ans;
                }

                int l = cur + k - 2 * min(cur, k);
                int r = cur + k - 2 * max(k - n + cur, 0);

                auto& t = ts[l % 2];
                auto it = t.lower_bound(l);

                while (it != t.end() && *it <= r) {
                    q.push(*it);
                    it = t.erase(it);
                }
            }
            ans++;
        }

        return -1;
    }
};
```

#### Go

```go
func minOperations(s string, k int) int {
	n := len(s)

	ts := [2]*redblacktree.Tree{
		redblacktree.NewWithIntComparator(),
		redblacktree.NewWithIntComparator(),
	}

	for i := 0; i <= n; i++ {
		ts[i%2].Put(i, struct{}{})
	}

	cnt0 := strings.Count(s, "0")
	ts[cnt0%2].Remove(cnt0)

	q := []int{cnt0}
	ans := 0

	for len(q) > 0 {
		nq := []int{}

		for _, cur := range q {
			if cur == 0 {
				return ans
			}

			l := cur + k - 2*min(cur, k)
			r := cur + k - 2*max(k-n+cur, 0)
			t := ts[l%2]

			node, found := t.Ceiling(l)
			for found && node.Key.(int) <= r {
				val := node.Key.(int)
				nq = append(nq, val)
				t.Remove(val)
				node, found = t.Ceiling(l)
			}
		}

		q = nq
		ans++
	}

	return -1
}
```

#### TypeScript

```ts
import { AvlTree } from '@datastructures-js/binary-search-tree';

function minOperations(s: string, k: number): number {
    const n: number = s.length;

    const ts = [new AvlTree<number>(), new AvlTree<number>()];

    for (let i = 0; i <= n; i++) {
        ts[i % 2].insert(i);
    }

    let cnt0 = 0;
    for (const c of s) {
        if (c === '0') cnt0++;
    }

    ts[cnt0 % 2].remove(cnt0);

    let q: number[] = [cnt0];
    let ans = 0;

    while (q.length > 0) {
        const nq: number[] = [];

        for (const cur of q) {
            if (cur === 0) {
                return ans;
            }

            const l = cur + k - 2 * Math.min(cur, k);
            const r = cur + k - 2 * Math.max(k - n + cur, 0);

            const t = ts[l % 2];
            let node = t.upperBound(l, true);
            while (node && node.getValue() <= r) {
                const val = node.getValue();
                nq.push(val);
                t.remove(val);
                node = t.upperBound(l, false);
            }
        }

        q = nq;
        ans++;
    }

    return -1;
}
```

#### Rust

```rust
use std::collections::{BTreeSet, VecDeque};

impl Solution {
    pub fn min_operations(s: String, k: i32) -> i32 {
        let n: i32 = s.len() as i32;
        let k: i32 = k;

        let mut ts: [BTreeSet<i32>; 2] = [BTreeSet::new(), BTreeSet::new()];
        for i in 0..=n {
            ts[(i % 2) as usize].insert(i);
        }

        let cnt0: i32 = s.bytes().filter(|&c| c == b'0').count() as i32;
        ts[(cnt0 % 2) as usize].remove(&cnt0);

        let mut q: VecDeque<i32> = VecDeque::new();
        q.push_back(cnt0);

        let mut ans: i32 = 0;

        while !q.is_empty() {
            let size = q.len();
            for _ in 0..size {
                let cur = q.pop_front().unwrap();
                if cur == 0 {
                    return ans;
                }

                let l = cur + k - 2 * cur.min(k);
                let r = cur + k - 2 * (k - n + cur).max(0);

                let parity = (l % 2) as usize;

                let vals: Vec<i32> = ts[parity]
                    .range(l..=r)
                    .cloned()
                    .collect();

                for v in vals {
                    q.push_back(v);
                    ts[parity].remove(&v);
                }
            }
            ans += 1;
        }

        -1
    }
}
```

#### C#

```cs
public class Solution {
    public int MinOperations(string s, int k) {
        int n = s.Length;

        var ts = new SortedSet<int>[2];
        ts[0] = new SortedSet<int>();
        ts[1] = new SortedSet<int>();

        for (int i = 0; i <= n; i++) {
            ts[i % 2].Add(i);
        }

        int cnt0 = 0;
        foreach (char c in s) {
            if (c == '0') {
                cnt0++;
            }
        }

        ts[cnt0 % 2].Remove(cnt0);

        var q = new Queue<int>();
        q.Enqueue(cnt0);

        int ans = 0;

        while (q.Count > 0) {
            int size = q.Count;
            for (int i = 0; i < size; i++) {
                int cur = q.Dequeue();
                if (cur == 0) {
                    return ans;
                }

                int l = cur + k - 2 * Math.Min(cur, k);
                int r = cur + k - 2 * Math.Max(k - n + cur, 0);

                var t = ts[l % 2];

                var toRemove = new List<int>();
                foreach (int next in t.GetViewBetween(l, r)) {
                    q.Enqueue(next);
                    toRemove.Add(next);
                }

                foreach (int next in toRemove) {
                    t.Remove(next);
                }
            }
            ans++;
        }

        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
