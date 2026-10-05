---
comments: true
difficulty: Easy
rating: 1190
source: Weekly Contest 487 Q1
tags:
    - Bit Manipulation
    - Enumeration
---

<!-- problem:start -->

# [3827. Count Monobit Integers](https://leetcode.com/problems/count-monobit-integers)

[中文文档](/solution/3800-3899/3827.Count%20Monobit%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Một số nguyên được gọi là <strong>Monobit</strong> nếu tất cả các bit trong biểu diễn nhị phân của nó đều giống nhau.</p>

<p>Hãy trả về số lượng số nguyên <strong>Monobit</strong> trong đoạn <code>[0, n]</code> (bao gồm cả hai đầu mút).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên trong đoạn <code>[0, 1]</code> có biểu diễn nhị phân là <code>&quot;0&quot;</code> và <code>&quot;1&quot;</code>.</li>
	<li>Mỗi biểu diễn đều chỉ gồm các bit giống nhau. Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên trong đoạn <code>[0, 4]</code> có các biểu diễn nhị phân <code>&quot;0&quot;</code>, <code>&quot;1&quot;</code>, <code>&quot;10&quot;</code>, <code>&quot;11&quot;</code> và <code>&quot;100&quot;</code>.</li>
	<li>Chỉ 0, 1 và 3 thỏa mãn điều kiện Monobit. Vì vậy, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên monobit có tất cả các bit giống nhau: $0$ hoặc một số có dạng $2^{t}-1$. Vì $n \le 1000$, ta có thể duyệt đoạn $[0,n]$, nhưng chỉ cần sinh các giá trị này.
>
> Bắt đầu từ $1$ và liên tục cộng thêm bit $1$ ở vị trí cao hơn tiếp theo, ta thu được $1,3,7,\ldots$ cho đến khi giá trị vượt quá $n$.
>
> Tính cả $0$ sẽ cho kích thước của đoạn cần tìm.
>
> Vòng lặp chạy $O(\log n)$ lần.

<!-- thinking:end -->

Theo mô tả đề bài, một số nguyên Monobit hoặc là $0$, hoặc có biểu diễn nhị phân chỉ gồm các bit $1$.

Do đó, trước tiên ta đưa $0$ vào đáp án, sau đó bắt đầu từ $1$ và lần lượt sinh các số nguyên có biểu diễn nhị phân chỉ gồm các bit $1$ cho đến khi số nguyên đó vượt quá $n$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMonobit(self, n: int) -> int:
        ans = x = 1
        i = 1
        while x <= n:
            ans += 1
            x += 1 << i
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int countMonobit(int n) {
        int ans = 1;
        for (int i = 1, x = 1; x <= n; ++i) {
            ++ans;
            x += (1 << i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countMonobit(int n) {
        int ans = 1;
        for (int i = 1, x = 1; x <= n; ++i) {
            ++ans;
            x += (1 << i);
        }
        return ans;
    }
};
```

#### Go

```go
func countMonobit(n int) (ans int) {
	ans = 1
	for i, x := 1, 1; x <= n; i++ {
		ans++
		x += (1 << i)
	}
	return
}
```

#### TypeScript

```ts
function countMonobit(n: number): number {
    let ans = 1;
    for (let i = 1, x = 1; x <= n; ++i) {
        ++ans;
        x += 1 << i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
