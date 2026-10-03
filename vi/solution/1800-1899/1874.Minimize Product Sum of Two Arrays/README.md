---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1874. Minimize Product Sum of Two Arrays 🔒](https://leetcode.com/problems/minimize-product-sum-of-two-arrays)

[中文文档](/solution/1800-1899/1874.Minimize%20Product%20Sum%20of%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p><b>Tổng tích </b>của hai mảng cùng độ dài <code>a</code> và <code>b</code> bằng tổng <code>a[i] * b[i]</code> với mọi <code>0 &lt;= i &lt; a.length</code> (<strong>đánh chỉ số từ 0</strong>).</p>

<ul>
	<li>Ví dụ, nếu <code>a = [1,2,3,4]</code> và <code>b = [5,2,3,1]</code>, <strong>tổng tích</strong> sẽ là <code>1*5 + 2*2 + 3*3 + 4*1 = 22</code>.</li>
</ul>

<p>Cho hai mảng <code>nums1</code> và <code>nums2</code> có độ dài <code>n</code>, hãy trả về <em><strong>tổng tích nhỏ nhất</strong> nếu được phép <strong>sắp xếp lại</strong> <strong>thứ tự</strong> các phần tử trong </em><code>nums1</code>.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [5,3,4,2], nums2 = [4,2,2,5]
<strong>Đầu ra:</strong> 40
<strong>Giải thích:</strong>&nbsp;Ta có thể sắp xếp lại nums1 thành [3,5,4,2]. Tổng tích của [3,5,4,2] và [4,2,2,5] là 3*4 + 5*2 + 4*2 + 2*5 = 40.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,1,4,5,7], nums2 = [3,2,4,8,6]
<strong>Đầu ra:</strong> 65
<strong>Giải thích: </strong>Ta có thể sắp xếp lại nums1 thành [5,7,4,1,2]. Tổng tích của [5,7,4,1,2] và [3,2,4,8,6] là 5*3 + 7*2 + 4*4 + 1*8 + 2*6 = 65.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể sắp xếp lại một mảng và cần tìm giá trị nhỏ nhất của $\sum nums1[i]\cdot nums2[i]$. Với các số dương, các giá trị lớn nên được ghép với các giá trị nhỏ.
>
> Sắp xếp $nums1$ tăng dần và $nums2$ giảm dần, sau đó tính tổng các tích theo từng cặp.

<!-- thinking:end -->

Vì cả hai mảng đều gồm các số nguyên dương, để tối thiểu hóa tổng các tích, ta có thể nhân giá trị lớn nhất của một mảng với giá trị nhỏ nhất của mảng kia, giá trị lớn thứ hai với giá trị nhỏ thứ hai, và tiếp tục như vậy.

Do đó, ta sắp xếp mảng $\textit{nums1}$ theo thứ tự tăng dần và mảng $\textit{nums2}$ theo thứ tự giảm dần. Sau đó, ta nhân các phần tử tương ứng của hai mảng và cộng các kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài mảng $\textit{nums1}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minProductSum(self, nums1: List[int], nums2: List[int]) -> int:
        nums1.sort()
        nums2.sort(reverse=True)
        return sum(x * y for x, y in zip(nums1, nums2))
```

#### Java

```java
class Solution {
    public int minProductSum(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int n = nums1.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += nums1[i] * nums2[n - i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minProductSum(vector<int>& nums1, vector<int>& nums2) {
        ranges::sort(nums1);
        ranges::sort(nums2, greater<int>());
        int n = nums1.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += nums1[i] * nums2[i];
        }
        return ans;
    }
};
```

#### Go

```go
func minProductSum(nums1 []int, nums2 []int) (ans int) {
	sort.Ints(nums1)
	sort.Ints(nums2)
	for i, x := range nums1 {
		ans += x * nums2[len(nums2)-1-i]
	}
	return
}
```

#### TypeScript

```ts
function minProductSum(nums1: number[], nums2: number[]): number {
    nums1.sort((a, b) => a - b);
    nums2.sort((a, b) => b - a);
    let ans = 0;
    for (let i = 0; i < nums1.length; ++i) {
        ans += nums1[i] * nums2[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
