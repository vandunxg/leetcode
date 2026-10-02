---
comments: true
difficulty: Easy
rating: 1301
source: Weekly Contest 214 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1646. Get Maximum in Generated Array](https://leetcode.com/problems/get-maximum-in-generated-array)

[中文文档](/solution/1600-1699/1646.Get%20Maximum%20in%20Generated%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>. Một mảng số nguyên <code>nums</code> có độ dài <code>n + 1</code>, <strong>đánh chỉ số từ 0</strong>, được tạo như sau:</p>

<ul>
	<li><code>nums[0] = 0</code></li>
	<li><code>nums[1] = 1</code></li>
	<li><code>nums[2 * i] = nums[i]</code> when <code>2 &lt;= 2 * i &lt;= n</code></li>
	<li><code>nums[2 * i + 1] = nums[i] + nums[i + 1]</code> when <code>2 &lt;= 2 * i + 1 &lt;= n</code></li>
</ul>

<p>Trả về<strong> </strong><em>số nguyên <strong>lớn nhất</strong> trong mảng </em><code>nums</code>​​​.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 7
<strong>Output:</strong> 3
<strong>Explanation:</strong> Theo các quy tắc đã cho:
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
Do đó, nums = [0,1,1,2,1,3,2,3], và giá trị lớn nhất là max(0,1,1,2,1,3,2,3) = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> 1
<strong>Explanation:</strong> Theo các quy tắc đã cho, nums = [0,1,1]. Giá trị lớn nhất là max(0,1,1) = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> Theo các quy tắc đã cho, nums = [0,1,1,2]. Giá trị lớn nhất là max(0,1,1,2) = 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng được định nghĩa bởi công thức truy hồi theo tính chẵn lẻ và $n$ rất nhỏ, nên ta tạo mảng rồi lấy giá trị lớn nhất.
>
> Với $n<2$, trả về $n$. Ngược lại, đặt $\textit{nums}[0]=0,\textit{nums}[1]=1$, sao chép $\textit{nums}[i/2]$ khi $i$ chẵn, và cộng hai chỉ số nửa khi $i$ lẻ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaximumGenerated(self, n: int) -> int:
        if n < 2:
            return n
        nums = [0] * (n + 1)
        nums[1] = 1
        for i in range(2, n + 1):
            nums[i] = nums[i >> 1] if i % 2 == 0 else nums[i >> 1] + nums[(i >> 1) + 1]
        return max(nums)
```

#### Java

```java
class Solution {
    public int getMaximumGenerated(int n) {
        if (n < 2) {
            return n;
        }
        int[] nums = new int[n + 1];
        nums[1] = 1;
        for (int i = 2; i <= n; ++i) {
            nums[i] = i % 2 == 0 ? nums[i >> 1] : nums[i >> 1] + nums[(i >> 1) + 1];
        }
        return Arrays.stream(nums).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaximumGenerated(int n) {
        if (n < 2) {
            return n;
        }
        int nums[n + 1];
        nums[0] = 0;
        nums[1] = 1;
        for (int i = 2; i <= n; ++i) {
            nums[i] = i % 2 == 0 ? nums[i >> 1] : nums[i >> 1] + nums[(i >> 1) + 1];
        }
        return *max_element(nums, nums + n + 1);
    }
};
```

#### Go

```go
func getMaximumGenerated(n int) int {
	if n < 2 {
		return n
	}
	nums := make([]int, n+1)
	nums[1] = 1
	for i := 2; i <= n; i++ {
		if i%2 == 0 {
			nums[i] = nums[i/2]
		} else {
			nums[i] = nums[i/2] + nums[i/2+1]
		}
	}
	return slices.Max(nums)
}
```

#### TypeScript

```ts
function getMaximumGenerated(n: number): number {
    if (n === 0) {
        return 0;
    }
    const nums: number[] = new Array(n + 1).fill(0);
    nums[1] = 1;
    for (let i = 2; i < n + 1; ++i) {
        nums[i] = i % 2 === 0 ? nums[i >> 1] : nums[i >> 1] + nums[(i >> 1) + 1];
    }
    return Math.max(...nums);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
