---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [891. Sum of Subsequence Widths](https://leetcode.com/problems/sum-of-subsequence-widths)

[中文文档](/solution/0800-0899/0891.Sum%20of%20Subsequence%20Widths/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Độ rộng</strong> của một dãy là hiệu giữa phần tử lớn nhất và phần tử nhỏ nhất trong dãy đó.</p>

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>tổng <strong>độ rộng</strong> của mọi <strong>dãy con</strong> không rỗng của </em><code>nums</code>. Vì đáp án có thể rất lớn, hãy trả về <strong>phần dư khi chia cho</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Dãy con</strong> là dãy có thể thu được từ một mảng bằng cách xóa một số phần tử hoặc không xóa phần tử nào, đồng thời giữ nguyên thứ tự các phần tử còn lại. Ví dụ, <code>[3,6,2,7]</code> là dãy con của mảng <code>[0,3,1,6,2,2,7]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,1,3]
<strong>Output:</strong> 6
Giải thích: Các dãy con là [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3].
Độ rộng tương ứng là 0, 0, 0, 1, 1, 2, 2.
Tổng các độ rộng này bằng 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tổng độ rộng của mọi dãy con. Có $2^n$ dãy con và $n\le 10^5$, nên không thể liệt kê tất cả. Độ rộng chỉ phụ thuộc vào giá trị nhỏ nhất và lớn nhất. Sau khi sắp xếp, $nums[i]$ là giá trị lớn nhất của $2^i$ dãy con và là giá trị nhỏ nhất của $2^{n-1-i}$ dãy con.
>
> Duyệt mảng đã sắp xếp một lần, cộng dồn $(v-\textit{nums}[n-1-i])\cdot 2^i$ đồng thời cập nhật lũy thừa của hai, sau đó lấy modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumSubseqWidths(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        nums.sort()
        ans, p = 0, 1
        for i, v in enumerate(nums):
            ans = (ans + (v - nums[-i - 1]) * p) % mod
            p = (p << 1) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int sumSubseqWidths(int[] nums) {
        Arrays.sort(nums);
        long ans = 0, p = 1;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            ans = (ans + (nums[i] - nums[n - i - 1]) * p + MOD) % MOD;
            p = (p << 1) % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int sumSubseqWidths(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        long ans = 0, p = 1;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            ans = (ans + (nums[i] - nums[n - i - 1]) * p + mod) % mod;
            p = (p << 1) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func sumSubseqWidths(nums []int) (ans int) {
	const mod int = 1e9 + 7
	sort.Ints(nums)
	p, n := 1, len(nums)
	for i, v := range nums {
		ans = (ans + (v-nums[n-i-1])*p + mod) % mod
		p = (p << 1) % mod
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
