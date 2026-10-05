---
comments: true
difficulty: Easy
rating: 1270
source: Biweekly Contest 184 Q1
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [3950. Exactly One Consecutive Set Bits Pair](https://leetcode.com/problems/exactly-one-consecutive-set-bits-pair)

[Tài liệu tiếng Trung](/solution/3900-3999/3950.Exactly%20One%20Consecutive%20Set%20Bits%20Pair/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>.</p>

<p>Trả về <code>true</code> nếu biểu diễn nhị phân của nó chứa <strong>chính xác một cặp liền kề</strong> gồm các <span data-keyword="set-bit">bit 1</span>, và <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Biểu diễn nhị phân của 6 là <code>110</code>.</li>
	<li>Có chính xác một cặp bit 1 liền kề (<code>&quot;11&quot;</code>). Do đó, đáp án là <code>true</code>​​​​​​​.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Biểu diễn nhị phân của 5 là <code>101</code>.</li>
	<li>Không có cặp bit 1 liền kề nào. Do đó, đáp án là <code>false</code>​​​​​​​.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần biểu diễn nhị phân chứa đúng một cặp số 1 liền kề. Tách lần lượt $\textit{cur}$ từ bit thấp và so sánh với $\textit{pre}$: nếu cả hai đều là 1 thì ghi nhận một cặp, còn gặp cặp thứ hai thì thất bại.
>
> Sau khi duyệt xong, $\textit{vis}$ là true khi và chỉ khi đã gặp đúng một cặp số 1 liên tiếp. Vì $n\le 10^5$, số lượng bit cần duyệt là nhỏ.

<!-- thinking:end -->

Ta dùng biến $\textit{pre}$ để lưu chữ số của bit trước đó, khởi tạo $\textit{pre} = 0$, và một biến $\textit{vis}$ để ghi nhận liệu đã tìm thấy một cặp bit 1 liên tiếp hay chưa, khởi tạo $\textit{vis} = \text{false}$.

Duyệt qua từng bit nhị phân của $n$, gọi bit hiện tại là $\textit{cur}$. Nếu $\textit{pre} = \textit{cur} = 1$, đồng thời $\textit{vis} = \text{true}$ tại thời điểm này, điều đó cho biết có nhiều cặp bit 1 liên tiếp, nên ta trả về $\text{false}$ ngay. Nếu không, ta đặt $\textit{vis}$ thành $\text{true}$. Sau đó, cập nhật $\textit{pre} = \textit{cur}$ và tiếp tục duyệt bit tiếp theo.

Sau khi kết thúc vòng lặp, nếu $\textit{vis} = \text{true}$ thì trả về $\text{true}$; ngược lại, trả về $\text{false}$.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def consecutiveSetBits(self, n: int) -> bool:
        pre = 0
        vis = False
        while n:
            cur = n & 1
            if pre == cur == 1:
                if vis:
                    return False
                vis = True
            pre = cur
            n = n >> 1
        return vis
```

#### Java

```java
class Solution {
    public boolean consecutiveSetBits(int n) {
        boolean vis = false;
        for (int pre = 0; n > 0; n >>= 1) {
            int cur = n & 1;
            if (pre == cur && cur == 1) {
                if (vis) {
                    return false;
                }
                vis = true;
            }
            pre = cur;
        }
        return vis;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool consecutiveSetBits(int n) {
        bool vis = false;
        for (int pre = 0; n > 0; n >>= 1) {
            int cur = n & 1;
            if (pre == cur && cur == 1) {
                if (vis) {
                    return false;
                }
                vis = true;
            }
            pre = cur;
        }
        return vis;
    }
};
```

#### Go

```go
func consecutiveSetBits(n int) bool {
	vis := false
	for pre := 0; n > 0; n >>= 1 {
		cur := n & 1
		if pre == cur && cur == 1 {
			if vis {
				return false
			}
			vis = true
		}
		pre = cur
	}
	return vis
}
```

#### TypeScript

```ts
function consecutiveSetBits(n: number): boolean {
    let vis = false;
    for (let pre = 0; n > 0; n >>= 1) {
        const cur = n & 1;
        if (pre === cur && cur === 1) {
            if (vis) {
                return false;
            }
            vis = true;
        }
        pre = cur;
    }
    return vis;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
