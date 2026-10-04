---
comments: true
difficulty: Medium
rating: 1518
source: Weekly Contest 421 Q1
tags:
    - Array
    - Math
    - Number Theory
    - Least Common Multiple
---

<!-- problem:start -->

# [3334. Find the Maximum Factor Score of Array](https://leetcode.com/problems/find-the-maximum-factor-score-of-array)

[Tài liệu tiếng Trung](/solution/3300-3399/3334.Find%20the%20Maximum%20Factor%20Score%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p><strong>Điểm nhân tử</strong> của một mảng được định nghĩa là <em>tích</em> của LCM và GCD của tất cả các phần tử trong mảng đó.</p>

<p>Hãy trả về <strong>điểm nhân tử lớn nhất</strong> của <code>nums</code> sau khi xóa <strong>tối đa</strong> một phần tử khỏi mảng.</p>

<p><strong>Lưu ý</strong> rằng <em>cả</em> <span data-keyword="lcm-function">LCM</span> và <span data-keyword="gcd-function">GCD</span> của một số duy nhất đều là chính số đó, còn <em>điểm nhân tử</em> của một mảng <strong>rỗng</strong> là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,8,16]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">64</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa 2, GCD của các phần tử còn lại là 4 còn LCM là 16, nên điểm nhân tử lớn nhất là <code>4 * 16 = 64</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">60</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm nhân tử lớn nhất là 60 và có thể đạt được mà không cần xóa phần tử nào.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3]</span></p>

<p><strong>Đầu ra:</strong> 9</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 100</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là $\gcd \times \operatorname{lcm}$ của toàn bộ mảng hoặc của mảng sau khi xóa một phần tử. Với $n \le 100$, ta có thể tính lại cho mỗi lần xóa, nhưng các phép gộp ở prefix/suffix cho phép trả lời mỗi ứng viên trong $O(1)$.
>
> Xây dựng suffix $\gcd$ và $\operatorname{lcm}$, sau đó duyệt một prefix: xóa $i$ tương ứng với việc gộp $\textit{pre}$ với $\textit{suf}[i+1]$.
>
> Ta cũng so sánh điểm số khi không xóa phần tử, tức $\textit{suf}[0]$, rồi giữ lại tích lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        n = len(nums)
        suf_gcd = [0] * (n + 1)
        suf_lcm = [0] * n + [1]
        for i in range(n - 1, -1, -1):
            suf_gcd[i] = gcd(suf_gcd[i + 1], nums[i])
            suf_lcm[i] = lcm(suf_lcm[i + 1], nums[i])
        ans = suf_gcd[0] * suf_lcm[0]
        pre_gcd, pre_lcm = 0, 1
        for i, x in enumerate(nums):
            ans = max(ans, gcd(pre_gcd, suf_gcd[i + 1]) * lcm(pre_lcm, suf_lcm[i + 1]))
            pre_gcd = gcd(pre_gcd, x)
            pre_lcm = lcm(pre_lcm, x)
        return ans
```

#### Java

```java
class Solution {
    public long maxScore(int[] nums) {
        int n = nums.length;
        long[] sufGcd = new long[n + 1];
        long[] sufLcm = new long[n + 1];
        sufLcm[n] = 1;
        for (int i = n - 1; i >= 0; --i) {
            sufGcd[i] = gcd(sufGcd[i + 1], nums[i]);
            sufLcm[i] = lcm(sufLcm[i + 1], nums[i]);
        }
        long ans = sufGcd[0] * sufLcm[0];
        long preGcd = 0, preLcm = 1;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, gcd(preGcd, sufGcd[i + 1]) * lcm(preLcm, sufLcm[i + 1]));
            preGcd = gcd(preGcd, nums[i]);
            preLcm = lcm(preLcm, nums[i]);
        }
        return ans;
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private long lcm(long a, long b) {
        return a / gcd(a, b) * b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums) {
        int n = nums.size();
        vector<long long> sufGcd(n + 1, 0);
        vector<long long> sufLcm(n + 1, 1);
        for (int i = n - 1; i >= 0; --i) {
            sufGcd[i] = gcd(sufGcd[i + 1], nums[i]);
            sufLcm[i] = lcm(sufLcm[i + 1], nums[i]);
        }

        long long ans = sufGcd[0] * sufLcm[0];
        long long preGcd = 0, preLcm = 1;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, gcd(preGcd, sufGcd[i + 1]) * lcm(preLcm, sufLcm[i + 1]));
            preGcd = gcd(preGcd, nums[i]);
            preLcm = lcm(preLcm, nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(nums []int) int64 {
    n := len(nums)
    sufGcd := make([]int64, n+1)
    sufLcm := make([]int64, n+1)
    sufLcm[n] = 1
    for i := n - 1; i >= 0; i-- {
        sufGcd[i] = gcd(sufGcd[i+1], int64(nums[i]))
        sufLcm[i] = lcm(sufLcm[i+1], int64(nums[i]))
    }

    ans := sufGcd[0] * sufLcm[0]
    preGcd, preLcm := int64(0), int64(1)
    for i := 0; i < n; i++ {
        ans = max(ans, gcd(preGcd, sufGcd[i+1])*lcm(preLcm, sufLcm[i+1]))
        preGcd = gcd(preGcd, int64(nums[i]))
        preLcm = lcm(preLcm, int64(nums[i]))
    }
    return ans
}

func gcd(a, b int64) int64 {
    if b == 0 {
        return a
    }
    return gcd(b, a%b)
}

func lcm(a, b int64) int64 {
    return a / gcd(a, b) * b
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const n = nums.length;
    const sufGcd: number[] = Array(n + 1).fill(0);
    const sufLcm: number[] = Array(n + 1).fill(1);
    for (let i = n - 1; i >= 0; i--) {
        sufGcd[i] = gcd(sufGcd[i + 1], nums[i]);
        sufLcm[i] = lcm(sufLcm[i + 1], nums[i]);
    }

    let ans = sufGcd[0] * sufLcm[0];
    let preGcd = 0,
        preLcm = 1;
    for (let i = 0; i < n; i++) {
        ans = Math.max(ans, gcd(preGcd, sufGcd[i + 1]) * lcm(preLcm, sufLcm[i + 1]));
        preGcd = gcd(preGcd, nums[i]);
        preLcm = lcm(preLcm, nums[i]);
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}

function lcm(a: number, b: number): number {
    return (a / gcd(a, b)) * b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
