---
comments: true
difficulty: Easy
rating: 1307
source: Biweekly Contest 62 Q1
tags:
    - Array
    - Matrix
    - Simulation
---

<!-- problem:start -->

# [2022. Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array)

[中文文档](/solution/2000-2099/2022.Convert%201D%20Array%20Into%202D%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên một chiều (1D) được đánh chỉ số từ <strong>0</strong> là <code>original</code>, cùng hai số nguyên, <code>m</code> và <code>n</code>. Nhiệm vụ của bạn là tạo một mảng hai chiều (2D) có <code> m</code> hàng và <code>n</code> cột bằng cách sử dụng <strong>tất cả</strong> các phần tử của <code>original</code>.</p>

<p>Các phần tử tại các chỉ số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm</strong>) của <code>original</code> sẽ tạo thành hàng đầu tiên của mảng 2D được xây dựng, các phần tử tại các chỉ số từ <code>n</code> đến <code>2 * n - 1</code> (<strong>bao gồm</strong>) sẽ tạo thành hàng thứ hai của mảng 2D được xây dựng, và tiếp tục như vậy.</p>

<p>Trả về <em>mảng 2D kích thước </em><code>m x n</code><em> được xây dựng theo quy trình trên, hoặc một mảng 2D rỗng nếu không thể thực hiện</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2022.Convert%201D%20Array%20Into%202D%20Array/images/image-20210826114243-1.png" style="width: 500px; height: 174px;" />
<pre>
<strong>Đầu vào:</strong> original = [1,2,3,4], m = 2, n = 2
<strong>Đầu ra:</strong> [[1,2],[3,4]]
<strong>Giải thích:</strong> Mảng 2D được xây dựng sẽ có 2 hàng và 2 cột.
Nhóm n=2 phần tử đầu tiên trong original, [1,2], trở thành hàng đầu tiên của mảng 2D được xây dựng.
Nhóm n=2 phần tử thứ hai trong original, [3,4], trở thành hàng thứ hai của mảng 2D được xây dựng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> original = [1,2,3], m = 1, n = 3
<strong>Đầu ra:</strong> [[1,2,3]]
<strong>Giải thích:</strong> Mảng 2D được xây dựng sẽ có 1 hàng và 3 cột.
Đặt cả ba phần tử trong original vào hàng đầu tiên của mảng 2D được xây dựng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> original = [1,2], m = 1, n = 1
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> original có 2 phần tử.
Không thể đặt 2 phần tử vào mảng 2D kích thước 1x1, vì vậy trả về một mảng 2D rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= original.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= original[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m, n &lt;= 4 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Việc reshape chỉ khả thi khi $mn$ bằng độ dài mảng nguồn. Vì độ dài $\le 5 \times 10^4$, chỉ cần chia mảng thành các đoạn theo từng hàng.
>
> Trả về mảng rỗng nếu không khớp; ngược lại, lấy các đoạn có độ rộng $n$.

<!-- thinking:end -->

Theo mô tả bài toán, để xây dựng một mảng hai chiều gồm $m$ hàng và $n$ cột, cần thỏa mãn $m \times n$ bằng độ dài của mảng ban đầu. Nếu không thỏa mãn, trả về ngay một mảng rỗng.

Nếu thỏa mãn, ta thực hiện theo quy trình được mô tả trong đề bài và đưa các phần tử của mảng ban đầu vào mảng hai chiều theo đúng thứ tự.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của mảng hai chiều. Không tính phần bộ nhớ dùng cho kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def construct2DArray(self, original: List[int], m: int, n: int) -> List[List[int]]:
        if m * n != len(original):
            return []
        return [original[i : i + n] for i in range(0, m * n, n)]
```

#### Java

```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        if (m * n != original.length) {
            return new int[0][0];
        }
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = original[i * n + j];
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
    vector<vector<int>> construct2DArray(vector<int>& original, int m, int n) {
        if (m * n != original.size()) {
            return {};
        }
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[i][j] = original[i * n + j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func construct2DArray(original []int, m int, n int) (ans [][]int) {
	if m*n != len(original) {
		return [][]int{}
	}
	for i := 0; i < m*n; i += n {
		ans = append(ans, original[i:i+n])
	}
	return
}
```

#### TypeScript

```ts
function construct2DArray(original: number[], m: number, n: number): number[][] {
    if (m * n != original.length) {
        return [];
    }
    const ans: number[][] = [];
    for (let i = 0; i < m * n; i += n) {
        ans.push(original.slice(i, i + n));
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} original
 * @param {number} m
 * @param {number} n
 * @return {number[][]}
 */
var construct2DArray = function (original, m, n) {
    if (m * n != original.length) {
        return [];
    }
    const ans = [];
    for (let i = 0; i < m * n; i += n) {
        ans.push(original.slice(i, i + n));
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
