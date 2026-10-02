---
comments: true
difficulty: Hard
rating: 1961
source: Weekly Contest 200 Q4
tags:
    - Greedy
    - Array
    - Two Pointers
    - Dynamic Programming
---

<!-- problem:start -->

# [1537. Get the Maximum Score](https://leetcode.com/problems/get-the-maximum-score)

[中文文档](/solution/1500-1599/1537.Get%20the%20Maximum%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>đã sắp xếp</strong> và không trùng nhau <code>nums1</code> và <code>nums2</code>.</p>

<p>Một <strong>đường đi<strong><em> </em></strong>hợp lệ</strong> được định nghĩa như sau:</p>

<ul>
	<li>Choose array <code>nums1</code> or <code>nums2</code> to traverse (from index-0).</li>
	<li>Traverse the current array from left to right.</li>
	<li>If you are reading any value that is present in <code>nums1</code> and <code>nums2</code> you are allowed to change your path to the other array. (Only one repeated value is considered in the valid path).</li>
</ul>

<p><strong>Điểm</strong> là tổng các giá trị khác nhau trên một đường đi hợp lệ.</p>

<p>Trả về <em>điểm lớn nhất có thể đạt được trong mọi <strong>đường đi hợp lệ</strong></em>. Vì đáp án có thể rất lớn, trả về phần dư khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1537.Get%20the%20Maximum%20Score/images/sample_1_1893.png" style="width: 500px; height: 151px;" />
<pre>
<strong>Input:</strong> nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]
<strong>Output:</strong> 30
<strong>Explanation:</strong> Valid paths:
[2,4,5,8,10], [2,4,5,8,9], [2,4,6,8,9], [2,4,6,8,10],  (starting from nums1)
[4,6,8,9], [4,5,8,10], [4,5,8,9], [4,6,8,10]    (starting from nums2)
The maximum is obtained with the path in green <strong>[2,4,6,8,10]</strong>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,3,5,7,9], nums2 = [3,5,100]
<strong>Output:</strong> 109
<strong>Explanation:</strong> Maximum sum is obtained with the path <strong>[1,3,5,100]</strong>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,2,3,4,5], nums2 = [6,7,8,9,10]
<strong>Output:</strong> 40
<strong>Explanation:</strong> There are no common elements between nums1 and nums2.
Maximum sum is obtained with the path [6,7,8,9,10].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>7</sup></code></li>
	<li><code>nums1</code> and <code>nums2</code> are strictly increasing.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai mảng tăng dần có thể chuyển tại các giá trị bằng nhau; điểm là tổng các số đã đi qua, trong đó giá trị chung chỉ tính một lần. $n\le 10^5$, nên ta không thể liệt kê các chuỗi chuyển.
>
> Các giá trị chung chia đường đi thành các đoạn, và giữa hai giá trị chung ta phải đi theo đúng một mảng. Hai con trỏ tiến cùng nhau: phía có phần tử đầu nhỏ hơn cộng nó vào tổng riêng; khi bằng nhau, cả hai cộng giá trị đó và bắt đầu lại từ tổng tốt hơn hiện tại. Đáp án là giá trị lớn hơn trong hai tổng cuối.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums1: List[int], nums2: List[int]) -> int:
        mod = 10**9 + 7
        m, n = len(nums1), len(nums2)
        i = j = 0
        f = g = 0
        while i < m or j < n:
            if i == m:
                g += nums2[j]
                j += 1
            elif j == n:
                f += nums1[i]
                i += 1
            elif nums1[i] < nums2[j]:
                f += nums1[i]
                i += 1
            elif nums1[i] > nums2[j]:
                g += nums2[j]
                j += 1
            else:
                f = g = max(f, g) + nums1[i]
                i += 1
                j += 1
        return max(f, g) % mod
```

#### Java

```java
class Solution {
    public int maxSum(int[] nums1, int[] nums2) {
        final int mod = (int) 1e9 + 7;
        int m = nums1.length, n = nums2.length;
        int i = 0, j = 0;
        long f = 0, g = 0;
        while (i < m || j < n) {
            if (i == m) {
                g += nums2[j++];
            } else if (j == n) {
                f += nums1[i++];
            } else if (nums1[i] < nums2[j]) {
                f += nums1[i++];
            } else if (nums1[i] > nums2[j]) {
                g += nums2[j++];
            } else {
                f = g = Math.max(f, g) + nums1[i];
                i++;
                j++;
            }
        }
        return (int) (Math.max(f, g) % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSum(vector<int>& nums1, vector<int>& nums2) {
        const int mod = 1e9 + 7;
        int m = nums1.size(), n = nums2.size();
        int i = 0, j = 0;
        long long f = 0, g = 0;
        while (i < m || j < n) {
            if (i == m) {
                g += nums2[j++];
            } else if (j == n) {
                f += nums1[i++];
            } else if (nums1[i] < nums2[j]) {
                f += nums1[i++];
            } else if (nums1[i] > nums2[j]) {
                g += nums2[j++];
            } else {
                f = g = max(f, g) + nums1[i];
                i++;
                j++;
            }
        }
        return max(f, g) % mod;
    }
};
```

#### Go

```go
func maxSum(nums1 []int, nums2 []int) int {
	const mod int = 1e9 + 7
	m, n := len(nums1), len(nums2)
	i, j := 0, 0
	f, g := 0, 0
	for i < m || j < n {
		if i == m {
			g += nums2[j]
			j++
		} else if j == n {
			f += nums1[i]
			i++
		} else if nums1[i] < nums2[j] {
			f += nums1[i]
			i++
		} else if nums1[i] > nums2[j] {
			g += nums2[j]
			j++
		} else {
			f = max(f, g) + nums1[i]
			g = f
			i++
			j++
		}
	}
	return max(f, g) % mod
}
```

#### TypeScript

```ts
function maxSum(nums1: number[], nums2: number[]): number {
    const mod = 1e9 + 7;
    const m = nums1.length;
    const n = nums2.length;
    let [f, g] = [0, 0];
    let [i, j] = [0, 0];
    while (i < m || j < n) {
        if (i === m) {
            g += nums2[j++];
        } else if (j === n) {
            f += nums1[i++];
        } else if (nums1[i] < nums2[j]) {
            f += nums1[i++];
        } else if (nums1[i] > nums2[j]) {
            g += nums2[j++];
        } else {
            f = g = Math.max(f, g) + nums1[i];
            i++;
            j++;
        }
    }
    return Math.max(f, g) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
