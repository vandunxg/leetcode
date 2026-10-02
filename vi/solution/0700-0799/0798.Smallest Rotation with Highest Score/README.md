---
comments: true
difficulty: Hard
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [798. Smallest Rotation with Highest Score](https://leetcode.com/problems/smallest-rotation-with-highest-score)

[中文文档](/solution/0700-0799/0798.Smallest%20Rotation%20with%20Highest%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code>. Bạn có thể xoay mảng một số lần không âm <code>k</code> để mảng trở thành <code>[nums[k], nums[k + 1], ... nums[nums.length - 1], nums[0], nums[1], ..., nums[k-1]]</code>. Sau đó, mỗi phần tử có giá trị nhỏ hơn hoặc bằng chỉ số của nó được tính một điểm.</p>

<ul>
	<li>Ví dụ, với <code>nums = [2,4,1,3,0]</code>, nếu xoay <code>k = 2</code> lần thì mảng trở thành <code>[1,3,0,2,4]</code>. Mảng được <code>3</code> điểm vì <code>1 &gt; 0</code> [không điểm], <code>3 &gt; 1</code> [không điểm], <code>0 &lt;= 2</code> [một điểm], <code>2 &lt;= 3</code> [một điểm], <code>4 &lt;= 4</code> [một điểm].</li>
</ul>

<p>Hãy trả về <em>chỉ số lần xoay </em><code>k</code><em> giúp mảng đạt số điểm cao nhất sau khi xoay </em><code>nums</code><em>.</em> Nếu có nhiều đáp án, hãy trả về chỉ số <code>k</code> nhỏ nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1,4,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Số điểm ứng với mỗi k được liệt kê bên dưới: 
k = 0,  nums = [2,3,1,4,0],    điểm 2
k = 1,  nums = [3,1,4,0,2],    điểm 3
k = 2,  nums = [1,4,0,2,3],    điểm 3
k = 3,  nums = [4,0,2,3,1],    điểm 4
k = 4,  nums = [0,2,3,1,4],    điểm 3
Vì vậy, ta nên chọn k = 3 vì đây là giá trị có số điểm cao nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,0,2,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Dù xoay thế nào, nums luôn đạt 3 điểm.
Vì vậy, ta chọn k nhỏ nhất là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau $k$ lần xoay, $nums[i]$ nằm ở chỉ số $(i-k)\bmod n$ và được tính điểm nếu chỉ số đó $\ge nums[i]$. $n\le 10^5$.
>
> Mỗi giá trị được tính điểm trên một khoảng vòng tròn của $k$. Mảng hiệu đánh dấu $+1$/$ -1$ tại hai đầu khoảng; tổng tiền tố cho biết số điểm ứng với mỗi $k$.
>
> Chọn $k$ nhỏ nhất có số điểm cao nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestRotation(self, nums: List[int]) -> int:
        n = len(nums)
        mx, ans = -1, n
        d = [0] * n
        for i, v in enumerate(nums):
            l, r = (i + 1) % n, (n + i + 1 - v) % n
            d[l] += 1
            d[r] -= 1
        s = 0
        for k, t in enumerate(d):
            s += t
            if s > mx:
                mx = s
                ans = k
        return ans
```

#### Java

```java
class Solution {
    public int bestRotation(int[] nums) {
        int n = nums.length;
        int[] d = new int[n];
        for (int i = 0; i < n; ++i) {
            int l = (i + 1) % n;
            int r = (n + i + 1 - nums[i]) % n;
            ++d[l];
            --d[r];
        }
        int mx = -1;
        int s = 0;
        int ans = n;
        for (int k = 0; k < n; ++k) {
            s += d[k];
            if (s > mx) {
                mx = s;
                ans = k;
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
    int bestRotation(vector<int>& nums) {
        int n = nums.size();
        int mx = -1, ans = n;
        vector<int> d(n);
        for (int i = 0; i < n; ++i) {
            int l = (i + 1) % n;
            int r = (n + i + 1 - nums[i]) % n;
            ++d[l];
            --d[r];
        }
        int s = 0;
        for (int k = 0; k < n; ++k) {
            s += d[k];
            if (s > mx) {
                mx = s;
                ans = k;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func bestRotation(nums []int) int {
	n := len(nums)
	d := make([]int, n)
	for i, v := range nums {
		l, r := (i+1)%n, (n+i+1-v)%n
		d[l]++
		d[r]--
	}
	mx, ans, s := -1, n, 0
	for k, t := range d {
		s += t
		if s > mx {
			mx = s
			ans = k
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
