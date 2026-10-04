---
comments: true
difficulty: Medium
rating: 1873
source: Weekly Contest 377 Q2
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [2975. Maximum Square Area by Removing Fences From a Field](https://leetcode.com/problems/maximum-square-area-by-removing-fences-from-a-field)

[中文文档](/solution/2900-2999/2975.Maximum%20Square%20Area%20by%20Removing%20Fences%20From%20a%20Field/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cánh đồng hình chữ nhật lớn kích thước <code>(m - 1) x (n - 1)</code>, với các góc tại <code>(1, 1)</code> và <code>(m, n)</code>, chứa một số hàng rào ngang và dọc được cho trong hai mảng <code>hFences</code> và <code>vFences</code> tương ứng.</p>

<p>Các hàng rào ngang kéo dài từ tọa độ <code>(hFences[i], 1)</code> đến <code>(hFences[i], n)</code>, còn các hàng rào dọc kéo dài từ tọa độ <code>(1, vFences[i])</code> đến <code>(m, vFences[i])</code>.</p>

<p>Trả về <em>diện tích <strong>lớn nhất</strong> của một cánh đồng hình <strong>vuông</strong> có thể tạo thành bằng cách <strong>tháo bỏ</strong> một số hàng rào (<strong>có thể không tháo</strong> hàng rào nào) hoặc </em><code>-1</code> <em>nếu không thể tạo ra cánh đồng hình vuông</em>.</p>

<p>Vì đáp án có thể lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9 </sup>+ 7</code>.</p>

<p><strong>Lưu ý: </strong>Cánh đồng được bao quanh bởi hai hàng rào ngang từ tọa độ <code>(1, 1)</code> đến <code>(1, n)</code> và từ <code>(m, 1)</code> đến <code>(m, n)</code>, cùng hai hàng rào dọc từ tọa độ <code>(1, 1)</code> đến <code>(m, 1)</code> và từ <code>(1, n)</code> đến <code>(m, n)</code>. <strong>Không thể tháo bỏ</strong> các hàng rào này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2975.Maximum%20Square%20Area%20by%20Removing%20Fences%20From%20a%20Field/images/screenshot-from-2023-11-05-22-40-25.png" /></p>

<pre>
<strong>Đầu vào:</strong> m = 4, n = 3, hFences = [2,3], vFences = [2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tháo bỏ hàng rào ngang tại 2 và hàng rào dọc tại 2 sẽ tạo ra một cánh đồng hình vuông có diện tích 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2975.Maximum%20Square%20Area%20by%20Removing%20Fences%20From%20a%20Field/images/maxsquareareaexample1.png" style="width: 285px; height: 242px;" /></p>

<pre>
<strong>Đầu vào:</strong> m = 6, n = 7, hFences = [2], vFences = [4]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không có cách nào tạo ra một cánh đồng hình vuông bằng cách tháo bỏ các hàng rào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= m, n &lt;= 10<sup>9</sup></code></li>
	<li><code><font face="monospace">1 &lt;= hF</font>ences<font face="monospace">.length, vFences.length &lt;= 600</font></code></li>
	<li><code><font face="monospace">1 &lt; hFences[i] &lt; m</font></code></li>
	<li><code><font face="monospace">1 &lt; vFences[i] &lt; n</font></code></li>
	<li><code><font face="monospace">hFences</font></code><font face="monospace"> và </font><code><font face="monospace">vFences</font></code><font face="monospace"> không trùng nhau.</font></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một hình vuông cần hai hàng rào ngang (bao gồm cả biên $1$ và $m$) có khoảng cách bằng với khoảng cách giữa hai hàng rào dọc (bao gồm cả biên $1$ và $n$). Có nhiều nhất $600$ hàng rào, nên ta có thể xét mọi cặp hàng rào theo từng hướng. Đưa mọi khoảng cách ngang và dọc vào một set, rồi lấy giá trị lớn nhất trong phần giao của hai set.
>
> $m$ và $n$ có thể đạt $10^9$, nên ta không cần dựng cả cánh đồng. Nếu không có khoảng cách chung, trả về $-1$; ngược lại, tính bình phương cạnh theo modulo số nguyên tố.

<!-- thinking:end -->

Ta có thể liệt kê mọi cặp hàng rào ngang $a$ và $b$ trong $\textit{hFences}$, tính khoảng cách $d$ giữa $a$ và $b$, rồi lưu khoảng cách đó vào hash table $hs$. Sau đó, ta liệt kê mọi cặp hàng rào dọc $c$ và $d$ trong $\textit{vFences}$, tính khoảng cách $d$ giữa $c$ và $d$, rồi lưu khoảng cách đó vào hash table $vs$. Cuối cùng, ta duyệt hash table $hs$. Nếu một khoảng cách $d$ trong $hs$ cũng tồn tại trong hash table $vs$, điều đó cho biết có một cánh đồng hình vuông với độ dài cạnh $d$, và diện tích là $d^2$. Ta chỉ cần lấy $d$ lớn nhất rồi tính $d^2 \bmod 10^9 + 7$.

Độ phức tạp thời gian là $O(h^2 + v^2)$, còn độ phức tạp không gian là $O(h^2 + v^2)$. Ở đây, $h$ và $v$ lần lượt là độ dài của $\textit{hFences}$ và $\textit{vFences}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeSquareArea(
        self, m: int, n: int, hFences: List[int], vFences: List[int]
    ) -> int:
        def f(nums: List[int], k: int) -> Set[int]:
            nums.extend([1, k])
            nums.sort()
            return {b - a for a, b in combinations(nums, 2)}

        mod = 10**9 + 7
        hs = f(hFences, m)
        vs = f(vFences, n)
        ans = max(hs & vs, default=0)
        return ans**2 % mod if ans else -1
```

#### Java

```java
class Solution {
    public int maximizeSquareArea(int m, int n, int[] hFences, int[] vFences) {
        Set<Integer> hs = f(hFences, m);
        Set<Integer> vs = f(vFences, n);
        hs.retainAll(vs);
        int ans = -1;
        final int mod = (int) 1e9 + 7;
        for (int x : hs) {
            ans = Math.max(ans, x);
        }
        return ans > 0 ? (int) (1L * ans * ans % mod) : -1;
    }

    private Set<Integer> f(int[] nums, int k) {
        int n = nums.length;
        nums = Arrays.copyOf(nums, n + 2);
        nums[n] = 1;
        nums[n + 1] = k;
        Arrays.sort(nums);
        Set<Integer> s = new HashSet<>();
        for (int i = 0; i < nums.length; ++i) {
            for (int j = 0; j < i; ++j) {
                s.add(nums[i] - nums[j]);
            }
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeSquareArea(int m, int n, vector<int>& hFences, vector<int>& vFences) {
        auto f = [](vector<int>& nums, int k) {
            nums.push_back(k);
            nums.push_back(1);
            sort(nums.begin(), nums.end());
            unordered_set<int> s;
            for (int i = 0; i < nums.size(); ++i) {
                for (int j = 0; j < i; ++j) {
                    s.insert(nums[i] - nums[j]);
                }
            }
            return s;
        };
        auto hs = f(hFences, m);
        auto vs = f(vFences, n);
        int ans = 0;
        for (int h : hs) {
            if (vs.count(h)) {
                ans = max(ans, h);
            }
        }
        const int mod = 1e9 + 7;
        return ans > 0 ? 1LL * ans * ans % mod : -1;
    }
};
```

#### Go

```go
func maximizeSquareArea(m int, n int, hFences []int, vFences []int) int {
	f := func(nums []int, k int) map[int]bool {
		nums = append(nums, 1, k)
		sort.Ints(nums)
		s := map[int]bool{}
		for i := 0; i < len(nums); i++ {
			for j := 0; j < i; j++ {
				s[nums[i]-nums[j]] = true
			}
		}
		return s
	}
	hs := f(hFences, m)
	vs := f(vFences, n)
	ans := 0
	for h := range hs {
		if vs[h] {
			ans = max(ans, h)
		}
	}
	if ans > 0 {
		return ans * ans % (1e9 + 7)
	}
	return -1
}
```

#### TypeScript

```ts
function maximizeSquareArea(m: number, n: number, hFences: number[], vFences: number[]): number {
    const f = (nums: number[], k: number): Set<number> => {
        nums.push(1, k);
        nums.sort((a, b) => a - b);
        const s: Set<number> = new Set();
        for (let i = 0; i < nums.length; ++i) {
            for (let j = 0; j < i; ++j) {
                s.add(nums[i] - nums[j]);
            }
        }
        return s;
    };
    const hs = f(hFences, m);
    const vs = f(vFences, n);
    let ans = 0;
    for (const h of hs) {
        if (vs.has(h)) {
            ans = Math.max(ans, h);
        }
    }
    return ans ? Number(BigInt(ans) ** 2n % 1000000007n) : -1;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn maximize_square_area(m: i32, n: i32, h_fences: Vec<i32>, v_fences: Vec<i32>) -> i32 {
        fn calc(mut nums: Vec<i32>, k: i32) -> HashSet<i32> {
            nums.push(k);
            nums.push(1);
            nums.sort_unstable();
            let mut s = HashSet::new();
            let len = nums.len();
            for i in 0..len {
                for j in 0..i {
                    s.insert(nums[i] - nums[j]);
                }
            }
            s
        }

        let hs = calc(h_fences, m);
        let vs = calc(v_fences, n);

        let mut ans = 0i64;
        for &h in hs.iter() {
            if vs.contains(&h) {
                ans = ans.max(h as i64);
            }
        }

        if ans > 0 {
            let modv = 1_000_000_007i64;
            ((ans * ans) % modv) as i32
        } else {
            -1
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
