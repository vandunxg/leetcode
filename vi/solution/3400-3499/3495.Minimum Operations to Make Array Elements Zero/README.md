---
comments: true
difficulty: Hard
rating: 2205
source: Weekly Contest 442 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [3495. Minimum Operations to Make Array Elements Zero](https://leetcode.com/problems/minimum-operations-to-make-array-elements-zero)

[中文文档](/solution/3400-3499/3495.Minimum%20Operations%20to%20Make%20Array%20Elements%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2 chiều <code>queries</code>, trong đó <code>queries[i]</code> có dạng <code>[l, r]</code>. Mỗi <code>queries[i]</code> xác định một mảng số nguyên <code>nums</code> gồm các phần tử từ <code>l</code> đến <code>r</code>, <strong>bao gồm cả hai đầu mút</strong>.</p>

<p>Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn hai số nguyên <code>a</code> và <code>b</code> trong mảng.</li>
	<li>Thay chúng bằng <code>floor(a / 4)</code> và <code>floor(b / 4)</code>.</li>
</ul>

<p>Nhiệm vụ của bạn là xác định <strong>số thao tác ít nhất</strong> cần thiết để giảm tất cả phần tử của mảng về 0 cho mỗi truy vấn. Trả về tổng các kết quả của tất cả truy vấn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[1,2],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với <code>queries[0]</code>:</p>

<ul>
	<li>Mảng ban đầu là <code>nums = [1, 2]</code>.</li>
	<li>Trong thao tác đầu tiên, chọn <code>nums[0]</code> và <code>nums[1]</code>. Mảng trở thành <code>[0, 0]</code>.</li>
	<li>Số thao tác ít nhất cần thiết là 1.</li>
</ul>

<p>Với <code>queries[1]</code>:</p>

<ul>
	<li>Mảng ban đầu là <code>nums = [2, 3, 4]</code>.</li>
	<li>Trong thao tác đầu tiên, chọn <code>nums[0]</code> và <code>nums[2]</code>. Mảng trở thành <code>[0, 3, 1]</code>.</li>
	<li>Trong thao tác thứ hai, chọn <code>nums[1]</code> và <code>nums[2]</code>. Mảng trở thành <code>[0, 0, 0]</code>.</li>
	<li>Số thao tác ít nhất cần thiết là 2.</li>
</ul>

<p>Kết quả là <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">queries = [[2,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với <code>queries[0]</code>:</p>

<ul>
	<li>Mảng ban đầu là <code>nums = [2, 3, 4, 5, 6]</code>.</li>
	<li>Trong thao tác đầu tiên, chọn <code>nums[0]</code> và <code>nums[3]</code>. Mảng trở thành <code>[0, 3, 4, 1, 6]</code>.</li>
	<li>Trong thao tác thứ hai, chọn <code>nums[2]</code> và <code>nums[4]</code>. Mảng trở thành <code>[0, 3, 1, 1, 1]</code>.</li>
	<li>Trong thao tác thứ ba, chọn <code>nums[1]</code> và <code>nums[2]</code>. Mảng trở thành <code>[0, 0, 0, 1, 1]</code>.</li>
	<li>Trong thao tác thứ tư, chọn <code>nums[3]</code> và <code>nums[4]</code>. Mảng trở thành <code>[0, 0, 0, 0, 0]</code>.</li>
	<li>Số thao tác ít nhất cần thiết là 4.</li>
</ul>

<p>Kết quả là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>queries[i] == [l, r]</code></li>
	<li><code>1 &lt;= l &lt; r &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác thay thế hai số dương trong đoạn bằng $\lfloor x/4\rfloor$. Vì $l,r$ có thể lên tới $10^9$ và có $10^5$ truy vấn, ta không thể mô phỏng trực tiếp đoạn này.
>
> Một giá trị $x$ cần số $p$ nhỏ nhất sao cho $4^p>x$, và số này không đổi trên mỗi đoạn $[4^{i-1},4^i)$. Một thao tác tác động lên hai số, nên chi phí của đoạn xấp xỉ một nửa tổng các $p$, ngoại trừ trường hợp giá trị cần nhiều thao tác nhất chi phối kết quả.
>
> $f(x)$ là tổng tiền tố của các $p$ trên $[1,x]$. Với $[l,r]$, đáp án là $\max(\lceil s/2\rceil,mx)$, trong đó $s=f(r)-f(l-1)$ và $mx$ là số $p$ tương ứng với chính $r$.

<!-- thinking:end -->

Theo mô tả bài toán, giả sử số thao tác ít nhất cần thiết để biến một phần tử $x$ thành $0$ là $p$, trong đó $p$ là số nguyên nhỏ nhất sao cho $4^p > x$.

Khi đã biết số thao tác ít nhất cho mỗi phần tử, với một đoạn $[l, r]$, đặt $s$ là tổng số thao tác ít nhất cho tất cả phần tử trong $[l, r]$, và $mx$ là số thao tác lớn nhất, cũng chính là số thao tác cho phần tử $r$. Khi đó, số thao tác ít nhất để biến tất cả phần tử trong $[l, r]$ thành $0$ là $\max(\lceil s / 2 \rceil, mx)$.

Ta định nghĩa hàm $f(x)$ là tổng số thao tác ít nhất cho tất cả phần tử trong đoạn $[1, x]$. Với mỗi truy vấn $[l, r]$, ta có thể tính $s = f(r) - f(l - 1)$ và $mx = f(r) - f(r - 1)$ để thu được đáp án.

Độ phức tạp thời gian là $O(q \log M)$, trong đó $q$ là số truy vấn và $M$ là giá trị lớn nhất trong đoạn truy vấn. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, queries: List[List[int]]) -> int:
        def f(x: int) -> int:
            res = 0
            p = i = 1
            while p <= x:
                cnt = min(p * 4 - 1, x) - p + 1
                res += cnt * i
                i += 1
                p *= 4
            return res

        ans = 0
        for l, r in queries:
            s = f(r) - f(l - 1)
            mx = f(r) - f(r - 1)
            ans += max((s + 1) // 2, mx)
        return ans
```

#### Java

```java
class Solution {
    public long minOperations(int[][] queries) {
        long ans = 0;
        for (int[] q : queries) {
            int l = q[0], r = q[1];
            long s = f(r) - f(l - 1);
            long mx = f(r) - f(r - 1);
            ans += Math.max((s + 1) / 2, mx);
        }
        return ans;
    }

    private long f(long x) {
        long res = 0;
        long p = 1;
        int i = 1;
        while (p <= x) {
            long cnt = Math.min(p * 4 - 1, x) - p + 1;
            res += cnt * i;
            i++;
            p *= 4;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<vector<int>>& queries) {
        auto f = [&](long long x) {
            long long res = 0;
            long long p = 1;
            int i = 1;
            while (p <= x) {
                long long cnt = min(p * 4 - 1, x) - p + 1;
                res += cnt * i;
                i++;
                p *= 4;
            }
            return res;
        };

        long long ans = 0;
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            long long s = f(r) - f(l - 1);
            long long mx = f(r) - f(r - 1);
            ans += max((s + 1) / 2, mx);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(queries [][]int) (ans int64) {
	f := func(x int64) (res int64) {
		var p int64 = 1
		i := int64(1)
		for p <= x {
			cnt := min(p*4-1, x) - p + 1
			res += cnt * i
			i++
			p *= 4
		}
		return
	}
	for _, q := range queries {
		l, r := int64(q[0]), int64(q[1])
		s := f(r) - f(l-1)
		mx := f(r) - f(r-1)
		ans += max((s+1)/2, mx)
	}
	return
}
```

#### TypeScript

```ts
function minOperations(queries: number[][]): number {
    const f = (x: number): number => {
        let res = 0;
        let p = 1;
        let i = 1;
        while (p <= x) {
            const cnt = Math.min(p * 4 - 1, x) - p + 1;
            res += cnt * i;
            i++;
            p *= 4;
        }
        return res;
    };

    let ans = 0;
    for (const [l, r] of queries) {
        const s = f(r) - f(l - 1);
        const mx = f(r) - f(r - 1);
        ans += Math.max(Math.ceil(s / 2), mx);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(queries: Vec<Vec<i32>>) -> i64 {
        let f = |x: i64| -> i64 {
            let mut res: i64 = 0;
            let mut p: i64 = 1;
            let mut i: i64 = 1;
            while p <= x {
                let cnt = std::cmp::min(p * 4 - 1, x) - p + 1;
                res += cnt * i;
                i += 1;
                p *= 4;
            }
            res
        };

        let mut ans: i64 = 0;
        for q in queries {
            let l = q[0] as i64;
            let r = q[1] as i64;
            let s = f(r) - f(l - 1);
            let mx = f(r) - f(r - 1);
            ans += std::cmp::max((s + 1) / 2, mx);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
