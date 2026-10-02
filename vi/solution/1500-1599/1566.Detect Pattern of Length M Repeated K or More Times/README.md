---
comments: true
difficulty: Easy
rating: 1486
source: Weekly Contest 204 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [1566. Detect Pattern of Length M Repeated K or More Times](https://leetcode.com/problems/detect-pattern-of-length-m-repeated-k-or-more-times)

[中文文档](/solution/1500-1599/1566.Detect%20Pattern%20of%20Length%20M%20Repeated%20K%20or%20More%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>arr</code>, hãy tìm một pattern độ dài <code>m</code> được lặp lại ít nhất <code>k</code> lần.</p>

<p>Một <strong>pattern</strong> là một mảng con (dãy con liên tiếp) gồm một hoặc nhiều giá trị, được lặp lại nhiều lần <strong>liên tiếp </strong> và không chồng lấn. Pattern được xác định bởi độ dài và số lần lặp.</p>

<p>Trả về <code>true</code> <em>nếu tồn tại pattern độ dài</em> <code>m</code> <em>được lặp lại</em> <code>k</code> <em>lần trở lên, ngược lại trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,4,4,4,4], m = 1, k = 3
<strong>Output:</strong> true
<strong>Giải thích: </strong>Pattern <strong>(4)</strong> độ dài 1 được lặp lại 4 lần liên tiếp. Pattern có thể được lặp lại k lần hoặc nhiều hơn, nhưng không được ít hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,1,2,1,1,1,3], m = 2, k = 2
<strong>Output:</strong> true
<strong>Giải thích: </strong>Pattern <strong>(1,2)</strong> độ dài 2 được lặp lại 2 lần liên tiếp. Pattern hợp lệ khác là <strong>(2,1)</strong> cũng được lặp lại 2 lần.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,1,2,1,3], m = 2, k = 3
<strong>Output:</strong> false
<strong>Giải thích: </strong>Pattern (1,2) có độ dài 2 nhưng chỉ được lặp lại 2 lần. Không có pattern độ dài 2 nào được lặp lại từ 3 lần trở lên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 100</code></li>
	<li><code>1 &lt;= m &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Cần xác định pattern độ dài $m$ có lặp lại ít nhất $k$ lần hay không. $n$ nhỏ nên thử mọi vị trí bắt đầu cũng được, nhưng chỉ cần một lần quét.
>
> Lặp $k$ lần nghĩa là $(k-1)m$ chỉ số liên tiếp thỏa $a_i=a_{i-m}$. Từ chỉ số $m$, tích lũy số lần bằng nhau và thành công khi đạt mục tiêu; nếu khác nhau thì đặt lại bộ đếm.

<!-- thinking:end -->

Trước hết, nếu độ dài mảng nhỏ hơn $m \times k$ thì chắc chắn không có pattern độ dài $m$ lặp ít nhất $k$ lần, nên trả về trực tiếp $\textit{false}$.

Tiếp theo, định nghĩa biến $\textit{cnt}$ để ghi nhận số lần lặp liên tiếp hiện tại. Nếu có $(k - 1) \times m$ phần tử liên tiếp $a_i$ thỏa $a_i = a_{i - m}$, ta đã tìm thấy pattern độ dài $m$ lặp ít nhất $k$ lần và trả về $\textit{true}$. Ngược lại, đặt lại $\textit{cnt}$ về $0$ và tiếp tục duyệt mảng.

Cuối cùng, nếu duyệt hết mảng mà không tìm thấy pattern thỏa điều kiện, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def containsPattern(self, arr: List[int], m: int, k: int) -> bool:
        if len(arr) < m * k:
            return False
        cnt, target = 0, (k - 1) * m
        for i in range(m, len(arr)):
            if arr[i] == arr[i - m]:
                cnt += 1
                if cnt == target:
                    return True
            else:
                cnt = 0
        return False
```

#### Java

```java
class Solution {
    public boolean containsPattern(int[] arr, int m, int k) {
        if (arr.length < m * k) {
            return false;
        }
        int cnt = 0, target = (k - 1) * m;
        for (int i = m; i < arr.length; ++i) {
            if (arr[i] == arr[i - m]) {
                if (++cnt == target) {
                    return true;
                }
            } else {
                cnt = 0;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool containsPattern(vector<int>& arr, int m, int k) {
        if (arr.size() < m * k) {
            return false;
        }
        int cnt = 0, target = (k - 1) * m;
        for (int i = m; i < arr.size(); ++i) {
            if (arr[i] == arr[i - m]) {
                if (++cnt == target) {
                    return true;
                }
            } else {
                cnt = 0;
            }
        }
        return false;
    }
};
```

#### Go

```go
func containsPattern(arr []int, m int, k int) bool {
	cnt, target := 0, (k-1)*m
	for i := m; i < len(arr); i++ {
		if arr[i] == arr[i-m] {
			cnt++
			if cnt == target {
				return true
			}
		} else {
			cnt = 0
		}
	}
	return false
}
```

#### TypeScript

```ts
function containsPattern(arr: number[], m: number, k: number): boolean {
    if (arr.length < m * k) {
        return false;
    }
    const target = (k - 1) * m;
    let cnt = 0;
    for (let i = m; i < arr.length; ++i) {
        if (arr[i] === arr[i - m]) {
            if (++cnt === target) {
                return true;
            }
        } else {
            cnt = 0;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
