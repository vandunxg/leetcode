---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2291. Maximum Profit From Trading Stocks 🔒](https://leetcode.com/problems/maximum-profit-from-trading-stocks)

[中文文档](/solution/2200-2299/2291.Maximum%20Profit%20From%20Trading%20Stocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>present</code> và <code>future</code> có cùng độ dài, được đánh chỉ số từ <strong>0</strong>, trong đó <code>present[i]</code> là giá hiện tại của cổ phiếu thứ <code>i<sup>th</sup></code>, còn <code>future[i]</code> là giá của cổ phiếu thứ <code>i<sup>th</sup></code> sau một năm. Bạn có thể mua mỗi cổ phiếu nhiều nhất <strong>một</strong> lần. Ngoài ra, bạn được cho một số nguyên <code>budget</code> biểu thị số tiền hiện có.</p>

<p>Hãy trả về <em>lợi nhuận lớn nhất bạn có thể thu được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> present = [5,4,6,2,3], future = [8,5,4,3,5], budget = 10
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Một cách để tối đa hóa lợi nhuận là:
Mua các cổ phiếu thứ 0<sup>th</sup>, 3<sup>rd</sup> và 4<sup>th</sup> với tổng giá là 5 + 2 + 3 = 10.
Năm sau, bán cả ba cổ phiếu với tổng giá là 8 + 3 + 5 = 16.
Lợi nhuận thu được là 16 - 10 = 6.
Có thể chứng minh rằng lợi nhuận lớn nhất bạn có thể thu được là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> present = [2,2,5], future = [3,4,10], budget = 6
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Cách duy nhất để tối đa hóa lợi nhuận là:
Mua cổ phiếu thứ 2<sup>nd</sup>, thu được lợi nhuận 10 - 5 = 5.
Có thể chứng minh rằng lợi nhuận lớn nhất bạn có thể thu được là 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> present = [3,3,12], future = [0,3,15], budget = 10
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Một cách để tối đa hóa lợi nhuận là:
Mua cổ phiếu thứ 1<sup>st</sup>, thu được lợi nhuận 3 - 3 = 0.
Có thể chứng minh rằng lợi nhuận lớn nhất bạn có thể thu được là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == present.length == future.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= present[i], future[i] &lt;= 100</code></li>
	<li><code>0 &lt;= budget &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cổ phiếu chỉ được mua một lần: trả giá hiện tại, thu được $\textit{future}-\textit{present}$, với tổng chi phí không vượt quá ngân sách. Vì $n$ và ngân sách đều bằng $10^3$, đây là bài toán ba lô $0$-$1$, trong đó kích thước của một món đồ là giá hiện tại và giá trị là lợi nhuận (chỉ xét khi lợi nhuận dương).
>
> $f[i][j]$ là lợi nhuận lớn nhất từ $i$ cổ phiếu đầu tiên với ngân sách $j$. Bỏ qua một cổ phiếu sẽ sao chép hàng trước; mua cổ phiếu yêu cầu $j\ge present[i]$ và giá tương lai cao hơn.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là lợi nhuận lớn nhất khi xét $i$ cổ phiếu đầu tiên với ngân sách $j$. Đáp án là $f[n][\textit{budget}]$.

Với cổ phiếu thứ $i$, ta có hai lựa chọn:

- Không mua cổ phiếu đó, khi đó $f[i][j] = f[i - 1][j]$;
- Mua cổ phiếu đó, khi đó $f[i][j] = f[i - 1][j - \textit{present}[i]] + \textit{future}[i] - \textit{present}[i]$.

Cuối cùng, trả về $f[n][\textit{budget}]$.

Độ phức tạp thời gian là $O(n \times \textit{budget})$, độ phức tạp không gian là $O(n \times \textit{budget})$. Trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProfit(self, present: List[int], future: List[int], budget: int) -> int:
        f = [[0] * (budget + 1) for _ in range(len(present) + 1)]
        for i, w in enumerate(present, 1):
            for j in range(budget + 1):
                f[i][j] = f[i - 1][j]
                if j >= w and future[i - 1] > w:
                    f[i][j] = max(f[i][j], f[i - 1][j - w] + future[i - 1] - w)
        return f[-1][-1]
```

#### Java

```java
class Solution {
    public int maximumProfit(int[] present, int[] future, int budget) {
        int n = present.length;
        int[][] f = new int[n + 1][budget + 1];
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= budget; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= present[i - 1]) {
                    f[i][j] = Math.max(
                        f[i][j], f[i - 1][j - present[i - 1]] + future[i - 1] - present[i - 1]);
                }
            }
        }
        return f[n][budget];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProfit(vector<int>& present, vector<int>& future, int budget) {
        int n = present.size();
        int f[n + 1][budget + 1];
        memset(f, 0, sizeof f);
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= budget; ++j) {
                f[i][j] = f[i - 1][j];
                if (j >= present[i - 1]) {
                    f[i][j] = max(f[i][j], f[i - 1][j - present[i - 1]] + future[i - 1] - present[i - 1]);
                }
            }
        }
        return f[n][budget];
    }
};
```

#### Go

```go
func maximumProfit(present []int, future []int, budget int) int {
	n := len(present)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, budget+1)
	}
	for i := 1; i <= n; i++ {
		for j := 0; j <= budget; j++ {
			f[i][j] = f[i-1][j]
			if j >= present[i-1] {
				f[i][j] = max(f[i][j], f[i-1][j-present[i-1]]+future[i-1]-present[i-1])
			}
		}
	}
	return f[n][budget]
}
```

#### TypeScript

```ts
function maximumProfit(present: number[], future: number[], budget: number): number {
    const f = new Array(budget + 1).fill(0);
    for (let i = 0; i < present.length; ++i) {
        const [a, b] = [present[i], future[i]];
        for (let j = budget; j >= a; --j) {
            f[j] = Math.max(f[j], f[j - a] + b - a);
        }
    }
    return f[budget];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ đọc hàng trước đó, nên có thể làm phẳng $f$ thành một chiều. Duyệt ngân sách $j$ theo chiều giảm dần để một cổ phiếu không bị sử dụng hai lần. Thời gian không đổi; không gian bổ sung là $O(\textit{budget})$.

<!-- thinking:end -->

Ta nhận thấy ở mỗi hàng, ta chỉ cần các giá trị của hàng trước đó, nên có thể tối ưu độ phức tạp không gian xuống còn $O(\text{budget})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProfit(self, present: List[int], future: List[int], budget: int) -> int:
        f = [0] * (budget + 1)
        for a, b in zip(present, future):
            for j in range(budget, a - 1, -1):
                f[j] = max(f[j], f[j - a] + b - a)
        return f[-1]
```

#### Java

```java
class Solution {
    public int maximumProfit(int[] present, int[] future, int budget) {
        int n = present.length;
        int[] f = new int[budget + 1];
        for (int i = 0; i < n; ++i) {
            int a = present[i], b = future[i];
            for (int j = budget; j >= a; --j) {
                f[j] = Math.max(f[j], f[j - a] + b - a);
            }
        }
        return f[budget];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProfit(vector<int>& present, vector<int>& future, int budget) {
        int n = present.size();
        int f[budget + 1];
        memset(f, 0, sizeof f);
        for (int i = 0; i < n; ++i) {
            int a = present[i], b = future[i];
            for (int j = budget; j >= a; --j) {
                f[j] = max(f[j], f[j - a] + b - a);
            }
        }
        return f[budget];
    }
};
```

#### Go

```go
func maximumProfit(present []int, future []int, budget int) int {
	f := make([]int, budget+1)
	for i, a := range present {
		for j := budget; j >= a; j-- {
			f[j] = max(f[j], f[j-a]+future[i]-a)
		}
	}
	return f[budget]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
