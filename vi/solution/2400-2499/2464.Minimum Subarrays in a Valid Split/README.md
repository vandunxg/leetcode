---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Number Theory
---

<!-- problem:start -->

# [2464. Minimum Subarrays in a Valid Split 🔒](https://leetcode.com/problems/minimum-subarrays-in-a-valid-split)

[中文文档](/solution/2400-2499/2464.Minimum%20Subarrays%20in%20a%20Valid%20Split/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Việc chia mảng số nguyên <code>nums</code> thành các <strong>mảng con</strong> là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><em>ước chung lớn nhất</em> của phần tử đầu tiên và phần tử cuối cùng trong mỗi mảng con <strong>lớn hơn</strong> <code>1</code>, và</li>
	<li>mỗi phần tử của <code>nums</code> thuộc đúng một mảng con.</li>
</ul>

<p>Trả về <em>số lượng <strong>ít nhất</strong> các mảng con trong một cách chia <strong>hợp lệ</strong> của</em> <code>nums</code>. Nếu không thể chia mảng con theo cách hợp lệ, trả về <code>-1</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li><strong>Ước chung lớn nhất</strong> của hai số là số nguyên dương lớn nhất chia hết cho cả hai số.</li>
	<li><strong>Mảng con</strong> là một phần liên tiếp, không rỗng của một mảng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,3,4,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chia hợp lệ theo cách sau: [2,6] | [3,4,3].
- Phần tử đầu của mảng con thứ 1<sup>st</sup> là 2 và phần tử cuối là 6. Ước chung lớn nhất của chúng là 2, lớn hơn 1.
- Phần tử đầu của mảng con thứ 2<sup>nd</sup> là 3 và phần tử cuối là 3. Ước chung lớn nhất của chúng là 3, lớn hơn 1.
Có thể chứng minh rằng 2 là số lượng mảng con ít nhất có thể đạt được trong một cách chia hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chia hợp lệ theo cách sau: [3] | [5].
- Phần tử đầu của mảng con thứ 1<sup>st</sup> là 3 và phần tử cuối là 3. Ước chung lớn nhất của chúng là 3, lớn hơn 1.
- Phần tử đầu của mảng con thứ 2<sup>nd</sup> là 5 và phần tử cuối là 5. Ước chung lớn nhất của chúng là 5, lớn hơn 1.
Có thể chứng minh rằng 2 là số lượng mảng con ít nhất có thể đạt được trong một cách chia hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể tạo cách chia hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn là hợp lệ khi và chỉ khi GCD của hai đầu mút lớn hơn $1$, và ta cần tìm số đoạn ít nhất. Từ $i$, thử mọi vị trí kết thúc bên phải $j$ và chọn $1+dfs(j+1)$ khi GCD thỏa mãn. Ghi nhớ các GCD trong $O(n^2)$; trả về $-1$ nếu không tìm được cách chia.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$ biểu diễn số lượng mảng con ít nhất khi bắt đầu từ chỉ số $i$. Với chỉ số $i$, ta có thể duyệt qua mọi điểm chia $j$, tức là $i \leq j < n$, trong đó $n$ là độ dài của mảng. Với mỗi điểm chia $j$, ta cần xác định liệu ước chung lớn nhất của $nums[i]$ và $nums[j]$ có lớn hơn $1$ hay không. Nếu lớn hơn $1$, ta có thể chia tại đó, và số lượng mảng con là $1 + dfs(j + 1)$; nếu không, số lượng mảng con là $+\infty$. Cuối cùng, ta lấy giá trị nhỏ nhất trong tất cả số lượng mảng con.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSubarraySplit(self, nums: List[int]) -> int:
        @cache
        def dfs(i):
            if i >= n:
                return 0
            ans = inf
            for j in range(i, n):
                if gcd(nums[i], nums[j]) > 1:
                    ans = min(ans, 1 + dfs(j + 1))
            return ans

        n = len(nums)
        ans = dfs(0)
        dfs.cache_clear()
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    private int n;
    private int[] f;
    private int[] nums;
    private int inf = 0x3f3f3f3f;

    public int validSubarraySplit(int[] nums) {
        n = nums.length;
        f = new int[n];
        this.nums = nums;
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] > 0) {
            return f[i];
        }
        int ans = inf;
        for (int j = i; j < n; ++j) {
            if (gcd(nums[i], nums[j]) > 1) {
                ans = Math.min(ans, 1 + dfs(j + 1));
            }
        }
        f[i] = ans;
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int inf = 0x3f3f3f3f;
    int validSubarraySplit(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);
        function<int(int)> dfs = [&](int i) -> int {
            if (i >= n) return 0;
            if (f[i]) return f[i];
            int ans = inf;
            for (int j = i; j < n; ++j) {
                if (__gcd(nums[i], nums[j]) > 1) {
                    ans = min(ans, 1 + dfs(j + 1));
                }
            }
            f[i] = ans;
            return ans;
        };
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
func validSubarraySplit(nums []int) int {
	n := len(nums)
	f := make([]int, n)
	var dfs func(int) int
	const inf int = 0x3f3f3f3f
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		ans := inf
		for j := i; j < n; j++ {
			if gcd(nums[i], nums[j]) > 1 {
				ans = min(ans, 1+dfs(j+1))
			}
		}
		f[i] = ans
		return ans
	}
	ans := dfs(0)
	if ans < inf {
		return ans
	}
	return -1
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
