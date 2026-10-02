---
comments: true
difficulty: Hard
rating: 2171
source: Weekly Contest 219 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1691. Maximum Height by Stacking Cuboids](https://leetcode.com/problems/maximum-height-by-stacking-cuboids)

[中文文档](/solution/1600-1699/1691.Maximum%20Height%20by%20Stacking%20Cuboids/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> <code>cuboids</code>, trong đó kích thước của khối hộp thứ <code>i<sup>th</sup></code> là <code>cuboids[i] = [width<sub>i</sub>, length<sub>i</sub>, height<sub>i</sub>]</code> (<strong>được đánh chỉ số từ 0</strong>). Hãy chọn một <strong>tập con</strong> của <code>cuboids</code> và xếp chúng lên nhau.</p>

<p>Bạn có thể đặt khối hộp <code>i</code> lên khối hộp <code>j</code> nếu <code>width<sub>i</sub> &lt;= width<sub>j</sub></code>, <code>length<sub>i</sub> &lt;= length<sub>j</sub></code> và <code>height<sub>i</sub> &lt;= height<sub>j</sub></code>. Bạn có thể xoay khối hộp để sắp xếp lại các kích thước trước khi đặt nó lên khối hộp khác.</p>

<p>Trả về <em><strong>chiều cao lớn nhất</strong> của chồng</em> <code>cuboids</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1691.Maximum%20Height%20by%20Stacking%20Cuboids/images/image.jpg" style="width: 420px; height: 299px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> cuboids = [[50,45,20],[95,37,53],[45,23,12]]
<strong>Đầu ra:</strong> 190
<strong>Giải thích:</strong>
Khối hộp 1 được đặt ở dưới cùng, với mặt 53x37 hướng xuống và chiều cao là 95.
Khối hộp 0 được đặt tiếp theo, với mặt 45x20 hướng xuống và chiều cao là 50.
Khối hộp 2 được đặt tiếp theo, với mặt 23x12 hướng xuống và chiều cao là 45.
Tổng chiều cao là 95 + 50 + 45 = 190.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cuboids = [[38,25,45],[76,35,3]]
<strong>Đầu ra:</strong> 76
<strong>Giải thích:</strong>
Bạn không thể đặt khối hộp nào lên khối hộp còn lại.
Ta chọn khối hộp 1 và xoay nó để mặt 35x3 hướng xuống, khi đó chiều cao là 76.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> cuboids = [[7,11,17],[7,17,11],[11,7,17],[11,17,7],[17,7,11],[17,11,7]]
<strong>Đầu ra:</strong> 102
<strong>Giải thích:</strong>
Sau khi sắp xếp lại các khối hộp, ta thấy tất cả đều có cùng kích thước.
Ta có thể đặt mặt 11x7 hướng xuống ở tất cả các khối hộp, nên chiều cao của mỗi khối là 17.
Chiều cao lớn nhất của chồng khối hộp là 6 * 17 = 102.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == cuboids.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= width<sub>i</sub>, length<sub>i</sub>, height<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các khối hộp có thể được xoay và phải có từng kích thước không lớn hơn khối bên dưới. Sắp xếp mỗi bộ ba thành length $\le$ width $\le$ height vẫn bảo toàn tính hợp lệ và tối đa hóa chiều cao; sau đó sắp xếp các khối hộp.
>
> $n$ nhỏ. $f[i]$ là chiều cao tốt nhất khi $i$ ở dưới cùng; thử mọi $j$ có width và height phù hợp, rồi đặt $f[i]=\max f[j]+h_i$.

<!-- thinking:end -->

Theo mô tả bài toán, khối hộp $j$ có thể được đặt lên khối hộp $i$ khi và chỉ khi "length, width, and height" của khối hộp $j$ lần lượt không lớn hơn "length, width, and height" của khối hộp $i$.

Bài toán này cho phép xoay các khối hộp, nghĩa là ta có thể chọn bất kỳ cạnh nào làm "height". Với mọi cách xếp hợp lệ, nếu xoay mỗi khối hộp thành "length <= width <= height", cách xếp vẫn hợp lệ và bảo đảm chiều cao lớn nhất.

Do đó, ta có thể sắp xếp các cạnh của mỗi khối hộp để mỗi khối thỏa mãn "length <= width <= height". Sau đó ta sắp xếp các khối hộp theo thứ tự tăng dần.

Tiếp theo, ta có thể sử dụng quy hoạch động để giải bài toán này.

Ta định nghĩa $f[i]$ là chiều cao lớn nhất khi khối hộp $i$ ở dưới cùng. Ta có thể duyệt từng khối hộp $j$ nằm trên khối hộp $i$, trong đó $0 \leq j < i$. Nếu $j$ có thể đặt lên $i$, ta có công thức chuyển trạng thái:

$$
f[i] = \max_{0 \leq j < i} \{f[j] + h[i]\}
$$

trong đó $h[i]$ biểu diễn chiều cao của khối hộp $i$.

Đáp án cuối cùng là giá trị lớn nhất của $f[i]$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng khối hộp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxHeight(self, cuboids: List[List[int]]) -> int:
        for c in cuboids:
            c.sort()
        cuboids.sort()
        n = len(cuboids)
        f = [0] * n
        for i in range(n):
            for j in range(i):
                if cuboids[j][1] <= cuboids[i][1] and cuboids[j][2] <= cuboids[i][2]:
                    f[i] = max(f[i], f[j])
            f[i] += cuboids[i][2]
        return max(f)
```

#### Java

```java
class Solution {
    public int maxHeight(int[][] cuboids) {
        for (var c : cuboids) {
            Arrays.sort(c);
        }
        Arrays.sort(cuboids,
            (a, b) -> a[0] == b[0] ? (a[1] == b[1] ? a[2] - b[2] : a[1] - b[1]) : a[0] - b[0]);
        int n = cuboids.length;
        int[] f = new int[n];
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2]) {
                    f[i] = Math.max(f[i], f[j]);
                }
            }
            f[i] += cuboids[i][2];
        }
        return Arrays.stream(f).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxHeight(vector<vector<int>>& cuboids) {
        for (auto& c : cuboids) {
            sort(c.begin(), c.end());
        }
        sort(cuboids.begin(), cuboids.end());
        int n = cuboids.size();
        vector<int> f(n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2]) {
                    f[i] = max(f[i], f[j]);
                }
            }
            f[i] += cuboids[i][2];
        }
        return *max_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func maxHeight(cuboids [][]int) int {
	for _, c := range cuboids {
		sort.Ints(c)
	}
	sort.Slice(cuboids, func(i, j int) bool {
		a, b := cuboids[i], cuboids[j]
		return a[0] < b[0] || a[0] == b[0] && (a[1] < b[1] || a[1] == b[1] && a[2] < b[2])
	})
	n := len(cuboids)
	f := make([]int, n)
	for i := range f {
		for j := 0; j < i; j++ {
			if cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2] {
				f[i] = max(f[i], f[j])
			}
		}
		f[i] += cuboids[i][2]
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function maxHeight(cuboids: number[][]): number {
    for (const c of cuboids) {
        c.sort((a, b) => a - b);
    }
    cuboids.sort((a, b) => {
        if (a[0] !== b[0]) {
            return a[0] - b[0];
        }
        if (a[1] !== b[1]) {
            return a[1] - b[1];
        }
        return a[2] - b[2];
    });
    const n = cuboids.length;
    const f = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            const ok = cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2];
            if (ok) f[i] = Math.max(f[i], f[j]);
        }
        f[i] += cuboids[i][2];
    }
    return Math.max(...f);
}
```

#### JavaScript

```js
/**
 * @param {number[][]} cuboids
 * @return {number}
 */
var maxHeight = function (cuboids) {
    for (const c of cuboids) {
        c.sort((a, b) => a - b);
    }
    cuboids.sort((a, b) => {
        if (a[0] !== b[0]) {
            return a[0] - b[0];
        }
        if (a[1] !== b[1]) {
            return a[1] - b[1];
        }
        return a[2] - b[2];
    });
    const n = cuboids.length;
    const f = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            const ok = cuboids[j][1] <= cuboids[i][1] && cuboids[j][2] <= cuboids[i][2];
            if (ok) f[i] = Math.max(f[i], f[j]);
        }
        f[i] += cuboids[i][2];
    }
    return Math.max(...f);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
