---
comments: true
difficulty: Medium
rating: 1491
source: Weekly Contest 234 Q2
tags:
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [1806. Minimum Number of Operations to Reinitialize a Permutation](https://leetcode.com/problems/minimum-number-of-operations-to-reinitialize-a-permutation)

[中文文档](/solution/1800-1899/1806.Minimum%20Number%20of%20Operations%20to%20Reinitialize%20a%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>chẵn</strong> <code>n</code>​​​​​​. Ban đầu, ta có một hoán vị <code>perm</code> kích thước <code>n</code>​​, trong đó <code>perm[i] == i</code>​ (đánh chỉ số từ <strong>0</strong>)​​​​.</p>

<p>Trong một phép toán, ta tạo một mảng mới <code>arr</code> và với mỗi <code>i</code>:</p>

<ul>
	<li>Nếu <code>i % 2 == 0</code>, thì <code>arr[i] = perm[i / 2]</code>.</li>
	<li>Nếu <code>i % 2 == 1</code>, thì <code>arr[i] = perm[n / 2 + (i - 1) / 2]</code>.</li>
</ul>

<p>Sau đó, gán <code>arr</code>​​​​ cho <code>perm</code>.</p>

<p>Hãy trả về <em>số phép toán <strong>khác 0</strong> nhỏ nhất cần thực hiện trên </em><code>perm</code><em> để đưa hoán vị về giá trị ban đầu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ban đầu perm = [0,1].
Sau phép toán thứ 1<sup>st</sup>, perm = [0,1]
Vậy chỉ cần 1 phép toán.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ban đầu perm = [0,1,2,3].
Sau phép toán thứ 1<sup>st</sup>, perm = [0,2,1,3]
Sau phép toán thứ 2<sup>nd</sup>, perm = [0,1,2,3]
Vậy chỉ cần 2 phép toán.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>n</code>​​​​​​ là số chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm quy luật + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phép toán đưa nửa đầu của mảng vào các chỉ số chẵn và nửa sau vào các chỉ số lẻ. Mô phỏng toàn bộ hoán vị tốn $O(n)$ cho mỗi vòng, và có thể có $\Theta(n)$ vòng; cách này vẫn chạy được, nhưng ta không cần lưu mọi vị trí.
>
> Ánh xạ chỉ số giống nhau ở mỗi vòng. Ngoại trừ $0$ và $n-1$, khi một chỉ số trở về vị trí ban đầu thì toàn bộ hoán vị cũng được khôi phục. Theo dõi chỉ số $1$: nếu nó nằm trong nửa đầu thì chuyển thành $i\ll 1$, nếu không thì chuyển thành $(i-(n\gg 1))\ll 1\mid 1$. Số bước cho đến khi nó trở lại $1$ chính là đáp án.

<!-- thinking:end -->

Ta quan sát quy luật thay đổi của các số và nhận thấy:

1. Các số ở chỉ số chẵn của mảng mới là các số ở nửa đầu của mảng ban đầu theo đúng thứ tự;
1. Các số ở chỉ số lẻ của mảng mới là các số ở nửa sau của mảng ban đầu theo đúng thứ tự.

Nói cách khác, nếu chỉ số $i$ của một số trong mảng ban đầu nằm trong khoảng `[0, n >> 1)`, thì chỉ số mới của số đó là `i << 1`; ngược lại, chỉ số mới là `(i - (n >> 1)) << 1 | 1`.

Ngoài ra, đường đi của các số là giống nhau trong mỗi vòng lặp. Chỉ cần một số (ngoại trừ các số $0$ và $n-1$) trở về vị trí ban đầu, toàn bộ dãy sẽ trùng với dãy trước đó.

Vì vậy, ta chọn số $1$, vốn có chỉ số ban đầu cũng là $1$. Mỗi lần ta di chuyển số $1$ đến vị trí mới, cho đến khi số $1$ trở lại vị trí ban đầu, ta sẽ có số phép toán nhỏ nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reinitializePermutation(self, n: int) -> int:
        ans, i = 0, 1
        while 1:
            ans += 1
            if i < n >> 1:
                i <<= 1
            else:
                i = (i - (n >> 1)) << 1 | 1
            if i == 1:
                return ans
```

#### Java

```java
class Solution {
    public int reinitializePermutation(int n) {
        int ans = 0;
        for (int i = 1;;) {
            ++ans;
            if (i < (n >> 1)) {
                i <<= 1;
            } else {
                i = (i - (n >> 1)) << 1 | 1;
            }
            if (i == 1) {
                return ans;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reinitializePermutation(int n) {
        int ans = 0;
        for (int i = 1;;) {
            ++ans;
            if (i < (n >> 1)) {
                i <<= 1;
            } else {
                i = (i - (n >> 1)) << 1 | 1;
            }
            if (i == 1) {
                return ans;
            }
        }
    }
};
```

#### Go

```go
func reinitializePermutation(n int) (ans int) {
	for i := 1; ; {
		ans++
		if i < (n >> 1) {
			i <<= 1
		} else {
			i = (i-(n>>1))<<1 | 1
		}
		if i == 1 {
			return ans
		}
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
