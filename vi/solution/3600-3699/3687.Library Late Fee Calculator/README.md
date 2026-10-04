---
comments: true
difficulty: Easy
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3687. Library Late Fee Calculator 🔒](https://leetcode.com/problems/library-late-fee-calculator)

[中文文档](/solution/3600-3699/3687.Library%20Late%20Fee%20Calculator/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>daysLate</code>, trong đó <code>daysLate[i]</code> cho biết quyển sách thứ <code>i<sup>th</sup></code> được trả muộn bao nhiêu ngày.</p>

<p>Mức phạt được tính như sau:</p>

<ul>
	<li>Nếu <code>daysLate[i] == 1</code>, mức phạt là 1.</li>
	<li>Nếu <code>2 &lt;= daysLate[i] &lt;= 5</code>, mức phạt là <code>2 * daysLate[i]</code>.</li>
	<li>Nếu <code>daysLate[i] &gt; 5</code>, mức phạt là <code>3 * daysLate[i]</code>.</li>
</ul>

<p>Hãy trả về tổng mức phạt của tất cả các quyển sách.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">daysLate = [5,1,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">32</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>daysLate[0] = 5</code>: Mức phạt là <code>2 * daysLate[0] = 2 * 5 = 10</code>.</li>
	<li><code>daysLate[1] = 1</code>: Mức phạt là <code>1</code>.</li>
	<li><code>daysLate[2] = 7</code>: Mức phạt là <code>3 * daysLate[2] = 3 * 7 = 21</code>.</li>
	<li>Vậy tổng mức phạt là <code>10 + 1 + 21 = 32</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">daysLate = [1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>daysLate[0] = 1</code>: Mức phạt là <code>1</code>.</li>
	<li><code>daysLate[1] = 1</code>: Mức phạt là <code>1</code>.</li>
	<li>Vậy tổng mức phạt là <code>1 + 1 = 2</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= daysLate.length &lt;= 100</code></li>
	<li><code>1 &lt;= daysLate[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mức phạt phụ thuộc từng đoạn vào số ngày trả muộn: $1$ nếu muộn một ngày, $2x$ nếu muộn từ $2\ldots 5$ ngày, và $3x$ nếu muộn hơn. Các quyển sách độc lập với nhau.
>
> Một hàm phụ $f$ cài đặt ba trường hợp trên và được tính tổng trên từng phần tử của $\textit{daysLate}$. Với $n\le 100$, không cần dùng thêm cấu trúc dữ liệu nào.

<!-- thinking:end -->

Ta định nghĩa một hàm $\text{f}(x)$ để tính phí phạt trả muộn cho từng quyển sách:

$$
\text{f}(x) = \begin{cases}
1 & x = 1 \\
2x & 2 \leq x \leq 5 \\
3x & x > 5
\end{cases}
$$

Sau đó, với mỗi phần tử $x$ trong mảng $\textit{daysLate}$, ta tính $\text{f}(x)$ rồi cộng lại để nhận được tổng phí phạt trả muộn.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{daysLate}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lateFee(self, daysLate: List[int]) -> int:
        def f(x: int) -> int:
            if x == 1:
                return 1
            if x > 5:
                return 3 * x
            return 2 * x

        return sum(f(x) for x in daysLate)
```

#### Java

```java
class Solution {
    public int lateFee(int[] daysLate) {
        IntUnaryOperator f = x -> {
            if (x == 1) {
                return 1;
            } else if (x > 5) {
                return 3 * x;
            } else {
                return 2 * x;
            }
        };

        int ans = 0;
        for (int x : daysLate) {
            ans += f.applyAsInt(x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lateFee(vector<int>& daysLate) {
        auto f = [](int x) {
            if (x == 1) {
                return 1;
            } else if (x > 5) {
                return 3 * x;
            } else {
                return 2 * x;
            }
        };

        int ans = 0;
        for (int x : daysLate) {
            ans += f(x);
        }
        return ans;
    }
};
```

#### Go

```go
func lateFee(daysLate []int) (ans int) {
	f := func(x int) int {
		if x == 1 {
			return 1
		} else if x > 5 {
			return 3 * x
		}
		return 2 * x
	}
	for _, x := range daysLate {
		ans += f(x)
	}
	return
}
```

#### TypeScript

```ts
function lateFee(daysLate: number[]): number {
    const f = (x: number): number => {
        if (x === 1) {
            return 1;
        } else if (x > 5) {
            return 3 * x;
        }
        return 2 * x;
    };
    return daysLate.reduce((acc, days) => acc + f(days), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
