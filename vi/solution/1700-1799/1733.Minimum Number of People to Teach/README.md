---
comments: true
difficulty: Medium
rating: 1983
source: Biweekly Contest 44 Q2
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1733. Minimum Number of People to Teach](https://leetcode.com/problems/minimum-number-of-people-to-teach)

[中文文档](/solution/1700-1799/1733.Minimum%20Number%20of%20People%20to%20Teach/README.md)

## Mô tả

<!-- description:start -->

<p>Trên một mạng xã hội gồm <code>m</code> người dùng và một số quan hệ bạn bè, hai người dùng có thể giao tiếp nếu họ biết chung một ngôn ngữ.</p>

<p>Bạn được cho số nguyên <code>n</code>, mảng <code>languages</code> và mảng <code>friendships</code>, trong đó:</p>

<ul>
	<li>Có <code>n</code> ngôn ngữ được đánh số từ <code>1</code> đến <code>n</code>,</li>
	<li><code>languages[i]</code> là tập hợp các ngôn ngữ mà người dùng thứ <code>i<sup>​​​​​​th</sup></code>​​​​ biết, và</li>
	<li><code>friendships[i] = [u<sub>​​​​​​i</sub>​​​, v<sub>​​​​​​i</sub>]</code> biểu diễn quan hệ bạn bè giữa người dùng <code>u<sup>​​​​​</sup><sub>​​​​​​i</sub></code>​​​​​ và <code>v<sub>i</sub></code>.</li>
</ul>

<p>Bạn có thể chọn <strong>một</strong> ngôn ngữ và dạy ngôn ngữ đó cho một số người dùng để tất cả bạn bè có thể giao tiếp. Trả về <i data-stringify-type="italic">số người dùng </i><i><strong>ít nhất</strong> </i><i data-stringify-type="italic">cần dạy.</i></p>
Lưu ý rằng quan hệ bạn bè không có tính bắc cầu: nếu <code>x</code> là bạn của <code>y</code> và <code>y</code> là bạn của <code>z</code>, điều đó không đảm bảo <code>x</code> là bạn của <code>z</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Bạn có thể dạy ngôn ngữ thứ hai cho người dùng 1 hoặc ngôn ngữ thứ nhất cho người dùng 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Dạy ngôn ngữ thứ ba cho người dùng 1 và 3, nên cần dạy cho hai người dùng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 500</code></li>
	<li><code>languages.length == m</code></li>
	<li><code>1 &lt;= m &lt;= 500</code></li>
	<li><code>1 &lt;= languages[i].length &lt;= n</code></li>
	<li><code>1 &lt;= languages[i][j] &lt;= n</code></li>
	<li><code>1 &lt;= u<sub>​​​​​​i</sub> &lt; v<sub>​​​​​​i</sub> &lt;= languages.length</code></li>
	<li><code>1 &lt;= friendships.length &lt;= 500</code></li>
	<li>Tất cả bộ <code>(u<sub>​​​​​i, </sub>v<sub>​​​​​​i</sub>)</code> đều khác nhau</li>
	<li><code>languages[i]</code> chỉ chứa các giá trị khác nhau</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Thống kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ được dạy một ngôn ngữ, và chỉ những cặp bạn hiện không thể nói chuyện mới cần hỗ trợ. Quan hệ bạn bè không bắc cầu nên các liên kết gián tiếp không quan trọng.
>
> Thu thập hai đầu mút của các cặp có tập ngôn ngữ rời nhau; những người đó tạo thành tập $s$ cần xét.
>
> Đếm số người trong $s$ đã biết từng ngôn ngữ. Dạy ngôn ngữ xuất hiện nhiều nhất tốn $|s|$ trừ đi số lượng lớn nhất đó.

<!-- thinking:end -->

Với mỗi quan hệ bạn bè, nếu hai tập ngôn ngữ của hai người không giao nhau, ta cần dạy một ngôn ngữ để họ có thể giao tiếp. Ta thêm những người này vào hash set $s$.

Sau đó, với mỗi ngôn ngữ, ta đếm số người trong tập $s$ biết ngôn ngữ đó và tìm số lượng lớn nhất, ký hiệu là $mx$. Đáp án là $|s| - mx$, trong đó $|s|$ là kích thước của tập $s$.

Độ phức tạp thời gian là $O(m^2 \times k)$. Ở đây, $m$ là số ngôn ngữ và $k$ là số quan hệ bạn bè.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTeachings(
        self, n: int, languages: List[List[int]], friendships: List[List[int]]
    ) -> int:
        def check(u: int, v: int) -> bool:
            for x in languages[u - 1]:
                for y in languages[v - 1]:
                    if x == y:
                        return True
            return False

        s = set()
        for u, v in friendships:
            if not check(u, v):
                s.add(u)
                s.add(v)
        cnt = Counter()
        for u in s:
            for l in languages[u - 1]:
                cnt[l] += 1
        return len(s) - max(cnt.values(), default=0)
```

#### Java

```java
class Solution {
    public int minimumTeachings(int n, int[][] languages, int[][] friendships) {
        Set<Integer> s = new HashSet<>();
        for (var e : friendships) {
            int u = e[0], v = e[1];
            if (!check(u, v, languages)) {
                s.add(u);
                s.add(v);
            }
        }
        if (s.isEmpty()) {
            return 0;
        }
        int[] cnt = new int[n + 1];
        for (int u : s) {
            for (int l : languages[u - 1]) {
                ++cnt[l];
            }
        }
        int mx = 0;
        for (int v : cnt) {
            mx = Math.max(mx, v);
        }
        return s.size() - mx;
    }

    private boolean check(int u, int v, int[][] languages) {
        for (int x : languages[u - 1]) {
            for (int y : languages[v - 1]) {
                if (x == y) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        unordered_set<int> s;
        auto check = [&](int u, int v) {
            for (int x : languages[u - 1]) {
                for (int y : languages[v - 1]) {
                    if (x == y) {
                        return true;
                    }
                }
            }
            return false;
        };
        for (auto& e : friendships) {
            int u = e[0], v = e[1];
            if (!check(u, v)) {
                s.insert(u);
                s.insert(v);
            }
        }
        if (s.empty()) {
            return 0;
        }
        vector<int> cnt(n + 1);
        for (int u : s) {
            for (int& l : languages[u - 1]) {
                ++cnt[l];
            }
        }
        return s.size() - ranges::max(cnt);
    }
};
```

#### Go

```go
func minimumTeachings(n int, languages [][]int, friendships [][]int) int {
	check := func(u, v int) bool {
		for _, x := range languages[u-1] {
			for _, y := range languages[v-1] {
				if x == y {
					return true
				}
			}
		}
		return false
	}
	s := map[int]bool{}
	for _, e := range friendships {
		u, v := e[0], e[1]
		if !check(u, v) {
			s[u], s[v] = true, true
		}
	}
	if len(s) == 0 {
		return 0
	}
	cnt := make([]int, n+1)
	for u := range s {
		for _, l := range languages[u-1] {
			cnt[l]++
		}
	}
	return len(s) - slices.Max(cnt)
}
```

#### TypeScript

```ts
function minimumTeachings(n: number, languages: number[][], friendships: number[][]): number {
    function check(u: number, v: number): boolean {
        for (const x of languages[u - 1]) {
            for (const y of languages[v - 1]) {
                if (x === y) {
                    return true;
                }
            }
        }
        return false;
    }

    const s = new Set<number>();
    for (const [u, v] of friendships) {
        if (!check(u, v)) {
            s.add(u);
            s.add(v);
        }
    }

    const cnt = new Map<number, number>();
    for (const u of s) {
        for (const l of languages[u - 1]) {
            cnt.set(l, (cnt.get(l) || 0) + 1);
        }
    }

    return s.size - Math.max(0, ...cnt.values());
}
```

#### Rust

```rust
use std::collections::{HashSet, HashMap};

impl Solution {
    pub fn minimum_teachings(n: i32, languages: Vec<Vec<i32>>, friendships: Vec<Vec<i32>>) -> i32 {
        fn check(u: usize, v: usize, languages: &Vec<Vec<i32>>) -> bool {
            for &x in &languages[u - 1] {
                for &y in &languages[v - 1] {
                    if x == y {
                        return true;
                    }
                }
            }
            false
        }

        let mut s: HashSet<usize> = HashSet::new();
        for edge in friendships.iter() {
            let u = edge[0] as usize;
            let v = edge[1] as usize;
            if !check(u, v, &languages) {
                s.insert(u);
                s.insert(v);
            }
        }

        let mut cnt: HashMap<i32, i32> = HashMap::new();
        for &u in s.iter() {
            for &l in &languages[u - 1] {
                *cnt.entry(l).or_insert(0) += 1;
            }
        }

        let mx = cnt.values().cloned().max().unwrap_or(0);
        (s.len() as i32) - mx
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
