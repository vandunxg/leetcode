---
comments: true
difficulty: Medium
rating: 1734
source: Weekly Contest 264 Q2
tags:
    - Hash Table
    - Math
    - Backtracking
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [2048. Next Greater Numerically Balanced Number](https://leetcode.com/problems/next-greater-numerically-balanced-number)

[中文文档](/solution/2000-2099/2048.Next%20Greater%20Numerically%20Balanced%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên <code>x</code> được gọi là <strong>cân bằng theo chữ số</strong> nếu với mỗi chữ số <code>d</code> xuất hiện trong số <code>x</code>, chữ số đó xuất hiện <strong>đúng</strong> <code>d</code> lần trong <code>x</code>.</p>

<p>Cho một số nguyên <code>n</code>, hãy trả về <em>số <strong>cân bằng theo chữ số nhỏ nhất</strong> <strong>lớn hơn nghiêm ngặt</strong> </em><code>n</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong>
22 cân bằng theo chữ số vì:
- Chữ số 2 xuất hiện 2 lần.
Đây cũng là số cân bằng theo chữ số nhỏ nhất lớn hơn nghiêm ngặt 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1000
<strong>Đầu ra:</strong> 1333
<strong>Giải thích:</strong>
1333 cân bằng theo chữ số vì:
- Chữ số 1 xuất hiện 1 lần.
- Chữ số 3 xuất hiện 3 lần.
Đây cũng là số cân bằng theo chữ số nhỏ nhất lớn hơn nghiêm ngặt 1000.
Lưu ý rằng 1022 không thể là đáp án vì chữ số 0 xuất hiện nhiều hơn 0 lần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3000
<strong>Đầu ra:</strong> 3133
<strong>Giải thích:</strong>
3133 cân bằng theo chữ số vì:
- Chữ số 1 xuất hiện 1 lần.
- Chữ số 3 xuất hiện 3 lần.
Đây cũng là số cân bằng theo chữ số nhỏ nhất lớn hơn nghiêm ngặt 3000.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 10^6$ và số cân bằng tiếp theo không vượt quá $1224444$, ta có thể kiểm tra lần lượt $x=n+1,n+2,\ldots$. Nếu xuất hiện, chữ số $d$ phải xuất hiện đúng $d$ lần.
>
> Đếm các chữ số thập phân của $x$ và chấp nhận khi mọi tần suất khác 0 đều bằng chữ số tương ứng. `count` tăng dần cho đến khi tìm thấy một số phù hợp.

<!-- thinking:end -->

Ta nhận thấy miền giá trị của $n$ trong bài toán là $[0, 10^6]$, và một trong các số cân bằng lớn hơn $10^6$ là $1224444$. Do đó, ta duyệt trực tiếp $x \in [n + 1, ..]$ rồi kiểm tra xem $x$ có phải là số cân bằng hay không. Giá trị $x$ được duyệt chắc chắn không vượt quá $1224444$.

Độ phức tạp thời gian là $O(M - n)$, trong đó $M = 1224444$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextBeautifulNumber(self, n: int) -> int:
        for x in count(n + 1):
            y = x
            cnt = [0] * 10
            while y:
                y, v = divmod(y, 10)
                cnt[v] += 1
            if all(v == 0 or i == v for i, v in enumerate(cnt)):
                return x
```

#### Java

```java
class Solution {
    public int nextBeautifulNumber(int n) {
        for (int x = n + 1;; ++x) {
            int[] cnt = new int[10];
            for (int y = x; y > 0; y /= 10) {
                ++cnt[y % 10];
            }
            boolean ok = true;
            for (int y = x; y > 0; y /= 10) {
                if (y % 10 != cnt[y % 10]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                return x;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int nextBeautifulNumber(int n) {
        for (int x = n + 1;; ++x) {
            int cnt[10]{};
            for (int y = x; y > 0; y /= 10) {
                ++cnt[y % 10];
            }
            bool ok = true;
            for (int y = x; y > 0; y /= 10) {
                if (y % 10 != cnt[y % 10]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                return x;
            }
        }
    }
};
```

#### Go

```go
func nextBeautifulNumber(n int) int {
	for x := n + 1; ; x++ {
		cnt := [10]int{}
		for y := x; y > 0; y /= 10 {
			cnt[y%10]++
		}
		ok := true
		for y := x; y > 0; y /= 10 {
			if y%10 != cnt[y%10] {
				ok = false
				break
			}
		}
		if ok {
			return x
		}
	}
}
```

#### TypeScript

```ts
function nextBeautifulNumber(n: number): number {
    for (let x = n + 1; ; ++x) {
        const cnt: number[] = Array(10).fill(0);
        for (let y = x; y > 0; y = (y / 10) | 0) {
            cnt[y % 10]++;
        }
        let ok = true;
        for (let i = 0; i < 10; ++i) {
            if (cnt[i] && cnt[i] !== i) {
                ok = false;
                break;
            }
        }
        if (ok) {
            return x;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn next_beautiful_number(n: i32) -> i32 {
        let mut x = n + 1;
        loop {
            let mut cnt = [0; 10];
            let mut y = x;
            while y > 0 {
                cnt[(y % 10) as usize] += 1;
                y /= 10;
            }
            let mut ok = true;
            let mut y2 = x;
            while y2 > 0 {
                let d = (y2 % 10) as usize;
                if d != cnt[d] {
                    ok = false;
                    break;
                }
                y2 /= 10;
            }
            if ok {
                return x;
            }
            x += 1;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
