---
comments: true
difficulty: Medium
rating: 1408
source: Weekly Contest 272 Q3
tags:
    - Array
    - Math
    - Two Pointers
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [2110. Number of Smooth Descent Periods of a Stock](https://leetcode.com/problems/number-of-smooth-descent-periods-of-a-stock)

[中文文档](/solution/2100-2199/2110.Number%20of%20Smooth%20Descent%20Periods%20of%20a%20Stock/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>prices</code> biểu diễn lịch sử giá cổ phiếu theo ngày, trong đó <code>prices[i]</code> là giá cổ phiếu vào ngày thứ <code>i<sup>th</sup></code>.</p>

<p>Một <strong>giai đoạn giảm đều</strong> của cổ phiếu gồm <strong>một hoặc nhiều ngày liên tiếp</strong>, sao cho giá mỗi ngày <strong>thấp hơn</strong> giá ngày <strong>liền trước</strong> <strong>chính xác</strong> <code>1</code>. Ngày đầu tiên của giai đoạn không phải tuân theo quy tắc này.</p>

<p>Hãy trả về <em>số lượng <strong>giai đoạn giảm đều</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [3,2,1,4]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 7 giai đoạn giảm đều:
[3], [2], [1], [4], [3,2], [2,1] và [3,2,1]
Lưu ý rằng theo định nghĩa, một giai đoạn chỉ có một ngày cũng là một giai đoạn giảm đều.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [8,6,7,7]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 giai đoạn giảm đều: [8], [6], [7] và [7]
Lưu ý rằng [8,6] không phải là một giai đoạn giảm đều vì 8 - 6 &ne; 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> prices = [1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 1 giai đoạn giảm đều: [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một giai đoạn giảm đều là một đoạn chỉ số liên tiếp mà các giá trị kề nhau chênh lệch chính xác $1$. Một đoạn có độ dài $L$ chứa $L(L+1)/2$ mảng con không rỗng. Việc kiểm tra mọi mảng con có độ phức tạp $O(n^2)$ và không phù hợp khi $n\le 10^5$.
>
> Các đoạn giảm đều cực đại không giao nhau và có thể được chia một cách tham lam: kéo dài sang phải khi hiệu giữa các phần tử kề nhau vẫn bằng $1$, sau đó cộng số lượng mảng con của đoạn bằng công thức tam giác.
>
> Hai con trỏ $i$ và $j$ đánh dấu từng đoạn; sau khi cộng $\textit{cnt}(\textit{cnt}+1)/2$, ta bắt đầu lại từ $j$.

<!-- thinking:end -->

Ta định nghĩa một biến đáp án $\textit{ans}$ với giá trị ban đầu là $0$.

Tiếp theo, ta sử dụng hai con trỏ $i$ và $j$, lần lượt trỏ đến ngày đầu tiên của giai đoạn giảm đều hiện tại và ngày ngay sau ngày cuối cùng. Ban đầu, $i = 0$ và $j = 0$.

Ta duyệt mảng $\textit{prices}$ từ trái sang phải. Với mỗi vị trí $i$, ta dịch chuyển $j$ sang phải cho đến khi $j$ chạm cuối mảng hoặc $\textit{prices}[j - 1] - \textit{prices}[j] \neq 1$. Khi đó, $\textit{cnt} = j - i$ là độ dài của giai đoạn giảm đều hiện tại, và ta cộng $\frac{(1 + \textit{cnt}) \times \textit{cnt}}{2}$ vào biến đáp án $\textit{ans}$. Sau đó, ta cập nhật $i$ thành $j$ và tiếp tục duyệt.

Sau khi duyệt xong, ta trả về biến đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{prices}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getDescentPeriods(self, prices: List[int]) -> int:
        ans = 0
        i, n = 0, len(prices)
        while i < n:
            j = i + 1
            while j < n and prices[j - 1] - prices[j] == 1:
                j += 1
            cnt = j - i
            ans += (1 + cnt) * cnt // 2
            i = j
        return ans
```

#### Java

```java
class Solution {
    public long getDescentPeriods(int[] prices) {
        long ans = 0;
        int n = prices.length;
        for (int i = 0, j = 0; i < n; i = j) {
            j = i + 1;
            while (j < n && prices[j - 1] - prices[j] == 1) {
                ++j;
            }
            int cnt = j - i;
            ans += (1L + cnt) * cnt / 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long getDescentPeriods(vector<int>& prices) {
        long long ans = 0;
        int n = prices.size();
        for (int i = 0, j = 0; i < n; i = j) {
            j = i + 1;
            while (j < n && prices[j - 1] - prices[j] == 1) {
                ++j;
            }
            int cnt = j - i;
            ans += (1LL + cnt) * cnt / 2;
        }
        return ans;
    }
};
```

#### Go

```go
func getDescentPeriods(prices []int) (ans int64) {
	n := len(prices)
	for i, j := 0, 0; i < n; i = j {
		j = i + 1
		for j < n && prices[j-1]-prices[j] == 1 {
			j++
		}
		cnt := j - i
		ans += int64((1 + cnt) * cnt / 2)
	}
	return
}
```

#### TypeScript

```ts
function getDescentPeriods(prices: number[]): number {
    let ans = 0;
    const n = prices.length;
    for (let i = 0, j = 0; i < n; i = j) {
        j = i + 1;
        while (j < n && prices[j - 1] - prices[j] === 1) {
            ++j;
        }
        const cnt = j - i;
        ans += Math.floor(((1 + cnt) * cnt) / 2);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_descent_periods(prices: Vec<i32>) -> i64 {
        let mut ans: i64 = 0;
        let n: usize = prices.len();
        let mut i: usize = 0;

        while i < n {
            let mut j: usize = i + 1;
            while j < n && prices[j - 1] - prices[j] == 1 {
                j += 1;
            }
            let cnt: i64 = (j - i) as i64;
            ans += (1 + cnt) * cnt / 2;
            i = j;
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public long GetDescentPeriods(int[] prices) {
        long ans = 0;
        int n = prices.Length;
        for (int i = 0, j = 0; i < n; i = j) {
            j = i + 1;
            while (j < n && prices[j - 1] - prices[j] == 1) {
                ++j;
            }
            int cnt = j - i;
            ans += (1L + cnt) * cnt / 2;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
