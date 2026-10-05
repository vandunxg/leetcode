---
comments: true
difficulty: Easy
rating: 1287
source: Weekly Contest 475 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3740. Minimum Distance Between Three Equal Elements I](https://leetcode.com/problems/minimum-distance-between-three-equal-elements-i)

[Tài liệu tiếng Trung](/solution/3700-3799/3740.Minimum%20Distance%20Between%20Three%20Equal%20Elements%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một bộ ba <code>(i, j, k)</code> gồm 3 chỉ số <strong>đôi một khác nhau</strong> được gọi là <strong>hợp lệ</strong> nếu <code>nums[i] == nums[j] == nums[k]</code>.</p>

<p><strong>Khoảng cách</strong> của một bộ ba <strong>hợp lệ</strong> là <code>abs(i - j) + abs(j - k) + abs(k - i)</code>, trong đó <code>abs(x)</code> là <strong>giá trị tuyệt đối</strong> của <code>x</code>.</p>

<p>Hãy trả về một số nguyên biểu thị <strong>khoảng cách</strong> <strong>nhỏ nhất</strong> có thể của một bộ ba <strong>hợp lệ</strong>. Nếu không tồn tại bộ ba <strong>hợp lệ</strong>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khoảng cách nhỏ nhất đạt được với bộ ba hợp lệ <code>(0, 2, 3)</code>.</p>

<p><code>(0, 2, 3)</code> là một bộ ba hợp lệ vì <code>nums[0] == nums[2] == nums[3] == 1</code>. Khoảng cách của nó là <code>abs(0 - 2) + abs(2 - 3) + abs(3 - 0) = 2 + 1 + 3 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,3,2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khoảng cách nhỏ nhất đạt được với bộ ba hợp lệ <code>(2, 4, 6)</code>.</p>

<p><code>(2, 4, 6)</code> là một bộ ba hợp lệ vì <code>nums[2] == nums[4] == nums[6] == 2</code>. Khoảng cách của nó là <code>abs(2 - 4) + abs(4 - 6) + abs(6 - 2) = 2 + 2 + 4 = 8</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại bộ ba hợp lệ nào. Vì vậy, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với các chỉ số đã sắp xếp, khoảng cách của bộ ba bằng $2(k-i)$, nên bộ ba tốt nhất của một giá trị là ba lần xuất hiện liên tiếp. Gom các chỉ số theo giá trị rồi duyệt một cửa sổ độ dài $3$ trên mỗi danh sách; $n\le 100$ nên cách này hoàn toàn phù hợp.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{g}$ để lưu danh sách các chỉ số của mỗi số trong mảng. Khi duyệt mảng, ta thêm chỉ số của mỗi số vào danh sách tương ứng của nó trong hash table. Đặt biến $\textit{ans}$ để lưu đáp án, với giá trị ban đầu là vô cực $\infty$.

Tiếp theo, ta duyệt qua từng danh sách chỉ số trong hash table. Nếu độ dài danh sách chỉ số của một số nào đó lớn hơn hoặc bằng $3$, nghĩa là tồn tại một bộ ba hợp lệ. Để tối thiểu hóa khoảng cách, ta có thể chọn ba chỉ số liên tiếp $i$, $j$ và $k$ trong danh sách chỉ số của số đó, với $i < j < k$. Khoảng cách của bộ ba này là $j - i + k - j + k - i = 2 \times (k - i)$. Ta duyệt qua tất cả các nhóm gồm ba chỉ số liên tiếp trong danh sách, tính khoảng cách rồi cập nhật đáp án.

Cuối cùng, nếu đáp án vẫn là giá trị ban đầu $\infty$, nghĩa là không tồn tại bộ ba hợp lệ nào, nên ta trả về $-1$; ngược lại, ta trả về khoảng cách nhỏ nhất đã tính được.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDistance(self, nums: List[int]) -> int:
        g = defaultdict(list)
        for i, x in enumerate(nums):
            g[x].append(i)
        ans = inf
        for ls in g.values():
            for h in range(len(ls) - 2):
                i, k = ls[h], ls[h + 2]
                ans = min(ans, (k - i) * 2)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minimumDistance(int[] nums) {
        int n = nums.length;
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            g.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        }
        final int inf = 1 << 30;
        int ans = inf;
        for (var ls : g.values()) {
            int m = ls.size();
            for (int h = 0; h < m - 2; ++h) {
                int i = ls.get(h);
                int k = ls.get(h + 2);
                int t = (k - i) * 2;
                ans = Math.min(ans, t);
            }
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDistance(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < n; ++i) {
            g[nums[i]].push_back(i);
        }
        const int inf = 1 << 30;
        int ans = inf;
        for (auto& [_, ls] : g) {
            int m = ls.size();
            for (int h = 0; h < m - 2; ++h) {
                int i = ls[h];
                int k = ls[h + 2];
                int t = (k - i) * 2;
                ans = min(ans, t);
            }
        }
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func minimumDistance(nums []int) int {
	g := make(map[int][]int)
	for i, x := range nums {
		g[x] = append(g[x], i)
	}

	inf := 1 << 30
	ans := inf

	for _, ls := range g {
		m := len(ls)
		for h := 0; h < m-2; h++ {
			i := ls[h]
			k := ls[h+2]
			t := (k - i) * 2
			ans = min(ans, t)
		}
	}

	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDistance(nums: number[]): number {
    const n = nums.length;
    const g = new Map<number, number[]>();

    for (let i = 0; i < n; i++) {
        if (!g.has(nums[i])) {
            g.set(nums[i], []);
        }
        g.get(nums[i])!.push(i);
    }

    const inf = 1 << 30;
    let ans = inf;

    for (const ls of g.values()) {
        const m = ls.length;
        for (let h = 0; h < m - 2; h++) {
            const i = ls[h];
            const k = ls[h + 2];
            const t = (k - i) * 2;
            ans = Math.min(ans, t);
        }
    }

    return ans === inf ? -1 : ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn minimum_distance(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut g: HashMap<i32, Vec<usize>> = HashMap::new();
        for (i, &num) in nums.iter().enumerate() {
            g.entry(num).or_insert_with(Vec::new).push(i);
        }

        let inf = 1 << 30;
        let mut ans = inf;
        for ls in g.values() {
            let m = ls.len();
            if m < 3 {
                continue;
            }
            for h in 0..m - 2 {
                let i = ls[h];
                let k = ls[h + 2];
                let t = (k - i) as i32 * 2;
                ans = ans.min(t);
            }
        }

        if ans == inf { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
