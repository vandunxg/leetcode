---
comments: true
difficulty: Medium
rating: 1549
source: Biweekly Contest 95 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [2527. Find Xor-Beauty of Array](https://leetcode.com/problems/find-xor-beauty-of-array)

[中文文档](/solution/2500-2599/2527.Find%20Xor-Beauty%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p><strong>Giá trị hiệu dụng</strong> của ba chỉ số <code>i</code>, <code>j</code> và <code>k</code> được định nghĩa là <code>((nums[i] | nums[j]) &amp; nums[k])</code>.</p>

<p><strong>Xor-beauty</strong> của mảng là phép XOR của <strong>giá trị hiệu dụng của tất cả các bộ ba chỉ số có thể có</strong> <code>(i, j, k)</code> với <code>0 &lt;= i, j, k &lt; n</code>.</p>

<p>Trả về <em>xor-beauty của</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li><code>val1 | val2</code> là phép OR theo bit của <code>val1</code> và <code>val2</code>.</li>
	<li><code>val1 &amp; val2</code> là phép AND theo bit của <code>val1</code> và <code>val2</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Các bộ ba và giá trị hiệu dụng tương ứng được liệt kê dưới đây:
- (0,0,0) với giá trị hiệu dụng ((1 | 1) &amp; 1) = 1
- (0,0,1) với giá trị hiệu dụng ((1 | 1) &amp; 4) = 0
- (0,1,0) với giá trị hiệu dụng ((1 | 4) &amp; 1) = 1
- (0,1,1) với giá trị hiệu dụng ((1 | 4) &amp; 4) = 4
- (1,0,0) với giá trị hiệu dụng ((4 | 1) &amp; 1) = 1
- (1,0,1) với giá trị hiệu dụng ((4 | 1) &amp; 4) = 4
- (1,1,0) với giá trị hiệu dụng ((4 | 4) &amp; 1) = 0
- (1,1,1) với giá trị hiệu dụng ((4 | 4) &amp; 4) = 4
Xor-beauty của mảng là XOR theo bit của tất cả các giá trị = 1 ^ 0 ^ 1 ^ 4 ^ 1 ^ 4 ^ 0 ^ 4 = 5.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [15,45,20,2,34,35,5,44,32,30]
<strong>Đầu ra:</strong> 34
<strong>Giải thích:</strong> <code>The xor-beauty of the given array is 34.</code>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Không thể thực hiện phép XOR của $(\textit{nums}[i]\mid \textit{nums}[j])\&\textit{nums}[k]$ trên tất cả các bộ ba khi $n\le 10^5$. Các cặp giống nhau sẽ triệt tiêu khi thực hiện XOR.
>
> Khi $i\neq j$, $(i,j,k)$ tương ứng với $(j,i,k)$ và chúng triệt tiêu lẫn nhau. Khi $i=j$ nhưng $i\neq k$, $\textit{nums}[i]\&\textit{nums}[k]$ triệt tiêu với cặp hoán đổi. Chỉ còn trường hợp $i=j=k$, nên đáp án là XOR của mọi phần tử.

<!-- thinking:end -->

Trước hết, xét trường hợp $i$ và $j$ không bằng nhau. Khi đó, `((nums[i] | nums[j]) & nums[k])` và `((nums[j] | nums[i]) & nums[k])` cho cùng một kết quả, nên XOR của chúng bằng $0$.

Do đó, ta chỉ cần xét trường hợp $i$ và $j$ bằng nhau. Khi đó, `((nums[i] | nums[j]) & nums[k]) = (nums[i] & nums[k])`. Nếu $i \neq k$, giá trị này giống với kết quả của `nums[k] & nums[i]`, nên XOR của hai giá trị bằng $0$.

Vì vậy, cuối cùng ta chỉ cần xét trường hợp $i = j = k$, và đáp án là kết quả XOR của tất cả $nums[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def xorBeauty(self, nums: List[int]) -> int:
        return reduce(xor, nums)
```

#### Java

```java
class Solution {
    public int xorBeauty(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            ans ^= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int xorBeauty(vector<int>& nums) {
        int ans = 0;
        for (auto& x : nums) {
            ans ^= x;
        }
        return ans;
    }
};
```

#### Go

```go
func xorBeauty(nums []int) (ans int) {
	for _, x := range nums {
		ans ^= x
	}
	return
}
```

#### TypeScript

```ts
function xorBeauty(nums: number[]): number {
    return nums.reduce((acc, cur) => acc ^ cur, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
