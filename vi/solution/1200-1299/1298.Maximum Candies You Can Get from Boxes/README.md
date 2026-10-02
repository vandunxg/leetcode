---
comments: true
difficulty: Hard
rating: 1824
source: Weekly Contest 168 Q4
tags:
    - Breadth-First Search
    - Graph
    - Array
---

<!-- problem:start -->

# [1298. Maximum Candies You Can Get from Boxes](https://leetcode.com/problems/maximum-candies-you-can-get-from-boxes)

[中文文档](/solution/1200-1299/1298.Maximum%20Candies%20You%20Can%20Get%20from%20Boxes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> hộp được đánh số từ <code>0</code> đến <code>n - 1</code>, cùng bốn mảng <code>status</code>, <code>candies</code>, <code>keys</code> và <code>containedBoxes</code>, trong đó:</p>

<ul>
	<li><code>status[i]</code> bằng <code>1</code> nếu hộp thứ <code>i<sup>th</sup></code> đang mở và bằng <code>0</code> nếu hộp đó đang khóa,</li>
	<li><code>candies[i]</code> là số kẹo trong hộp thứ <code>i<sup>th</sup></code>,</li>
	<li><code>keys[i]</code> là danh sách nhãn các hộp mà bạn có thể mở sau khi mở hộp thứ <code>i<sup>th</sup></code>.</li>
	<li><code>containedBoxes[i]</code> là danh sách các hộp tìm thấy bên trong hộp thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Bạn được cho mảng số nguyên <code>initialBoxes</code> chứa nhãn các hộp ban đầu mình có. Bạn có thể lấy toàn bộ kẹo trong <strong>bất kỳ hộp nào đang mở</strong>, dùng chìa khóa bên trong để mở hộp khác và sử dụng những hộp tìm thấy bên trong.</p>

<p>Trả về <em>số kẹo tối đa bạn có thể lấy được theo các quy tắc trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> status = [1,0,1,0], candies = [7,5,4,100], keys = [[],[],[1],[]], containedBoxes = [[1,2],[3],[],[]], initialBoxes = [0]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Ban đầu bạn nhận được hộp 0. Trong đó có 7 viên kẹo và các hộp 1, 2.
Hộp 1 đang khóa và bạn chưa có chìa khóa, nên bạn mở hộp 2. Trong hộp 2 có 4 viên kẹo và chìa khóa mở hộp 1.
Trong hộp 1 có 5 viên kẹo và hộp 3, nhưng không có chìa khóa mở hộp 3 nên hộp này vẫn bị khóa.
Tổng số kẹo thu được = 7 + 4 + 5 = 16 viên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> status = [1,0,0,0,0,0], candies = [1,1,1,1,1,1], keys = [[1,2,3,4,5],[],[],[],[],[]], containedBoxes = [[1,2,3,4,5],[],[],[],[],[]], initialBoxes = [0]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ban đầu bạn có hộp 0. Mở hộp này sẽ tìm được các hộp 1, 2, 3, 4, 5 cùng chìa khóa của chúng.
Tổng số kẹo thu được là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == status.length == candies.length == keys.length == containedBoxes.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>status[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= candies[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= keys[i].length &lt;= n</code></li>
	<li><code>0 &lt;= keys[i][j] &lt; n</code></li>
	<li>Tất cả giá trị trong <code>keys[i]</code> đều <strong>khác nhau</strong>.</li>
	<li><code>0 &lt;= containedBoxes[i].length &lt;= n</code></li>
	<li><code>0 &lt;= containedBoxes[i][j] &lt; n</code></li>
	<li>Tất cả giá trị trong <code>containedBoxes[i]</code> đều khác nhau.</li>
	<li>Mỗi hộp nằm bên trong nhiều nhất một hộp khác.</li>
	<li><code>0 &lt;= initialBoxes.length &lt;= n</code></li>
	<li><code>0 &lt;= initialBoxes[i] &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Hash Set

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể mở một hộp khi ta đang giữ hộp đó và hộp đã mở khóa (hoặc ta có chìa khóa của nó). Hộp và chìa khóa có thể nằm lồng bên trong nhau; với $n \le 1000$, BFS trên các hộp hiện có thể mở là phù hợp.
>
> $has$ là tập các hộp ta đang có; $took$ giúp tránh xử lý lại. Queue chứa các hộp đã mở nhưng chưa xử lý: ta dùng chìa khóa trong hộp để mở khóa các hộp đang khóa và thêm các hộp bên trong vào $has$. Những hộp ban đầu đã mở được đưa vào queue và cộng kẹo; mỗi hộp mở thêm sau đó cũng chỉ được cộng kẹo một lần.

<!-- thinking:end -->

Bài toán cho một tập hộp, mỗi hộp có trạng thái mở/khóa, kẹo, chìa khóa và có thể chứa các hộp khác. Mục tiêu là dùng những hộp ban đầu để mở thêm nhiều hộp nhất có thể và lấy kẹo bên trong. Ta có thể mở khóa hộp mới khi tìm được chìa khóa, đồng thời nhận thêm tài nguyên từ các hộp nằm bên trong hộp khác.

Ta dùng BFS để mô phỏng toàn bộ quá trình khám phá.

Ta dùng queue $q$ để lưu các hộp hiện có thể lấy kẹo và đã **được mở**; hai set, $\textit{has}$ và $\textit{took}$, lần lượt lưu **tất cả hộp ta đang có** và **các hộp đã xử lý** để tránh trùng lặp.

Ban đầu, thêm toàn bộ $\textit{initialBoxes}$ vào $\textit{has}$. Nếu một hộp ban đầu đang mở, đưa nó ngay vào queue $\textit{q}$ và cộng số kẹo bên trong vào tổng.

Sau đó, thực hiện BFS bằng cách lần lượt lấy hộp khỏi $\textit{q}$:

- Lấy các chìa khóa trong hộp $\textit{keys[box]}$ và đưa những hộp có thể mở khóa vào queue;
- Thu thập các hộp bên trong qua $\textit{containedBoxes[box]}$. Nếu hộp bên trong đang mở và chưa được xử lý, xử lý ngay hộp đó;

Mỗi hộp được xử lý nhiều nhất một lần và kẹo của nó cũng chỉ được cộng một lần. Cuối cùng, trả về tổng số kẹo $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là tổng số hộp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCandies(
        self,
        status: List[int],
        candies: List[int],
        keys: List[List[int]],
        containedBoxes: List[List[int]],
        initialBoxes: List[int],
    ) -> int:
        q = deque()
        has, took = set(initialBoxes), set()
        ans = 0

        for box in initialBoxes:
            if status[box]:
                q.append(box)
                took.add(box)
                ans += candies[box]
        while q:
            box = q.popleft()
            for k in keys[box]:
                if not status[k]:
                    status[k] = 1
                    if k in has and k not in took:
                        q.append(k)
                        took.add(k)
                        ans += candies[k]

            for b in containedBoxes[box]:
                has.add(b)
                if status[b] and b not in took:
                    q.append(b)
                    took.add(b)
                    ans += candies[b]
        return ans
```

#### Java

```java
class Solution {
    public int maxCandies(
        int[] status, int[] candies, int[][] keys, int[][] containedBoxes, int[] initialBoxes) {
        Deque<Integer> q = new ArrayDeque<>();
        Set<Integer> has = new HashSet<>();
        Set<Integer> took = new HashSet<>();
        int ans = 0;
        for (int box : initialBoxes) {
            has.add(box);
            if (status[box] == 1) {
                q.offer(box);
                took.add(box);
                ans += candies[box];
            }
        }
        while (!q.isEmpty()) {
            int box = q.poll();
            for (int k : keys[box]) {
                if (status[k] == 0) {
                    status[k] = 1;
                    if (has.contains(k) && !took.contains(k)) {
                        q.offer(k);
                        took.add(k);
                        ans += candies[k];
                    }
                }
            }
            for (int b : containedBoxes[box]) {
                has.add(b);
                if (status[b] == 1 && !took.contains(b)) {
                    q.offer(b);
                    took.add(b);
                    ans += candies[b];
                }
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
    int maxCandies(
        vector<int>& status,
        vector<int>& candies,
        vector<vector<int>>& keys,
        vector<vector<int>>& containedBoxes,
        vector<int>& initialBoxes) {
        queue<int> q;
        unordered_set<int> has, took;
        int ans = 0;

        for (int box : initialBoxes) {
            has.insert(box);
            if (status[box]) {
                q.push(box);
                took.insert(box);
                ans += candies[box];
            }
        }

        while (!q.empty()) {
            int box = q.front();
            q.pop();

            for (int k : keys[box]) {
                if (!status[k]) {
                    status[k] = 1;
                    if (has.count(k) && !took.count(k)) {
                        q.push(k);
                        took.insert(k);
                        ans += candies[k];
                    }
                }
            }

            for (int b : containedBoxes[box]) {
                has.insert(b);
                if (status[b] && !took.count(b)) {
                    q.push(b);
                    took.insert(b);
                    ans += candies[b];
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxCandies(status []int, candies []int, keys [][]int, containedBoxes [][]int, initialBoxes []int) (ans int) {
	q := []int{}
	has := make(map[int]bool)
	took := make(map[int]bool)
	for _, box := range initialBoxes {
		has[box] = true
		if status[box] == 1 {
			q = append(q, box)
			took[box] = true
			ans += candies[box]
		}
	}
	for len(q) > 0 {
		box := q[0]
		q = q[1:]
		for _, k := range keys[box] {
			if status[k] == 0 {
				status[k] = 1
				if has[k] && !took[k] {
					q = append(q, k)
					took[k] = true
					ans += candies[k]
				}
			}
		}
		for _, b := range containedBoxes[box] {
			has[b] = true
			if status[b] == 1 && !took[b] {
				q = append(q, b)
				took[b] = true
				ans += candies[b]
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function maxCandies(
    status: number[],
    candies: number[],
    keys: number[][],
    containedBoxes: number[][],
    initialBoxes: number[],
): number {
    const q: number[] = [];
    const has: Set<number> = new Set();
    const took: Set<number> = new Set();
    let ans = 0;

    for (const box of initialBoxes) {
        has.add(box);
        if (status[box] === 1) {
            q.push(box);
            took.add(box);
            ans += candies[box];
        }
    }

    while (q.length > 0) {
        const box = q.pop()!;

        for (const k of keys[box]) {
            if (status[k] === 0) {
                status[k] = 1;
                if (has.has(k) && !took.has(k)) {
                    q.push(k);
                    took.add(k);
                    ans += candies[k];
                }
            }
        }

        for (const b of containedBoxes[box]) {
            has.add(b);
            if (status[b] === 1 && !took.has(b)) {
                q.push(b);
                took.add(b);
                ans += candies[b];
            }
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn max_candies(
        mut status: Vec<i32>,
        candies: Vec<i32>,
        keys: Vec<Vec<i32>>,
        contained_boxes: Vec<Vec<i32>>,
        initial_boxes: Vec<i32>,
    ) -> i32 {
        let mut q: VecDeque<i32> = VecDeque::new();
        let mut has: HashSet<i32> = HashSet::new();
        let mut took: HashSet<i32> = HashSet::new();
        let mut ans = 0;

        for &box_ in &initial_boxes {
            has.insert(box_);
            if status[box_ as usize] == 1 {
                q.push_back(box_);
                took.insert(box_);
                ans += candies[box_ as usize];
            }
        }

        while let Some(box_) = q.pop_front() {
            for &k in &keys[box_ as usize] {
                if status[k as usize] == 0 {
                    status[k as usize] = 1;
                    if has.contains(&k) && !took.contains(&k) {
                        q.push_back(k);
                        took.insert(k);
                        ans += candies[k as usize];
                    }
                }
            }

            for &b in &contained_boxes[box_ as usize] {
                has.insert(b);
                if status[b as usize] == 1 && !took.contains(&b) {
                    q.push_back(b);
                    took.insert(b);
                    ans += candies[b as usize];
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
