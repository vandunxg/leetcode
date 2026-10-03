---
comments: true
difficulty: Medium
rating: 2024
source: Biweekly Contest 44 Q3
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [1734. Decode XORed Permutation](https://leetcode.com/problems/decode-xored-permutation)

[中文文档](/solution/1700-1799/1734.Decode%20XORed%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Có một mảng số nguyên <code>perm</code> là một hoán vị của <code>n</code> số nguyên dương đầu tiên, trong đó <code>n</code> luôn là số <strong>lẻ</strong>.</p>

<p>Nó được mã hóa thành một mảng số nguyên khác <code>encoded</code> có độ dài <code>n - 1</code>, sao cho <code>encoded[i] = perm[i] XOR perm[i + 1]</code>. Ví dụ, nếu <code>perm = [1,3,2]</code> thì <code>encoded = [2,1]</code>.</p>

<p>Cho mảng <code>encoded</code>, hãy trả về <em>mảng ban đầu</em> <code>perm</code>. Đảm bảo đáp án tồn tại và duy nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> encoded = [3,1]
<strong>Output:</strong> [1,2,3]
<strong>Giải thích:</strong> Nếu perm = [1,2,3] thì encoded = [1 XOR 2,2 XOR 3] = [3,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> encoded = [6,5,4,6]
<strong>Output:</strong> [2,4,1,5,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;&nbsp;10<sup>5</sup></code></li>
	<li><code>n</code>&nbsp;là số lẻ.</li>
	<li><code>encoded.length == n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{perm}$ là hoán vị của $1..n$ với $n$ lẻ, và $\textit{encoded}[i]=\textit{perm}[i]\oplus\textit{perm}[i+1]$. Nếu thiếu một đầu mút, ta không thể suy ngược.
>
> Ta biết $1\oplus\cdots\oplus n$. XOR các phần tử ở chỉ số chẵn của $\textit{encoded}$ loại bỏ đúng $\textit{perm}[n-1]$, từ đó khôi phục giá trị cuối.
>
> Duyệt ngược bằng công thức $\textit{perm}[i]=\textit{encoded}[i]\oplus\textit{perm}[i+1]$.

<!-- thinking:end -->

Ta nhận thấy mảng $perm$ là hoán vị của $n$ số nguyên dương đầu tiên, nên XOR của mọi phần tử trong $perm$ là $1 \oplus 2 \oplus \cdots \oplus n$, ký hiệu là $a$. Vì $encode[i]=perm[i] \oplus perm[i+1]$, nếu XOR các phần tử $encode[0],encode[2],\cdots,encode[n-3]$ là $b$ thì $perm[n-1]=a \oplus b$. Khi biết phần tử cuối của $perm$, ta có thể tìm mọi phần tử của $perm$ bằng cách duyệt mảng $encode$ theo thứ tự ngược.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $perm$. Không tính phần không gian của đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decode(self, encoded: List[int]) -> List[int]:
        n = len(encoded) + 1
        a = b = 0
        for i in range(0, n - 1, 2):
            a ^= encoded[i]
        for i in range(1, n + 1):
            b ^= i
        perm = [0] * n
        perm[-1] = a ^ b
        for i in range(n - 2, -1, -1):
            perm[i] = encoded[i] ^ perm[i + 1]
        return perm
```

#### Java

```java
class Solution {
    public int[] decode(int[] encoded) {
        int n = encoded.length + 1;
        int a = 0, b = 0;
        for (int i = 0; i < n - 1; i += 2) {
            a ^= encoded[i];
        }
        for (int i = 1; i <= n; ++i) {
            b ^= i;
        }
        int[] perm = new int[n];
        perm[n - 1] = a ^ b;
        for (int i = n - 2; i >= 0; --i) {
            perm[i] = encoded[i] ^ perm[i + 1];
        }
        return perm;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> decode(vector<int>& encoded) {
        int n = encoded.size() + 1;
        int a = 0, b = 0;
        for (int i = 0; i < n - 1; i += 2) {
            a ^= encoded[i];
        }
        for (int i = 1; i <= n; ++i) {
            b ^= i;
        }
        vector<int> perm(n);
        perm[n - 1] = a ^ b;
        for (int i = n - 2; ~i; --i) {
            perm[i] = encoded[i] ^ perm[i + 1];
        }
        return perm;
    }
};
```

#### Go

```go
func decode(encoded []int) []int {
	n := len(encoded) + 1
	a, b := 0, 0
	for i := 0; i < n-1; i += 2 {
		a ^= encoded[i]
	}
	for i := 1; i <= n; i++ {
		b ^= i
	}
	perm := make([]int, n)
	perm[n-1] = a ^ b
	for i := n - 2; i >= 0; i-- {
		perm[i] = encoded[i] ^ perm[i+1]
	}
	return perm
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
