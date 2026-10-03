---
comments: true
difficulty: Medium
rating: 1785
source: Biweekly Contest 55 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1911. Maximum Alternating Subsequence Sum](https://leetcode.com/problems/maximum-alternating-subsequence-sum)

[中文文档](/solution/1900-1999/1911.Maximum%20Alternating%20Subsequence%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Tổng xen kẽ</strong> của một mảng được <strong>đánh chỉ số từ 0</strong> được định nghĩa là <strong>tổng</strong> các phần tử ở chỉ số <strong>chẵn</strong> <strong>trừ</strong> đi <strong>tổng</strong> các phần tử ở chỉ số <strong>lẻ</strong>.</p>

<ul>
	<li>Ví dụ, tổng xen kẽ của <code>[4,2,5,3]</code> là <code>(4 + 5) - (2 + 3) = 4</code>.</li>
</ul>

<p>Cho một mảng <code>nums</code>, hãy trả về <em><strong>tổng xen kẽ lớn nhất</strong> của một dãy con bất kỳ của </em><code>nums</code><em> (sau khi <strong>đánh lại chỉ số</strong> cho các phần tử của dãy con)</em>.</p>

<ul>
</ul>

<p><strong>Dãy con</strong> của một mảng là một mảng mới được tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự tương đối của các phần tử còn lại. Ví dụ, <code>[2,7,4]</code> là một dãy con của <code>[4,<u>2</u>,3,<u>7</u>,2,1,<u>4</u>]</code> (các phần tử được gạch chân), còn <code>[2,4,2]</code> thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u>4</u>,<u>2</u>,<u>5</u>,3]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Chọn dãy con [4,2,5] là tối ưu, với tổng xen kẽ (4 + 5) - 2 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,6,7,<u>8</u>]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Chọn dãy con [8] là tối ưu, với tổng xen kẽ bằng 8.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u>6</u>,2,<u>1</u>,2,4,<u>5</u>]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Chọn dãy con [6,1,5] là tối ưu, với tổng xen kẽ (6 + 5) - 1 = 10.
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

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê tất cả các dãy con có độ phức tạp theo cấp số mũ. Với $n\le 10^5$, ta cần một quy hoạch động tuyến tính dựa trên tính chẵn lẻ của vị trí phần tử được chọn cuối cùng.
>
> Gọi $f[i]$ là tổng xen kẽ tốt nhất của $i$ phần tử đầu tiên, trong đó phần tử được chọn cuối cùng nằm ở vị trí lẻ (bị trừ), còn $g[i]$ là tổng tốt nhất khi phần tử được chọn cuối cùng nằm ở vị trí chẵn (được cộng). Phần tử $x$ hoặc được thêm vào vị trí có tính chẵn lẻ đối lập, hoặc bị bỏ qua.
>
> Các công thức truy hồi là $f[i]=\max(g[i-1]-x,f[i-1])$ và $g[i]=\max(f[i-1]+x,g[i-1])$; đáp án là giá trị lớn hơn giữa $f[n]$ và $g[n]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        n = len(nums)
        f = [0] * (n + 1)
        g = [0] * (n + 1)
        for i, x in enumerate(nums, 1):
            f[i] = max(g[i - 1] - x, f[i - 1])
            g[i] = max(f[i - 1] + x, g[i - 1])
        return max(f[n], g[n])
```

#### Java

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {
        int n = nums.length;
        long[] f = new long[n + 1];
        long[] g = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            f[i] = Math.max(g[i - 1] - nums[i - 1], f[i - 1]);
            g[i] = Math.max(f[i - 1] + nums[i - 1], g[i - 1]);
        }
        return Math.max(f[n], g[n]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxAlternatingSum(vector<int>& nums) {
        int n = nums.size();
        vector<long long> f(n + 1), g(n + 1);
        for (int i = 1; i <= n; ++i) {
            f[i] = max(g[i - 1] - nums[i - 1], f[i - 1]);
            g[i] = max(f[i - 1] + nums[i - 1], g[i - 1]);
        }
        return max(f[n], g[n]);
    }
};
```

#### Go

```go
func maxAlternatingSum(nums []int) int64 {
	n := len(nums)
	f := make([]int, n+1)
	g := make([]int, n+1)
	for i, x := range nums {
		i++
		f[i] = max(g[i-1]-x, f[i-1])
		g[i] = max(f[i-1]+x, g[i-1])
	}
	return int64(max(f[n], g[n]))
}
```

#### TypeScript

```ts
function maxAlternatingSum(nums: number[]): number {
    const n = nums.length;
    const f: number[] = new Array(n + 1).fill(0);
    const g = f.slice();
    for (let i = 1; i <= n; ++i) {
        f[i] = Math.max(g[i - 1] + nums[i - 1], f[i - 1]);
        g[i] = Math.max(f[i - 1] - nums[i - 1], g[i - 1]);
    }
    return Math.max(f[n], g[n]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu hóa không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ đọc $f$ và $g$ của bước trước, ta có thể dùng hai biến vô hướng cập nhật từ trái sang phải để thay thế các mảng và giảm không gian phụ xuống còn $O(1)$.

<!-- thinking:end -->

$f[i]$ và $g[i]$ chỉ phụ thuộc vào chỉ số trước đó, nên chỉ cần hai biến là đủ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        f = g = 0
        for x in nums:
            f, g = max(g - x, f), max(f + x, g)
        return max(f, g)
```

#### Java

```java
class Solution {
    public long maxAlternatingSum(int[] nums) {
        long f = 0, g = 0;
        for (int x : nums) {
            long ff = Math.max(g - x, f);
            long gg = Math.max(f + x, g);
            f = ff;
            g = gg;
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxAlternatingSum(vector<int>& nums) {
        long long f = 0, g = 0;
        for (int& x : nums) {
            long ff = max(g - x, f), gg = max(f + x, g);
            f = ff, g = gg;
        }
        return max(f, g);
    }
};
```

#### Go

```go
func maxAlternatingSum(nums []int) int64 {
	var f, g int
	for _, x := range nums {
		f, g = max(g-x, f), max(f+x, g)
	}
	return int64(max(f, g))
}
```

#### TypeScript

```ts
function maxAlternatingSum(nums: number[]): number {
    let [f, g] = [0, 0];
    for (const x of nums) {
        [f, g] = [Math.max(g - x, f), Math.max(f + x, g)];
    }
    return g;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
