---
comments: true
difficulty: Medium
tags:
    - Minimax
    - Math
    - Dynamic Programming
    - Game Theory
---

<!-- problem:start -->

# [375. Guess Number Higher or Lower II](https://leetcode.com/problems/guess-number-higher-or-lower-ii)

[中文文档](/solution/0300-0399/0375.Guess%20Number%20Higher%20or%20Lower%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Ta đang chơi trò đoán số. Trò chơi diễn ra như sau:</p>

<ol>
	<li>Tôi chọn một số trong khoảng từ&nbsp;<code>1</code>&nbsp;đến&nbsp;<code>n</code>.</li>
	<li>Bạn đoán một số.</li>
	<li>Nếu đoán đúng, <strong>bạn thắng trò chơi</strong>.</li>
	<li>Nếu đoán sai, tôi sẽ cho biết số mình chọn <strong>lớn hơn hay nhỏ hơn</strong> số bạn đoán, rồi bạn tiếp tục đoán.</li>
	<li>Mỗi lần đoán sai số&nbsp;<code>x</code>, bạn phải trả&nbsp;<code>x</code>&nbsp;đô la. Nếu hết tiền, <strong>bạn thua trò chơi</strong>.</li>
</ol>

<p>Cho một giá trị&nbsp;<code>n</code>, hãy trả về&nbsp;<em>số tiền ít nhất cần có để&nbsp;<strong>chắc chắn thắng bất kể tôi chọn số nào</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0375.Guess%20Number%20Higher%20or%20Lower%20II/images/graph.png" style="width: 505px; height: 388px;" />
<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Chiến lược thắng như sau:
- Phạm vi là [1,10]. Đoán 7.
&nbsp;   - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $0. Nếu không, bạn trả $7.
&nbsp;   - Nếu số tôi chọn lớn hơn, phạm vi là [8,10]. Đoán 9.
&nbsp;       - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $7. Nếu không, bạn trả $9.
&nbsp;       - Nếu số tôi chọn lớn hơn, thì đó phải là 10. Đoán 10. Tổng số tiền bạn trả là $7 + $9 = $16.
&nbsp;       - Nếu số tôi chọn nhỏ hơn, thì đó phải là 8. Đoán 8. Tổng số tiền bạn trả là $7 + $9 = $16.
&nbsp;   - Nếu số tôi chọn nhỏ hơn, phạm vi là [1,6]. Đoán 3.
&nbsp;       - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $7. Nếu không, bạn trả $3.
&nbsp;       - Nếu số tôi chọn lớn hơn, phạm vi là [4,6]. Đoán 5.
&nbsp;           - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $7 + $3 = $10. Nếu không, bạn trả $5.
&nbsp;           - Nếu số tôi chọn lớn hơn, thì đó phải là 6. Đoán 6. Tổng số tiền bạn trả là $7 + $3 + $5 = $15.
&nbsp;           - Nếu số tôi chọn nhỏ hơn, thì đó phải là 4. Đoán 4. Tổng số tiền bạn trả là $7 + $3 + $5 = $15.
&nbsp;       - Nếu số tôi chọn nhỏ hơn, phạm vi là [1,2]. Đoán 1.
&nbsp;           - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $7 + $3 = $10. Nếu không, bạn trả $1.
&nbsp;           - Nếu số tôi chọn lớn hơn, thì đó phải là 2. Đoán 2. Tổng số tiền bạn trả là $7 + $3 + $1 = $11.
Trường hợp tệ nhất trong các tình huống này là bạn phải trả $16. Vì vậy, chỉ cần có $16 là bạn chắc chắn thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>&nbsp;Chỉ có một số có thể được chọn, nên bạn có thể đoán 1 mà không phải trả tiền.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>&nbsp;Có hai số có thể được chọn: 1 và 2.
- Đoán 1.
&nbsp;   - Nếu đây là số tôi chọn, tổng số tiền bạn trả là $0. Nếu không, bạn trả $1.
&nbsp;   - Nếu số tôi chọn lớn hơn, thì đó phải là 2. Đoán 2. Tổng số tiền bạn trả là $1.
Trường hợp tệ nhất là bạn phải trả $1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đoán sai sẽ mất số tiền bằng số vừa đoán; ta cần tối thiểu hóa chi phí lớn nhất để chắc chắn thắng. Các thứ tự đoán tạo thành một cây trò chơi. Đáp án tối ưu trên $[i,j]$ chỉ phụ thuộc vào các đoạn ngắn hơn.
>
> $f[i][j]$ là chi phí min-max đó. Nếu đoán $k$, chi phí là $k+\max(left,right)$; ta lấy giá trị nhỏ nhất theo mọi $k$. Tính lần lượt theo độ dài đoạn; đáp án là $f[1][n]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là chi phí nhỏ nhất cần để đoán được một số bất kỳ trong đoạn $[i, j]$. Ban đầu, $f[i][i] = 0$ vì đoán số duy nhất trong đoạn không tốn chi phí; với $i > j$, ta cũng có $f[i][j] = 0$. Đáp án là $f[1][n]$.

Để tính $f[i][j]$, ta xét mọi số $k$ trong $[i, j]$, chia đoạn thành hai phần $[i, k - 1]$ và $[k + 1, j]$, rồi lấy giá trị lớn hơn giữa hai phần cộng với chi phí đoán $k$,

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMoneyAmount(self, n: int) -> int:
        f = [[0] * (n + 1) for _ in range(n + 1)]
        for i in range(n - 1, 0, -1):
            for j in range(i + 1, n + 1):
                f[i][j] = j + f[i][j - 1]
                for k in range(i, j):
                    f[i][j] = min(f[i][j], max(f[i][k - 1], f[k + 1][j]) + k)
        return f[1][n]
```

#### Java

```java
class Solution {
    public int getMoneyAmount(int n) {
        int[][] f = new int[n + 1][n + 1];
        for (int i = n - 1; i > 0; --i) {
            for (int j = i + 1; j <= n; ++j) {
                f[i][j] = j + f[i][j - 1];
                for (int k = i; k < j; ++k) {
                    f[i][j] = Math.min(f[i][j], Math.max(f[i][k - 1], f[k + 1][j]) + k);
                }
            }
        }
        return f[1][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMoneyAmount(int n) {
        int f[n + 1][n + 1];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; i; --i) {
            for (int j = i + 1; j <= n; ++j) {
                f[i][j] = j + f[i][j - 1];
                for (int k = i; k < j; ++k) {
                    f[i][j] = min(f[i][j], max(f[i][k - 1], f[k + 1][j]) + k);
                }
            }
        }
        return f[1][n];
    }
};
```

#### Go

```go
func getMoneyAmount(n int) int {
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for i := n - 1; i > 0; i-- {
		for j := i + 1; j <= n; j++ {
			f[i][j] = j + f[i][j-1]
			for k := i; k < j; k++ {
				f[i][j] = min(f[i][j], k+max(f[i][k-1], f[k+1][j]))
			}
		}
	}
	return f[1][n]
}
```

#### TypeScript

```ts
function getMoneyAmount(n: number): number {
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));
    for (let i = n - 1; i; --i) {
        for (let j = i + 1; j <= n; ++j) {
            f[i][j] = j + f[i][j - 1];
            for (let k = i; k < j; ++k) {
                f[i][j] = Math.min(f[i][j], k + Math.max(f[i][k - 1], f[k + 1][j]));
            }
        }
    }
    return f[1][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
