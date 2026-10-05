---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 512 Q1
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [4000. Largest Integer With Given Digit Sum](https://leetcode.com/problems/largest-integer-with-given-digit-sum)

[中文文档](/solution/4000-4099/4000.Largest%20Integer%20With%20Given%20Digit%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên không âm <code>n</code> và <code>s</code>.</p>

<p>Hãy trả về số nguyên <strong>lớn nhất</strong> có <strong>nhiều nhất</strong> <code>n</code> chữ số và có tổng các chữ số bằng <code>s</code>. Nếu không tồn tại số như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, s = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">90</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên lớn nhất có nhiều nhất 2 chữ số và tổng các chữ số bằng 9 là 90.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, s = 19</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên nào có nhiều nhất 2 chữ số và tổng các chữ số bằng 19, nên đáp án là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, s = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên không âm duy nhất có tổng các chữ số bằng 0 là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5</code></li>
	<li><code>0 &lt;= s &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tạo số nguyên gồm $n$ chữ số có tổng chữ số bằng $s$ bằng cách liệt kê mọi số khả thi khi $n\le 5$, nhưng cách đó không cần thiết.
>
> Khi tổng chữ số cố định, độ lớn được quyết định bởi các vị trí cao hơn: tăng một chữ số ở vị trí cao sẽ có tác động lớn hơn mọi cách sắp xếp lại các chữ số thấp hơn. Vì vậy, ta điền từ cao xuống thấp bằng $\min(s,9)$ và để phần còn lại cho các chữ số sau.
>
> Nếu $n\times 9<s$, ngay cả chuỗi toàn chữ số 9 cũng không đạt được tổng cần có, nên đáp án là $-1$.

<!-- thinking:end -->

Nếu $n \times 9 < s$, ngay cả khi điền $9$ vào mọi chữ số cũng không đạt được tổng chữ số $s$, nên trả về $-1$.

Ngược lại, để số nguyên lớn nhất, ta gán chữ số lớn nhất có thể cho các vị trí cao hơn. Xây dựng $n$ chữ số từ cao xuống thấp: mỗi chữ số nhận $\min(s, 9)$, sau đó trừ giá trị đó khỏi $s$. Số nguyên thu được là đáp án (nếu $s = 0$ thì kết quả là $0$).

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestInteger(self, n: int, s: int) -> int:
        if n * 9 < s:
            return -1
        ans = 0
        for _ in range(n):
            x = min(s, 9)
            ans = ans * 10 + x
            s -= x
        return ans
```

#### Java

```java
class Solution {
    public int largestInteger(int n, int s) {
        if (n * 9 < s) {
            return -1;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int x = Math.min(s, 9);
            ans = ans * 10 + x;
            s -= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestInteger(int n, int s) {
        if (n * 9 < s) {
            return -1;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int x = min(s, 9);
            ans = ans * 10 + x;
            s -= x;
        }
        return ans;
    }
};
```

#### Go

```go
func largestInteger(n int, s int) (ans int) {
	if n*9 < s {
		return -1
	}
	for i := 0; i < n; i++ {
		x := min(s, 9)
		ans = ans*10 + x
		s -= x
	}
	return
}
```

#### TypeScript

```ts
function largestInteger(n: number, s: number): number {
    if (n * 9 < s) {
        return -1;
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const x = Math.min(s, 9);
        ans = ans * 10 + x;
        s -= x;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
