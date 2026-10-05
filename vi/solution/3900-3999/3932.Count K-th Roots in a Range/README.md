---
comments: true
difficulty: Medium
rating: 1548
source: Weekly Contest 502 Q2
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [3932. Count K-th Roots in a Range](https://leetcode.com/problems/count-k-th-roots-in-a-range)

[中文文档](/solution/3900-3999/3932.Count%20K-th%20Roots%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>l</code>, <code>r</code> và <code>k</code>.</p>

<p>Một số nguyên <code>y</code> được gọi là <strong>lũy thừa bậc k<sup>th</sup> hoàn hảo</strong> nếu tồn tại một số nguyên <code>x</code> sao cho <code>y = x<sup>k</sup></code>.</p>

<p>Hãy trả về số lượng số nguyên <code>y</code> trong đoạn <code>[l, r]</code> (tính cả hai đầu mút) là <strong>lũy thừa bậc k<sup>th</sup> hoàn hảo</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 1, r = 9, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>
Các lập phương hoàn hảo trong đoạn <code>[1, 9]</code> là:

<ul>
	<li><code>1 = 1<sup>3</sup></code></li>
	<li><code>8 = 2<sup>3</sup></code></li>
</ul>
Do đó, đáp án là 2.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 8, r = 30, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>
Các bình phương hoàn hảo trong đoạn <code>[8, 30]</code> là:

<ul>
	<li><code>9 = 3<sup>2</sup></code></li>
	<li><code>16 = 4<sup>2</sup></code></li>
	<li><code>25 = 5<sup>2</sup></code></li>
</ul>
Do đó, đáp án là 3.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= l &lt;= r &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $r\le 10^9$, ta không thể duyệt mọi số nguyên trong đoạn. Các lũy thừa bậc $k$ hoàn hảo là các giá trị $x^k$ thỏa mãn $l\le x^k\le r$.
>
> Khi $k=1$, đáp án là độ dài của đoạn. Nếu không, $x$ nhiều nhất xấp xỉ $r^{1/k}$; ta tính $y=x^k$ với $x=0,1,2,\ldots$, dừng khi vượt quá $r$ và đếm các giá trị nằm trong $[l,r]$.
>
> Số lần liệt kê giảm nhanh khi $k$ tăng, nên cách này đáp ứng được $k\le 30$.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem $k$ có bằng 1 hay không. Nếu có, số lượng lũy thừa bậc 1 hoàn hảo trong đoạn chính là số lượng số nguyên trong đoạn, bằng $r - l + 1$.

Nếu không, ta liệt kê các số nguyên $x$ và tính $y = x^k$. Nếu $y$ lớn hơn $r$, ta dừng việc liệt kê. Nếu $y$ nằm trong đoạn $[l, r]$, ta tăng đáp án lên 1.

Độ phức tạp thời gian là $O(r^{1/k} \cdot k)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKthRoots(self, l: int, r: int, k: int) -> int:
        if k == 1:
            return r - l + 1
        ans = 0
        for x in count():
            y = x**k
            if y > r:
                break
            if l <= y <= r:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countKthRoots(int l, int r, int k) {
        if (k == 1) {
            return r - l + 1;
        }
        int ans = 0;
        for (int x = 0;; x++) {
            long y = 1;
            for (int i = 0; i < k; i++) {
                y *= x;
                if (y > r) {
                    break;
                }
            }
            if (y > r) {
                break;
            }
            if (l <= y && y <= r) {
                ans++;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKthRoots(int l, int r, int k) {
        if (k == 1) {
            return r - l + 1;
        }
        int ans = 0;
        for (int x = 0;; x++) {
            long long y = 1;
            for (int i = 0; i < k; i++) {
                y *= x;
                if (y > r) {
                    break;
                }
            }
            if (y > r) {
                break;
            }
            if (l <= y && y <= r) {
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countKthRoots(l int, r int, k int) int {
	if k == 1 {
		return r - l + 1
	}
	ans := 0
	for x := 0; ; x++ {
		y := 1
		for i := 0; i < k; i++ {
			y *= x
			if y > r {
				break
			}
		}
		if y > r {
			break
		}
		if l <= y && y <= r {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countKthRoots(l: number, r: number, k: number): number {
    if (k === 1) {
        return r - l + 1;
    }
    let ans = 0;
    for (let x = 0; ; x++) {
        let y = 1;
        for (let i = 0; i < k; i++) {
            y *= x;
            if (y > r) {
                break;
            }
        }
        if (y > r) {
            break;
        }
        if (l <= y && y <= r) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
