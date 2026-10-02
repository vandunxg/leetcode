---
comments: true
difficulty: Hard
rating: 2366
source: Biweekly Contest 8 Q4
tags:
    - Greedy
    - Math
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1183. Maximum Number of Ones 🔒](https://leetcode.com/problems/maximum-number-of-ones)

[中文文档](/solution/1100-1199/1183.Maximum%20Number%20of%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Xét ma trận <code>M</code> có kích thước <code>width * height</code>, mỗi ô có giá trị <code>0</code> hoặc <code>1</code>, và mọi ma trận con <strong>hình vuông</strong> của <code>M</code> có kích thước <code>sideLength * sideLength</code> chứa nhiều nhất <code>maxOnes</code> số 1.</p>

<p>Trả về số lượng số 1 lớn nhất có thể có trong ma trận <code>M</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> width = 3, height = 3, sideLength = 2, maxOnes = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Trong ma trận 3*3, không ma trận con 2*2 nào có thể chứa nhiều hơn 1 số 1.
Cách sắp xếp tốt nhất có 4 số 1 là:
[1,0,1]
[0,0,0]
[1,0,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> width = 3, height = 3, sideLength = 2, maxOnes = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
[1,0,1]
[1,0,1]
[1,0,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= width, height &lt;= 100</code></li>
	<li><code>1 &lt;= sideLength &lt;= width, height</code></li>
	<li><code>0 &lt;= maxOnes &lt;= sideLength * sideLength</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm các vị trí tương đương

<!-- thinking:start -->

> **Tư duy**
>
> Ma trận được lặp theo một mẫu kích thước $sideLength\times sideLength$, trong đó có nhiều nhất $maxOnes$ số 1. Ô $(i,j)$ tương ứng với $(i\bmod x,j\bmod x)$; tần suất xuất hiện của vị trí dư này cho biết lợi ích khi đặt số 1 tại đó. Đếm tần suất của $x^2$ vị trí dư rồi cộng $maxOnes$ giá trị lớn nhất.

<!-- thinking:end -->

Để tiện trình bày, ký hiệu $x = sideLength$.

Xét một hình vuông kích thước $x \times x$, ta cần chọn tối đa $maxOnes$ điểm bên trong và đặt chúng thành 1. Lưu ý rằng khi chọn điểm có tọa độ $(i, j)$, ta cũng có thể đặt 1 tại mọi điểm có tọa độ $(i\pm k_1 \times x, j\pm k_2 \times x)$. Vì vậy, ta đếm số vị trí tương đương của tọa độ $(i, j)$ trong ma trận, rồi chọn $maxOnes$ vị trí có số lượng lớn nhất.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumNumberOfOnes(
        self, width: int, height: int, sideLength: int, maxOnes: int
    ) -> int:
        x = sideLength
        cnt = [0] * (x * x)
        for i in range(width):
            for j in range(height):
                k = (i % x) * x + (j % x)
                cnt[k] += 1
        cnt.sort(reverse=True)
        return sum(cnt[:maxOnes])
```

#### Java

```java
class Solution {
    public int maximumNumberOfOnes(int width, int height, int sideLength, int maxOnes) {
        int x = sideLength;
        int[] cnt = new int[x * x];
        for (int i = 0; i < width; ++i) {
            for (int j = 0; j < height; ++j) {
                int k = (i % x) * x + (j % x);
                ++cnt[k];
            }
        }
        Arrays.sort(cnt);
        int ans = 0;
        for (int i = 0; i < maxOnes; ++i) {
            ans += cnt[cnt.length - i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumNumberOfOnes(int width, int height, int sideLength, int maxOnes) {
        int x = sideLength;
        vector<int> cnt(x * x);
        for (int i = 0; i < width; ++i) {
            for (int j = 0; j < height; ++j) {
                int k = (i % x) * x + (j % x);
                ++cnt[k];
            }
        }
        sort(cnt.rbegin(), cnt.rend());
        int ans = 0;
        for (int i = 0; i < maxOnes; ++i) {
            ans += cnt[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maximumNumberOfOnes(width int, height int, sideLength int, maxOnes int) int {
	x := sideLength
	cnt := make([]int, x*x)
	for i := 0; i < width; i++ {
		for j := 0; j < height; j++ {
			k := (i%x)*x + (j % x)
			cnt[k]++
		}
	}
	sort.Ints(cnt)
	ans := 0
	for i := range cnt[:maxOnes] {
		ans += cnt[len(cnt)-i-1]
	}
	return ans
}
```

#### JavaScript

```js
/**
 * @param {number} width
 * @param {number} height
 * @param {number} sideLength
 * @param {number} maxOnes
 * @return {number}
 */
var maximumNumberOfOnes = function (width, height, sideLength, maxOnes) {
    const x = sideLength;
    const cnt = new Array(x * x).fill(0);
    for (let i = 0; i < width; ++i) {
        for (let j = 0; j < height; ++j) {
            const k = (i % x) * x + (j % x);
            ++cnt[k];
        }
    }
    cnt.sort((a, b) => b - a);
    return cnt.slice(0, maxOnes).reduce((a, b) => a + b, 0);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
