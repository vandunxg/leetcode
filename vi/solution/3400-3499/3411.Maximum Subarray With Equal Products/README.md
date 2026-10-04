---
comments: true
difficulty: Easy
rating: 1443
source: Weekly Contest 431 Q1
tags:
    - Array
    - Math
    - Enumeration
    - Number Theory
    - Sliding Window
---

<!-- problem:start -->

# [3411. Maximum Subarray With Equal Products](https://leetcode.com/problems/maximum-subarray-with-equal-products)

[中文文档](/solution/3400-3499/3411.Maximum%20Subarray%20With%20Equal%20Products/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Một mảng <code>arr</code> được gọi là <strong>tương đương tích</strong> nếu <code>prod(arr) == lcm(arr) * gcd(arr)</code>, trong đó:</p>

<ul>
	<li><code>prod(arr)</code> là tích của tất cả các phần tử trong <code>arr</code>.</li>
	<li><code>gcd(arr)</code> là <span data-keyword="gcd-function">ƯCLN</span> của tất cả các phần tử trong <code>arr</code>.</li>
	<li><code>lcm(arr)</code> là <span data-keyword="lcm-function">BCNN</span> của tất cả các phần tử trong <code>arr</code>.</li>
</ul>

<p>Trả về độ dài của <span data-keyword="subarray-nonempty">mảng con không rỗng</span> <strong>tương đương tích</strong> <strong>dài nhất</strong> của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Mảng con tương đương tích dài nhất là <code>[1, 2, 1, 1, 1]</code>, trong đó&nbsp;<code>prod([1, 2, 1, 1, 1]) = 2</code>,&nbsp;<code>gcd([1, 2, 1, 1, 1]) = 1</code> và&nbsp;<code>lcm([1, 2, 1, 1, 1]) = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Mảng con tương đương tích dài nhất là <code>[3, 4, 5].</code></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,1,4,5,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện tích bằng $\gcd\cdot\operatorname{lcm}$ là một phép kiểm tra đại số trên mảng con. Với $n\le 100$ và các giá trị $\le 10$, ta có thể liệt kê mọi mảng con đồng thời duy trì tích, ƯCLN và BCNN.
>
> Tích tăng rất nhanh. Khi nó vượt quá $\operatorname{lcm}(\textit{nums})\cdot\max(\textit{nums})$, không có hậu tố dài hơn nào thỏa mãn đẳng thức, vì vậy vòng lặp bên trong nên dừng lại.
>
> Ta cố định đầu trái $i$, mở rộng dần về bên phải và cập nhật $p$, $g$, $l$, ghi nhận độ dài khi $p=g\cdot l$, rồi dừng khi $p$ đã quá lớn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxLength(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        max_p = lcm(*nums) * max(nums)
        for i in range(n):
            p, g, l = 1, 0, 1
            for j in range(i, n):
                p *= nums[j]
                g = gcd(g, nums[j])
                l = lcm(l, nums[j])
                if p == g * l:
                    ans = max(ans, j - i + 1)
                if p > max_p:
                    break
        return ans
```

#### Java

```java
class Solution {
    public int maxLength(int[] nums) {
        int mx = 0, ml = 1;
        for (int x : nums) {
            mx = Math.max(mx, x);
            ml = lcm(ml, x);
        }
        int maxP = ml * mx;
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int p = 1, g = 0, l = 1;
            for (int j = i; j < n; ++j) {
                p *= nums[j];
                g = gcd(g, nums[j]);
                l = lcm(l, nums[j]);
                if (p == g * l) {
                    ans = Math.max(ans, j - i + 1);
                }
                if (p > maxP) {
                    break;
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    private int lcm(int a, int b) {
        return a / gcd(a, b) * b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxLength(vector<int>& nums) {
        int mx = 0, ml = 1;
        for (int x : nums) {
            mx = max(mx, x);
            ml = lcm(ml, x);
        }

        long long maxP = (long long) ml * mx;
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            long long p = 1, g = 0, l = 1;
            for (int j = i; j < n; ++j) {
                p *= nums[j];
                g = gcd(g, nums[j]);
                l = lcm(l, nums[j]);

                if (p == g * l) {
                    ans = max(ans, j - i + 1);
                }
                if (p > maxP) {
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxLength(nums []int) int {
	mx, ml := 0, 1
	for _, x := range nums {
		mx = max(mx, x)
		ml = lcm(ml, x)
	}
	maxP := ml * mx
	n := len(nums)
	ans := 0
	for i := 0; i < n; i++ {
		p, g, l := 1, 0, 1
		for j := i; j < n; j++ {
			p *= nums[j]
			g = gcd(g, nums[j])
			l = lcm(l, nums[j])
			if p == g*l {
				ans = max(ans, j-i+1)
			}
			if p > maxP {
				break
			}
		}
	}
	return ans
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}

func lcm(a, b int) int {
	return a / gcd(a, b) * b
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
