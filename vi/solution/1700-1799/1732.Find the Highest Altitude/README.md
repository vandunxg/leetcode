---
comments: true
difficulty: Easy
rating: 1256
source: Biweekly Contest 44 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1732. Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude)

[中文文档](/solution/1700-1799/1732.Find%20the%20Highest%20Altitude/README.md)

## Mô tả

<!-- description:start -->

<p>Có một người đi xe đạp thực hiện chuyến đi trên đường. Chuyến đi gồm <code>n + 1</code> điểm ở các độ cao khác nhau. Người đó bắt đầu tại điểm <code>0</code> với độ cao bằng <code>0</code>.</p>

<p>Bạn được cho mảng số nguyên <code>gain</code> có độ dài <code>n</code>, trong đó <code>gain[i]</code> là <strong>mức thay đổi độ cao</strong> giữa điểm <code>i</code>​​​​​​ và <code>i + 1</code> với mọi (<code>0 &lt;= i &lt; n)</code>. Hãy trả về <em><strong>độ cao lớn nhất</strong> của một điểm.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> gain = [-5,1,5,0,-7]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Các độ cao là [0,-5,-4,1,1,-6]. Độ cao lớn nhất là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> gain = [-4,-3,-2,-1,4,3,2]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Các độ cao là [0,-4,-7,-9,-10,-6,-3,-1]. Độ cao lớn nhất là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == gain.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>-100 &lt;= gain[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố (Mảng hiệu)

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{gain}$ lưu các hiệu độ cao liên tiếp, bắt đầu từ $0$. Điểm cao nhất là tổng tiền tố lớn nhất của mảng hiệu đó.
>
> Cộng dồn $\textit{gain}$ từ $0$ và duy trì giá trị lớn nhất hiện tại.

<!-- thinking:end -->

Gọi độ cao của mỗi điểm là $h_i$. Vì $gain[i]$ biểu diễn hiệu độ cao giữa điểm thứ $i$ và điểm thứ $(i + 1)$, ta có $gain[i] = h_{i + 1} - h_i$. Do đó:

$$
\sum_{i = 0}^{n-1} gain[i] = h_1 - h_0 + h_2 - h_1 + \cdots + h_n - h_{n - 1} = h_n - h_0 = h_n
$$

suy ra:

$$
h_{i+1} = \sum_{j = 0}^{i} gain[j]
$$

Ta thấy độ cao của mỗi điểm có thể được tính bằng tổng tiền tố. Vì vậy, ta chỉ cần duyệt mảng một lần và tìm giá trị lớn nhất của tổng tiền tố, đó chính là độ cao lớn nhất.

> Thực tế, mảng $gain$ trong đề bài là một mảng hiệu. Tổng tiền tố của mảng hiệu cho ta mảng độ cao ban đầu, sau đó tìm giá trị lớn nhất của mảng độ cao đó.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài của mảng `gain`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        return max(accumulate(gain, initial=0))
```

#### Java

```java
class Solution {
    public int largestAltitude(int[] gain) {
        int ans = 0, h = 0;
        for (int v : gain) {
            h += v;
            ans = Math.max(ans, h);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int ans = 0, h = 0;
        for (int v : gain) h += v, ans = max(ans, h);
        return ans;
    }
};
```

#### Go

```go
func largestAltitude(gain []int) (ans int) {
	h := 0
	for _, v := range gain {
		h += v
		if ans < h {
			ans = h
		}
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_altitude(gain: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut h = 0;
        for v in gain.iter() {
            h += v;
            ans = ans.max(h);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} gain
 * @return {number}
 */
var largestAltitude = function (gain) {
    let ans = 0;
    let h = 0;
    for (const v of gain) {
        h += v;
        ans = Math.max(ans, h);
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $gain
     * @return Integer
     */
    function largestAltitude($gain) {
        $max = 0;
        for ($i = 0; $i < count($gain); $i++) {
            $tmp += $gain[$i];
            if ($tmp > $max) {
                $max = $tmp;
            }
        }
        return $max;
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int largestAltitude(int* gain, int gainSize) {
    int ans = 0;
    int h = 0;
    for (int i = 0; i < gainSize; i++) {
        h += gain[i];
        ans = max(ans, h);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
