---
comments: true
difficulty: Easy
rating: 1201
source: Weekly Contest 504 Q1
tags:
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3945. Digit Frequency Score](https://leetcode.com/problems/digit-frequency-score)

[中文文档](/solution/3900-3999/3945.Digit%20Frequency%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p><strong>Điểm số</strong> của <code>n</code> được định nghĩa là <strong>tổng</strong> của <code>d * freq(d)</code> trên tất cả các chữ số <strong>phân biệt</strong> <code>d</code>, trong đó <code>freq(d)</code> là số lần chữ số <code>d</code> xuất hiện trong <code>n</code>.</p>

<p>Trả về một số nguyên biểu thị điểm số của <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 122</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chữ số 1 xuất hiện 1 lần, đóng góp <code>1 * 1 = 1</code>.</li>
	<li>Chữ số 2 xuất hiện 2 lần, đóng góp <code>2 * 2 = 4</code>.</li>
	<li>Vì vậy, điểm số của <code>n</code> là <code>1 + 4 = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 101</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chữ số 0 xuất hiện 1 lần, đóng góp <code>0 * 1 = 0</code>.</li>
	<li>Chữ số 1 xuất hiện 2 lần, đóng góp <code>1 * 2 = 2</code>.</li>
	<li>Vì vậy, điểm số của <code>n</code> là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là tổng các chữ số thập phân. Ta liên tục lấy $n\bmod 10$ rồi chia cho $10$ cho đến khi $n$ trở thành $0$.
>
> Độ phức tạp là $O(\log n)$ và không cần chuyển đổi sang chuỗi.

<!-- thinking:end -->

Bài toán tương đương với việc tìm tổng các chữ số của một số. Ta có thể lấy từng chữ số bằng cách liên tục lấy phần dư và chia cho 10, rồi cộng dồn kết quả.

Độ phức tạp thời gian là $O(\log n)$, trong đó $\log n$ là số chữ số của $n$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def digitFrequencyScore(self, n: int) -> int:
        ans = 0
        while n:
            n, x = divmod(n, 10)
            ans += x
        return ans
```

#### Java

```java
class Solution {
    public int digitFrequencyScore(int n) {
        int ans = 0;
        for (; n > 0; n /= 10) {
            ans += n % 10;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int digitFrequencyScore(int n) {
        int ans = 0;
        for (; n > 0; n /= 10) {
            ans += n % 10;
        }
        return ans;
    }
};
```

#### Go

```go
func digitFrequencyScore(n int) (ans int) {
	for ; n > 0; n /= 10 {
		ans += n % 10
	}
	return
}
```

#### TypeScript

```ts
function digitFrequencyScore(n: number): number {
    let ans = 0;
    for (; n; n = Math.floor(n / 10)) {
        ans += n % 10;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
