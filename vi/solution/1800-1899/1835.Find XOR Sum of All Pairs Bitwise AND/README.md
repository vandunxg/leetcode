---
comments: true
difficulty: Hard
rating: 1825
source: Weekly Contest 237 Q4
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [1835. Find XOR Sum of All Pairs Bitwise AND](https://leetcode.com/problems/find-xor-sum-of-all-pairs-bitwise-and)

[中文文档](/solution/1800-1899/1835.Find%20XOR%20Sum%20of%20All%20Pairs%20Bitwise%20AND/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Tổng XOR</strong> của một danh sách là phép <code>XOR</code> theo bit của tất cả phần tử trong danh sách. Nếu danh sách chỉ chứa một phần tử, thì <strong>tổng XOR</strong> bằng chính phần tử đó.</p>

<ul>
	<li>Ví dụ, <strong>tổng XOR</strong> của <code>[1,2,3,4]</code> bằng <code>1 XOR 2 XOR 3 XOR 4 = 4</code>, còn <strong>tổng XOR</strong> của <code>[3]</code> bằng <code>3</code>.</li>
</ul>

<p>Cho hai mảng <strong>đánh chỉ số từ 0</strong> <code>arr1</code> và <code>arr2</code> chỉ gồm các số nguyên không âm.</p>

<p>Xét danh sách chứa kết quả của <code>arr1[i] AND arr2[j]</code> (phép <code>AND</code> theo bit) với mọi cặp <code>(i, j)</code> sao cho <code>0 &lt;= i &lt; arr1.length</code> và <code>0 &lt;= j &lt; arr2.length</code>.</p>

<p>Trả về <em><strong>tổng XOR</strong> của danh sách nói trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,2,3], arr2 = [6,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Danh sách = [1 AND 6, 1 AND 5, 2 AND 6, 2 AND 5, 3 AND 6, 3 AND 5] = [0,1,2,0,2,1].
Tổng XOR = 0 XOR 1 XOR 2 XOR 0 XOR 2 XOR 1 = 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [12], arr2 = [4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Danh sách = [12 AND 4] = [4]. Tổng XOR = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length, arr2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= arr1[i], arr2[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính $\bigoplus_{i,j}(arr1[i]\wedge arr2[j])$. Không thể liệt kê các cặp khi độ dài có thể lên tới $10^5$.
>
> Xét theo từng bit, AND hoạt động như phép nhân còn XOR giống phép cộng không nhớ, nên biểu thức bằng $(\bigoplus arr1)\wedge(\bigoplus arr2)$. Ta tính XOR của từng mảng rồi AND hai kết quả.

<!-- thinking:end -->

Giả sử các phần tử của mảng $arr1$ là $a_1, a_2, ..., a_n$, còn các phần tử của mảng $arr2$ là $b_1, b_2, ..., b_m$. Khi đó, đáp án của bài toán là:

$$
\begin{aligned}
\textit{ans} &= (a_1 \wedge b_1) \oplus (a_1 \wedge b_2) ... (a_1 \wedge b_m) \\
&\quad \oplus (a_2 \wedge b_1) \oplus (a_2 \wedge b_2) ... (a_2 \wedge b_m) \\
&\quad \oplus \cdots \\
&\quad \oplus (a_n \wedge b_1) \oplus (a_n \wedge b_2) ... (a_n \wedge b_m) \\
\end{aligned}
$$

Trong đại số Boolean, phép XOR là phép cộng không nhớ và phép AND là phép nhân, nên công thức trên có thể rút gọn thành:

$$
\textit{ans} = (a_1 \oplus a_2 \oplus \cdots \oplus a_n) \wedge (b_1 \oplus b_2 \oplus \cdots \oplus b_m)
$$

Nói cách khác, đó là phép AND theo bit của tổng XOR của mảng $arr1$ và tổng XOR của mảng $arr2$.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của các mảng $arr1$ và $arr2$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getXORSum(self, arr1: List[int], arr2: List[int]) -> int:
        a = reduce(xor, arr1)
        b = reduce(xor, arr2)
        return a & b
```

#### Java

```java
class Solution {
    public int getXORSum(int[] arr1, int[] arr2) {
        int a = 0, b = 0;
        for (int v : arr1) {
            a ^= v;
        }
        for (int v : arr2) {
            b ^= v;
        }
        return a & b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getXORSum(vector<int>& arr1, vector<int>& arr2) {
        int a = accumulate(arr1.begin(), arr1.end(), 0, bit_xor<int>());
        int b = accumulate(arr2.begin(), arr2.end(), 0, bit_xor<int>());
        return a & b;
    }
};
```

#### Go

```go
func getXORSum(arr1 []int, arr2 []int) int {
	var a, b int
	for _, v := range arr1 {
		a ^= v
	}
	for _, v := range arr2 {
		b ^= v
	}
	return a & b
}
```

#### TypeScript

```ts
function getXORSum(arr1: number[], arr2: number[]): number {
    const a = arr1.reduce((acc, x) => acc ^ x);
    const b = arr2.reduce((acc, x) => acc ^ x);
    return a & b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
