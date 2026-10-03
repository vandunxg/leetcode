---
comments: true
difficulty: Medium
rating: 1614
source: Biweekly Contest 70 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2145. Count the Hidden Sequences](https://leetcode.com/problems/count-the-hidden-sequences)

[中文文档](/solution/2100-2199/2145.Count%20the%20Hidden%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> gồm <code>n</code> phần tử là <code>differences</code>, mô tả <strong>hiệu</strong> giữa mỗi cặp phần tử <strong>liên tiếp</strong> của một dãy <strong>ẩn</strong> có độ dài <code>(n + 1)</code>. Cụ thể hơn, gọi dãy ẩn là <code>hidden</code>, ta có <code>differences[i] = hidden[i + 1] - hidden[i]</code>.</p>

<p>Bạn được cho thêm hai số nguyên <code>lower</code> và <code>upper</code>, mô tả miền giá trị <strong>bao gồm hai đầu mút</strong> <code>[lower, upper]</code> mà dãy ẩn có thể chứa.</p>

<ul>
	<li>Ví dụ, với <code>differences = [1, -3, 4]</code>, <code>lower = 1</code>, <code>upper = 6</code>, dãy ẩn là một dãy có độ dài <code>4</code>, trong đó các phần tử nằm trong khoảng từ <code>1</code> đến <code>6</code> (<strong>bao gồm cả hai đầu mút</strong>).

    <ul>
    <li><code>[3, 4, 1, 5]</code> và <code>[4, 5, 2, 6]</code> là các dãy ẩn có thể có.</li>
    <li><code>[5, 6, 3, 7]</code> không hợp lệ vì chứa phần tử lớn hơn <code>6</code>.</li>
    <li><code>[1, 2, 3, 4]</code> không hợp lệ vì các hiệu không đúng.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em>số lượng dãy ẩn <strong>có thể có</strong></em>. Nếu không có dãy nào, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> differences = [1,-3,4], lower = 1, upper = 6
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các dãy ẩn có thể có là:
- [3, 4, 1, 5]
- [4, 5, 2, 6]
Do đó, ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> differences = [3,-4,5,1,-2], lower = -4, upper = 5
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các dãy ẩn có thể có là:
- [-3, 0, -4, 1, 2, 0]
- [-2, 1, -3, 2, 3, 1]
- [-1, 2, -2, 3, 4, 2]
- [0, 3, -1, 4, 5, 3]
Do đó, ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> differences = [4,-7,2], lower = 3, upper = 6
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có dãy ẩn nào có thể có. Do đó, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == differences.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= differences[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= lower &lt;= upper &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Các hiệu giữa những phần tử kề nhau cố định dãy ẩn, ngoại trừ một phép tịnh tiến; phần tử đầu tiên phải khiến mọi giá trị nằm trong $[\textit{lower},\textit{upper}]$. Không cần thử từng phần tử đầu tiên rồi dựng lại dãy.
>
> Xây dựng các tổng tiền tố với phần tử đầu giả là $0$, đồng thời ghi nhận $\textit{mi}$ và $\textit{mx}$. Phần tử đầu thực tế $x$ phải thỏa mãn $\textit{lower}-\textit{mi}\le x\le \textit{upper}-\textit{mx}$; số cách là độ dài khoảng này, hoặc bằng không nếu khoảng rỗng.
>
> Chỉ cần duyệt một lượt để duy trì các giá trị nhỏ nhất và lớn nhất của tổng tiền tố.

<!-- thinking:end -->

Vì mảng $\textit{differences}$ đã được xác định, nên hiệu giữa giá trị lớn nhất và nhỏ nhất của các phần tử trong mảng $\textit{hidden}$ cũng cố định. Ta chỉ cần đảm bảo hiệu này không vượt quá $\textit{upper} - \textit{lower}$.

Giả sử phần tử đầu tiên của mảng $\textit{hidden}$ là $0$. Khi đó, $\textit{hidden}[i] = \textit{hidden}[i - 1] + \textit{differences}[i - 1]$, với $1 \leq i \leq n$. Gọi giá trị lớn nhất của mảng $\textit{hidden}$ là $mx$ và giá trị nhỏ nhất là $mi$. Nếu $mx - mi \leq \textit{upper} - \textit{lower}$, ta có thể xây dựng một mảng $\textit{hidden}$ hợp lệ. Số cách xây dựng là $\textit{upper} - \textit{lower} - (mx - mi) + 1$. Ngược lại, không thể xây dựng mảng $\textit{hidden}$ hợp lệ và ta trả về $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{differences}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfArrays(self, differences: List[int], lower: int, upper: int) -> int:
        x = mi = mx = 0
        for d in differences:
            x += d
            mi = min(mi, x)
            mx = max(mx, x)
        return max(upper - lower - (mx - mi) + 1, 0)
```

#### Java

```java
class Solution {
    public int numberOfArrays(int[] differences, int lower, int upper) {
        long x = 0, mi = 0, mx = 0;
        for (int d : differences) {
            x += d;
            mi = Math.min(mi, x);
            mx = Math.max(mx, x);
        }
        return (int) Math.max(upper - lower - (mx - mi) + 1, 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfArrays(vector<int>& differences, int lower, int upper) {
        long long x = 0, mi = 0, mx = 0;
        for (int d : differences) {
            x += d;
            mi = min(mi, x);
            mx = max(mx, x);
        }
        return max(upper - lower - (mx - mi) + 1, 0LL);
    }
};
```

#### Go

```go
func numberOfArrays(differences []int, lower int, upper int) int {
	x, mi, mx := 0, 0, 0
	for _, d := range differences {
		x += d
		mi = min(mi, x)
		mx = max(mx, x)
	}
	return max(0, upper-lower-(mx-mi)+1)
}
```

#### TypeScript

```ts
function numberOfArrays(differences: number[], lower: number, upper: number): number {
    let [x, mi, mx] = [0, 0, 0];
    for (const d of differences) {
        x += d;
        mi = Math.min(mi, x);
        mx = Math.max(mx, x);
    }
    return Math.max(0, upper - lower - (mx - mi) + 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
