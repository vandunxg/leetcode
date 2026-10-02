---
comments: true
difficulty: Medium
rating: 2059
source: Weekly Contest 146 Q4
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1131. Maximum of Absolute Value Expression](https://leetcode.com/problems/maximum-of-absolute-value-expression)

[中文文档](/solution/1100-1199/1131.Maximum%20of%20Absolute%20Value%20Expression/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên có cùng độ dài. Hãy trả về giá trị lớn nhất của biểu thức:</p>

<p><code>|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|</code></p>

<p>trong đó lấy giá trị lớn nhất trên mọi cặp <code>0 &lt;= i, j &lt; arr1.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,2,3,4], arr2 = [-1,4,5,6]
<strong>Đầu ra:</strong> 13
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,-2,-5,0,10], arr2 = [0,-2,-1,-7,-4]
<strong>Đầu ra:</strong> 20
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr1.length == arr2.length &lt;= 40000</code></li>
	<li><code>-10^6 &lt;= arr1[i], arr2[i] &lt;= 10^6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học và duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Hai lớp giá trị tuyệt đối khiến cách duyệt hai vòng lặp có độ phức tạp $O(n^2)$. Khai triển $|x_i-x_j|+|y_i-y_j|+|i-j|$ cho ta hiệu giữa giá trị lớn nhất và nhỏ nhất của $ax+by+\textit{index}$ với bốn cặp dấu.
>
> Với mỗi $a,b\in\{\pm 1\}$, duyệt một lượt để tìm giá trị lớn nhất và nhỏ nhất. Đáp án là khoảng chênh lệch lớn nhất trong các trường hợp đó, tính được trong thời gian tuyến tính.

<!-- thinking:end -->

Gọi $x_i = arr1[i]$, $y_i = arr2[i]$. Vì đổi vị trí $i$ và $j$ không làm thay đổi giá trị biểu thức, ta có thể giả sử $i \ge j$. Khi đó, biểu thức được biến đổi thành:

$$
| x_i - x_j | + | y_i - y_j | + i - j = \max \begin{cases} (x_i + y_i) - (x_j + y_j) \\ (x_i - y_i) - (x_j - y_j) \\ (-x_i + y_i) - (-x_j + y_j) \\ (-x_i - y_i) - (-x_j - y_j) \end{cases} + i - j\\
= \max \begin{cases} (x_i + y_i + i) - (x_j + y_j + j) \\ (x_i - y_i + i) - (x_j - y_j + j) \\ (-x_i + y_i + i) - (-x_j + y_j + j) \\ (-x_i - y_i + i) - (-x_j - y_j + j) \end{cases}
$$

Vì vậy, ta chỉ cần tìm giá trị lớn nhất $mx$ và nhỏ nhất $mi$ của $a \times x_i + b \times y_i + i$, với $a, b \in \{-1, 1\}$. Đáp án là giá trị lớn nhất trong các hiệu $mx - mi$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [1330. Reverse Subarray To Maximize Array Value](https://github.com/doocs/leetcode/blob/main/solution/1300-1399/1330.Reverse%20Subarray%20To%20Maximize%20Array%20Value/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAbsValExpr(self, arr1: List[int], arr2: List[int]) -> int:
        dirs = (1, -1, -1, 1, 1)
        ans = -inf
        for a, b in pairwise(dirs):
            mx, mi = -inf, inf
            for i, (x, y) in enumerate(zip(arr1, arr2)):
                mx = max(mx, a * x + b * y + i)
                mi = min(mi, a * x + b * y + i)
                ans = max(ans, mx - mi)
        return ans
```

#### Java

```java
class Solution {
    public int maxAbsValExpr(int[] arr1, int[] arr2) {
        int[] dirs = {1, -1, -1, 1, 1};
        final int inf = 1 << 30;
        int ans = -inf;
        int n = arr1.length;
        for (int k = 0; k < 4; ++k) {
            int a = dirs[k], b = dirs[k + 1];
            int mx = -inf, mi = inf;
            for (int i = 0; i < n; ++i) {
                mx = Math.max(mx, a * arr1[i] + b * arr2[i] + i);
                mi = Math.min(mi, a * arr1[i] + b * arr2[i] + i);
                ans = Math.max(ans, mx - mi);
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
    int maxAbsValExpr(vector<int>& arr1, vector<int>& arr2) {
        int dirs[5] = {1, -1, -1, 1, 1};
        const int inf = 1 << 30;
        int ans = -inf;
        int n = arr1.size();
        for (int k = 0; k < 4; ++k) {
            int a = dirs[k], b = dirs[k + 1];
            int mx = -inf, mi = inf;
            for (int i = 0; i < n; ++i) {
                mx = max(mx, a * arr1[i] + b * arr2[i] + i);
                mi = min(mi, a * arr1[i] + b * arr2[i] + i);
                ans = max(ans, mx - mi);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxAbsValExpr(arr1 []int, arr2 []int) int {
	dirs := [5]int{1, -1, -1, 1, 1}
	const inf = 1 << 30
	ans := -inf
	for k := 0; k < 4; k++ {
		a, b := dirs[k], dirs[k+1]
		mx, mi := -inf, inf
		for i, x := range arr1 {
			y := arr2[i]
			mx = max(mx, a*x+b*y+i)
			mi = min(mi, a*x+b*y+i)
			ans = max(ans, mx-mi)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxAbsValExpr(arr1: number[], arr2: number[]): number {
    const dirs = [1, -1, -1, 1, 1];
    const inf = 1 << 30;
    let ans = -inf;
    for (let k = 0; k < 4; ++k) {
        const [a, b] = [dirs[k], dirs[k + 1]];
        let mx = -inf;
        let mi = inf;
        for (let i = 0; i < arr1.length; ++i) {
            const [x, y] = [arr1[i], arr2[i]];
            mx = Math.max(mx, a * x + b * y + i);
            mi = Math.min(mi, a * x + b * y + i);
            ans = Math.max(ans, mx - mi);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
