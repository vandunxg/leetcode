---
comments: true
difficulty: Easy
rating: 1341
source: Biweekly Contest 23 Q1
tags:
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [1399. Count Largest Group](https://leetcode.com/problems/count-largest-group)

[中文文档](/solution/1300-1399/1399.Count%20Largest%20Group/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>.</p>

<p>Hãy nhóm các số từ <code>1</code> đến <code>n</code> theo tổng chữ số. Ví dụ, 14 và 5 thuộc <strong>cùng</strong> một nhóm, còn 13 và 3 thuộc các nhóm <strong>khác nhau</strong>.</p>

<p>Trả về số nhóm có kích thước lớn nhất, tức có số phần tử <strong>nhiều nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 13
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tổng cộng có 9 nhóm, được tạo bằng cách nhóm các số từ 1 đến 13 theo tổng chữ số:
[1,10], [2,11], [3,12], [4,13], [5], [6], [7], [8], [9].
Có 4 nhóm đạt kích thước lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 nhóm [1], [2], mỗi nhóm có kích thước 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Nhóm các số từ $1$ đến $n$ theo tổng chữ số và đếm số nhóm cùng đạt kích thước lớn nhất. Vì $n \le 10^4$, tổng chữ số tối đa là $36$. Tính tổng chữ số của từng số nguyên, đếm kích thước các nhóm, đồng thời theo dõi kích thước lớn nhất hiện tại và số nhóm đạt kích thước đó.

<!-- thinking:end -->

Do các số không vượt quá $10^4$, tổng chữ số cũng không vượt quá $9 \times 4 = 36$. Vì vậy, ta có thể dùng hash table hoặc mảng độ dài $40$, ký hiệu là $cnt$, để đếm số lượng số ứng với từng tổng chữ số, và dùng biến $mx$ biểu diễn số lượng lớn nhất hiện tại.

Ta duyệt từng số trong $[1,..n]$, tính tổng chữ số $s$, rồi tăng $cnt[s]$ thêm $1$. Nếu $mx < cnt[s]$, ta cập nhật $mx = cnt[s]$ và đặt $ans$ bằng $1$. Nếu $mx = cnt[s]$, ta tăng $ans$ thêm $1$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countLargestGroup(self, n: int) -> int:
        cnt = Counter()
        ans = mx = 0
        for i in range(1, n + 1):
            s = 0
            while i:
                s += i % 10
                i //= 10
            cnt[s] += 1
            if mx < cnt[s]:
                mx = cnt[s]
                ans = 1
            elif mx == cnt[s]:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countLargestGroup(int n) {
        int[] cnt = new int[40];
        int ans = 0, mx = 0;
        for (int i = 1; i <= n; ++i) {
            int s = 0;
            for (int x = i; x > 0; x /= 10) {
                s += x % 10;
            }
            ++cnt[s];
            if (mx < cnt[s]) {
                mx = cnt[s];
                ans = 1;
            } else if (mx == cnt[s]) {
                ++ans;
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
    int countLargestGroup(int n) {
        int cnt[40]{};
        int ans = 0, mx = 0;
        for (int i = 1; i <= n; ++i) {
            int s = 0;
            for (int x = i; x; x /= 10) {
                s += x % 10;
            }
            ++cnt[s];
            if (mx < cnt[s]) {
                mx = cnt[s];
                ans = 1;
            } else if (mx == cnt[s]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countLargestGroup(n int) (ans int) {
	cnt := [40]int{}
	mx := 0
	for i := 1; i <= n; i++ {
		s := 0
		for x := i; x > 0; x /= 10 {
			s += x % 10
		}
		cnt[s]++
		if mx < cnt[s] {
			mx = cnt[s]
			ans = 1
		} else if mx == cnt[s] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countLargestGroup(n: number): number {
    const cnt: number[] = Array(40).fill(0);
    let mx = 0;
    let ans = 0;
    for (let i = 1; i <= n; ++i) {
        let s = 0;
        for (let x = i; x; x = Math.floor(x / 10)) {
            s += x % 10;
        }
        ++cnt[s];
        if (mx < cnt[s]) {
            mx = cnt[s];
            ans = 1;
        } else if (mx === cnt[s]) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_largest_group(n: i32) -> i32 {
        let mut cnt = vec![0; 40];
        let mut ans = 0;
        let mut mx = 0;

        for i in 1..=n {
            let mut s = 0;
            let mut x = i;
            while x > 0 {
                s += x % 10;
                x /= 10;
            }
            cnt[s as usize] += 1;
            if mx < cnt[s as usize] {
                mx = cnt[s as usize];
                ans = 1;
            } else if mx == cnt[s as usize] {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
