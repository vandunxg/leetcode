---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Math
---

<!-- problem:start -->

# [390. Elimination Game](https://leetcode.com/problems/elimination-game)

[中文文档](/solution/0300-0399/0390.Elimination%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>arr</code> gồm các số nguyên trong khoảng <code>[1, n]</code>, được sắp xếp theo thứ tự tăng nghiêm ngặt. Hãy áp dụng thuật toán sau lên <code>arr</code>:</p>

<ul>
	<li>Duyệt từ trái sang phải, xóa số đầu tiên rồi xóa cách một số tiếp theo cho đến hết danh sách.</li>
	<li>Lặp lại bước trên nhưng lần này duyệt từ phải sang trái, xóa số ngoài cùng bên phải rồi xóa cách một số trong các số còn lại.</li>
	<li>Tiếp tục lặp lại các bước, luân phiên duyệt từ trái sang phải và từ phải sang trái, cho đến khi chỉ còn một số.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số cuối cùng còn lại trong</em> <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 9
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
arr = [<u>1</u>, 2, <u>3</u>, 4, <u>5</u>, 6, <u>7</u>, 8, <u>9</u>]
arr = [2, <u>4</u>, 6, <u>8</u>]
arr = [<u>2</u>, 6]
arr = [6]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xóa cách một số, luân phiên từ trái sang phải và từ phải sang trái. Mô phỏng trực tiếp danh sách tốn $O(n)$, không phù hợp với $n\le 10^9$. Sau mỗi lượt, số phần tử giảm một nửa; có thể cập nhật vị trí đầu, cuối và bước nhảy bằng công thức.
>
> Theo dõi phần tử đầu hiện tại $a1$, phần tử cuối $an$, bước nhảy và số lượng phần tử. Khi duyệt từ trái sang phải, phần tử đầu luôn dịch chuyển; khi duyệt từ phải sang trái, nó dịch chuyển nếu số phần tử là lẻ. Quy tắc cho phần tử cuối đối xứng. Khi chỉ còn một phần tử, trả về $a1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastRemaining(self, n: int) -> int:
        a1, an = 1, n
        i, step, cnt = 0, 1, n
        while cnt > 1:
            if i % 2:
                an -= step
                if cnt % 2:
                    a1 += step
            else:
                a1 += step
                if cnt % 2:
                    an -= step
            cnt >>= 1
            step <<= 1
            i += 1
        return a1
```

#### Java

```java
class Solution {
    public int lastRemaining(int n) {
        int a1 = 1, an = n, step = 1;
        for (int i = 0, cnt = n; cnt > 1; cnt >>= 1, step <<= 1, ++i) {
            if (i % 2 == 1) {
                an -= step;
                if (cnt % 2 == 1) {
                    a1 += step;
                }
            } else {
                a1 += step;
                if (cnt % 2 == 1) {
                    an -= step;
                }
            }
        }
        return a1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lastRemaining(int n) {
        int a1 = 1, an = n, step = 1;
        for (int i = 0, cnt = n; cnt > 1; cnt >>= 1, step <<= 1, ++i) {
            if (i % 2) {
                an -= step;
                if (cnt % 2) a1 += step;
            } else {
                a1 += step;
                if (cnt % 2) an -= step;
            }
        }
        return a1;
    }
};
```

#### Go

```go
func lastRemaining(n int) int {
	a1, an, step := 1, n, 1
	for i, cnt := 0, n; cnt > 1; cnt, step, i = cnt>>1, step<<1, i+1 {
		if i%2 == 1 {
			an -= step
			if cnt%2 == 1 {
				a1 += step
			}
		} else {
			a1 += step
			if cnt%2 == 1 {
				an -= step
			}
		}
	}
	return a1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
