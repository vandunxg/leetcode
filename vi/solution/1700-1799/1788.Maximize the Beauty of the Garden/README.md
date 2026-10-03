---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [1788. Maximize the Beauty of the Garden 🔒](https://leetcode.com/problems/maximize-the-beauty-of-the-garden)

[中文文档](/solution/1700-1799/1788.Maximize%20the%20Beauty%20of%20the%20Garden/README.md)

## Mô tả

<!-- description:start -->

<p>Có một khu vườn gồm <code>n</code> bông hoa, mỗi bông có một giá trị vẻ đẹp nguyên. Các bông hoa được xếp thành một hàng. Cho mảng số nguyên <code>flowers</code> độ dài <code>n</code>, trong đó <code>flowers[i]</code> là vẻ đẹp của bông hoa thứ <code>i<sup>th</sup></code>.</p>

<p>Khu vườn <strong>hợp lệ</strong> nếu thỏa mãn các điều kiện:</p>

<ul>
	<li>Khu vườn có ít nhất hai bông hoa.</li>
	<li>Bông hoa đầu tiên và cuối cùng có cùng giá trị vẻ đẹp.</li>
</ul>

<p>Là người làm vườn được chỉ định, bạn có thể <strong>loại bỏ</strong> bất kỳ số lượng bông hoa nào, kể cả không loại bỏ bông nào. Bạn muốn loại bỏ hoa sao cho khu vườn còn lại <strong>hợp lệ</strong>. Vẻ đẹp của khu vườn là tổng vẻ đẹp của tất cả bông hoa còn lại.</p>

<p>Trả về vẻ đẹp lớn nhất có thể của một khu vườn <strong>hợp lệ</strong> sau khi loại bỏ hoa.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> flowers = [1,2,3,1,2]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Có thể tạo khu vườn hợp lệ [2,3,1,2] với tổng vẻ đẹp 2 + 3 + 1 + 2 = 8.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> flowers = [100,1,1,-3,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể tạo khu vườn hợp lệ [1,1,1] với tổng vẻ đẹp 1 + 1 + 1 = 3.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> flowers = [-1,-2,0,-1]
<strong>Đầu ra:</strong> -2
<strong>Giải thích:</strong> Có thể tạo khu vườn hợp lệ [-1,-1] với tổng vẻ đẹp -1 + -1 = -2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= flowers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= flowers[i] &lt;= 10<sup>4</sup></code></li>
	<li>Có thể tạo một khu vườn hợp lệ bằng cách loại bỏ một số bông hoa, có thể không loại bỏ bông nào.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Khu vườn hợp lệ có hai đầu bằng nhau; các bông hoa âm ở giữa có thể bỏ đi. Với mỗi cặp đầu bằng nhau, giữ lại mọi bông hoa dương ở giữa.
>
> Một map lưu chỉ số đầu tiên của mỗi giá trị vẻ đẹp; tổng tiền tố chỉ cộng các giá trị dương. Khi gặp lại $v$, điểm số là hai lần $v$ cộng với tổng dương ở giữa.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu lần xuất hiện đầu tiên của mỗi giá trị vẻ đẹp, và mảng tổng tiền tố $s$ để lưu tổng các giá trị vẻ đẹp trước vị trí hiện tại. Nếu giá trị vẻ đẹp $v$ xuất hiện ở vị trí $i$ và $j$ (với $i \lt j$), ta có khu vườn hợp lệ $[i+1,j]$, có vẻ đẹp là $s[i] - s[j + 1] + v \times 2$. Dùng giá trị này để cập nhật đáp án. Nếu chưa từng xuất hiện, ta lưu vị trí hiện tại $i$ của giá trị vào hash table $d$. Sau đó cập nhật tổng tiền tố. Nếu giá trị vẻ đẹp $v$ âm, ta xem nó là $0$.

Sau khi duyệt qua tất cả giá trị vẻ đẹp, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số bông hoa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBeauty(self, flowers: List[int]) -> int:
        s = [0] * (len(flowers) + 1)
        d = {}
        ans = -inf
        for i, v in enumerate(flowers):
            if v in d:
                ans = max(ans, s[i] - s[d[v] + 1] + v * 2)
            else:
                d[v] = i
            s[i + 1] = s[i] + max(v, 0)
        return ans
```

#### Java

```java
class Solution {
    public int maximumBeauty(int[] flowers) {
        int n = flowers.length;
        int[] s = new int[n + 1];
        Map<Integer, Integer> d = new HashMap<>();
        int ans = Integer.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            int v = flowers[i];
            if (d.containsKey(v)) {
                ans = Math.max(ans, s[i] - s[d.get(v) + 1] + v * 2);
            } else {
                d.put(v, i);
            }
            s[i + 1] = s[i] + Math.max(v, 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumBeauty(vector<int>& flowers) {
        int n = flowers.size();
        vector<int> s(n + 1);
        unordered_map<int, int> d;
        int ans = INT_MIN;
        for (int i = 0; i < n; ++i) {
            int v = flowers[i];
            if (d.count(v)) {
                ans = max(ans, s[i] - s[d[v] + 1] + v * 2);
            } else {
                d[v] = i;
            }
            s[i + 1] = s[i] + max(v, 0);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBeauty(flowers []int) int {
	n := len(flowers)
	s := make([]int, n+1)
	d := map[int]int{}
	ans := math.MinInt32
	for i, v := range flowers {
		if j, ok := d[v]; ok {
			ans = max(ans, s[i]-s[j+1]+v*2)
		} else {
			d[v] = i
		}
		s[i+1] = s[i] + max(v, 0)
	}
	return ans
}
```

#### TypeScript

```ts
function maximumBeauty(flowers: number[]): number {
    const n = flowers.length;
    const s: number[] = Array(n + 1).fill(0);
    const d: Map<number, number> = new Map();
    let ans = -Infinity;
    for (let i = 0; i < n; ++i) {
        const v = flowers[i];
        if (d.has(v)) {
            ans = Math.max(ans, s[i] - s[d.get(v)! + 1] + v * 2);
        } else {
            d.set(v, i);
        }
        s[i + 1] = s[i] + Math.max(v, 0);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn maximum_beauty(flowers: Vec<i32>) -> i32 {
        let mut s = vec![0; flowers.len() + 1];
        let mut d = HashMap::new();
        let mut ans = i32::MIN;

        for (i, &v) in flowers.iter().enumerate() {
            if let Some(&j) = d.get(&v) {
                ans = ans.max(s[i] - s[j + 1] + v * 2);
            } else {
                d.insert(v, i);
            }
            s[i + 1] = s[i] + v.max(0);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
