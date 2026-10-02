---
comments: true
difficulty: Hard
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [629. K Inverse Pairs Array](https://leetcode.com/problems/k-inverse-pairs-array)

[中文文档](/solution/0600-0699/0629.K%20Inverse%20Pairs%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Với mảng số nguyên <code>nums</code>, một <strong>cặp nghịch thế</strong> là cặp chỉ số <code>[i, j]</code> thỏa mãn <code>0 &lt;= i &lt; j &lt; nums.length</code> và <code>nums[i] &gt; nums[j]</code>.</p>

<p>Cho hai số nguyên n và k, hãy trả về số mảng khác nhau gồm các số từ <code>1</code> đến <code>n</code> có đúng <code>k</code> <strong>cặp nghịch thế</strong>. Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 0
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có mảng [1,2,3] gồm các số từ 1 đến 3 có đúng 0 cặp nghịch thế.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai mảng [1,3,2] và [2,1,3] có đúng 1 cặp nghịch thế.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Prefix Sum

<!-- thinking:start -->

> **Ý tưởng**
>
> Với $n,k\le 10^3$, không thể đếm bằng cách liệt kê các hoán vị độ dài $n$ có đúng $k$ nghịch thế.
>
> Gọi $f[i][j]$ là số cách tương ứng. Khi chèn $i$, số nghịch thế tăng từ $0$ đến $i-1$, nên $f[i][j]$ là tổng trên một đoạn của hàng trước. Dùng prefix sum giúp mỗi bước chuyển mất $O(1)$; dùng mảng cuốn vòng giữ bộ nhớ ở mức $O(k)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số mảng độ dài $i$ có $j$ cặp nghịch thế. Ban đầu, $f[0][0] = 1$, còn các giá trị $f[i][j]$ khác bằng 0.

Tiếp theo, ta xét cách tính $f[i][j]$.

Giả sử đã xác định $i-1$ số đầu tiên và cần chèn số $i$. Ta xét từng vị trí có thể chèn $i$:

- Nếu chèn $i$ vào vị trí thứ nhất, số cặp nghịch thế tăng thêm $i-1$, nên $f[i][j] += f[i-1][j-(i-1)]$.
- Nếu chèn $i$ vào vị trí thứ hai, số cặp nghịch thế tăng thêm $i-2$, nên $f[i][j] += f[i-1][j-(i-2)]$.
- ...
- Nếu chèn $i$ vào vị trí thứ $(i-1)$, số cặp nghịch thế tăng thêm 1, nên $f[i][j] += f[i-1][j-1]$.
- Nếu chèn $i$ vào vị trí thứ $i$, số cặp nghịch thế không đổi, nên $f[i][j] += f[i-1][j]$.

Do đó, $f[i][j] = \sum_{k=1}^{i} f[i-1][j-(i-k)]$.

Ta nhận thấy phép tính $f[i][j]$ thực chất liên quan đến prefix sum, nên có thể dùng prefix sum để tối ưu. Ngoài ra, vì $f[i][j]$ chỉ phụ thuộc vào $f[i-1][j]$, ta có thể dùng mảng một chiều để giảm độ phức tạp bộ nhớ.

Độ phức tạp thời gian là $O(n \times k)$, còn độ phức tạp bộ nhớ là $O(k)$. Ở đây, $n$ là độ dài mảng và $k$ là số cặp nghịch thế.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kInversePairs(self, n: int, k: int) -> int:
        mod = 10**9 + 7
        f = [1] + [0] * k
        s = [0] * (k + 2)
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                f[j] = (s[j + 1] - s[max(0, j - (i - 1))]) % mod
            for j in range(1, k + 2):
                s[j] = (s[j - 1] + f[j - 1]) % mod
        return f[k]
```

#### Java

```java
class Solution {
    public int kInversePairs(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[k + 1];
        int[] s = new int[k + 2];
        f[0] = 1;
        Arrays.fill(s, 1);
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[j] = (s[j + 1] - s[Math.max(0, j - (i - 1))] + mod) % mod;
            }
            for (int j = 1; j <= k + 1; ++j) {
                s[j] = (s[j - 1] + f[j - 1]) % mod;
            }
        }
        return f[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kInversePairs(int n, int k) {
        int f[k + 1];
        int s[k + 2];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        fill(s, s + k + 2, 1);
        s[0] = 0;
        const int mod = 1e9 + 7;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                f[j] = (s[j + 1] - s[max(0, j - (i - 1))] + mod) % mod;
            }
            for (int j = 1; j <= k + 1; ++j) {
                s[j] = (s[j - 1] + f[j - 1]) % mod;
            }
        }
        return f[k];
    }
};
```

#### Go

```go
func kInversePairs(n int, k int) int {
	f := make([]int, k+1)
	s := make([]int, k+2)
	f[0] = 1
	for i, x := range f {
		s[i+1] = s[i] + x
	}
	const mod = 1e9 + 7
	for i := 1; i <= n; i++ {
		for j := 1; j <= k; j++ {
			f[j] = (s[j+1] - s[max(0, j-(i-1))] + mod) % mod
		}
		for j := 1; j <= k+1; j++ {
			s[j] = (s[j-1] + f[j-1]) % mod
		}
	}
	return f[k]
}
```

#### TypeScript

```ts
function kInversePairs(n: number, k: number): number {
    const f: number[] = Array(k + 1).fill(0);
    f[0] = 1;
    const s: number[] = Array(k + 2).fill(1);
    s[0] = 0;
    const mod: number = 1e9 + 7;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= k; ++j) {
            f[j] = (s[j + 1] - s[Math.max(0, j - (i - 1))] + mod) % mod;
        }
        for (let j = 1; j <= k + 1; ++j) {
            s[j] = (s[j - 1] + f[j - 1]) % mod;
        }
    }
    return f[k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
