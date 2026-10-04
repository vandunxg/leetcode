---
comments: true
difficulty: Medium
rating: 1521
source: Biweekly Contest 120 Q2
tags:
    - Greedy
    - Array
    - Polygon
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2971. Find Polygon With the Largest Perimeter](https://leetcode.com/problems/find-polygon-with-the-largest-perimeter)

[中文文档](/solution/2900-2999/2971.Find%20Polygon%20With%20the%20Largest%20Perimeter/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>dương</strong> <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Đa giác</strong> là một hình phẳng kín có ít nhất <code>3</code> cạnh. <strong>Cạnh dài nhất</strong> của đa giác <strong>nhỏ hơn</strong> tổng các cạnh còn lại.</p>

<p>Ngược lại, nếu có <code>k</code> (<code>k &gt;= 3</code>) số thực <strong>dương</strong> <code>a<sub>1</sub></code>, <code>a<sub>2</sub></code>, <code>a<sub>3</sub></code>, ..., <code>a<sub>k</sub></code> thỏa mãn <code>a<sub>1</sub> &lt;= a<sub>2</sub> &lt;= a<sub>3</sub> &lt;= ... &lt;= a<sub>k</sub></code> <strong>và</strong> <code>a<sub>1</sub> + a<sub>2</sub> + a<sub>3</sub> + ... + a<sub>k-1</sub> &gt; a<sub>k</sub></code>, thì <strong>luôn</strong> tồn tại một đa giác có <code>k</code> cạnh với độ dài các cạnh lần lượt là <code>a<sub>1</sub></code>, <code>a<sub>2</sub></code>, <code>a<sub>3</sub></code>, ..., <code>a<sub>k</sub></code>.</p>

<p><strong>Chu vi</strong> của một đa giác là tổng độ dài các cạnh của nó.</p>

<p>Trả về <em><strong>chu vi</strong> <strong>lớn nhất</strong> có thể có của một <strong>đa giác</strong> mà các cạnh được tạo từ</em> <code>nums</code>, <em>hoặc</em> <code>-1</code> <em>nếu không thể tạo đa giác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Đa giác duy nhất có thể tạo từ nums có 3 cạnh: 5, 5 và 5. Chu vi là 5 + 5 + 5 = 15.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,12,1,2,5,50,3]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Đa giác có chu vi lớn nhất có thể tạo từ nums có 5 cạnh: 1, 1, 2, 3 và 5. Chu vi là 1 + 1 + 2 + 3 + 5 = 12.
Không thể tạo đa giác có 12 hoặc 50 là cạnh dài nhất, vì không thể chọn 2 cạnh nhỏ hơn trở lên có tổng lớn hơn một trong hai cạnh đó.
Có thể chứng minh rằng chu vi lớn nhất có thể có là 12.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,50]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào tạo được đa giác từ nums, vì đa giác phải có ít nhất 3 cạnh và 50 &gt; 5 + 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một đa giác cần có cạnh dài nhất nhỏ hơn tổng các cạnh còn lại. Sau khi sắp xếp, nếu cạnh dài nhất là $a_k$, thì các cạnh còn lại nên là $k-1$ giá trị nhỏ hơn, vì vậy chỉ cần xét các đoạn tiền tố.
>
> Khi $s[k-1]>nums[k-1]$, cập nhật chu vi bằng $s[k]$. Duyệt $k$ từ $3$ đến $n$ với $n \le 10^5$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestPerimeter(self, nums: List[int]) -> int:
        nums.sort()
        s = list(accumulate(nums, initial=0))
        ans = -1
        for k in range(3, len(nums) + 1):
            if s[k - 1] > nums[k - 1]:
                ans = max(ans, s[k])
        return ans
```

#### Java

```java
class Solution {
    public long largestPerimeter(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        long ans = -1;
        for (int k = 3; k <= n; ++k) {
            if (s[k - 1] > nums[k - 1]) {
                ans = Math.max(ans, s[k]);
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
    long long largestPerimeter(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<long long> s(n + 1);
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        long long ans = -1;
        for (int k = 3; k <= n; ++k) {
            if (s[k - 1] > nums[k - 1]) {
                ans = max(ans, s[k]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestPerimeter(nums []int) int64 {
	sort.Ints(nums)
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	ans := -1
	for k := 3; k <= n; k++ {
		if s[k-1] > nums[k-1] {
			ans = max(ans, s[k])
		}
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function largestPerimeter(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let ans = -1;
    for (let k = 3; k <= n; ++k) {
        if (s[k - 1] > nums[k - 1]) {
            ans = Math.max(ans, s[k]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
