---
comments: true
difficulty: Easy
rating: 1216
source: Weekly Contest 513 Q1
tags:
    - Array
    - Math
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [4010. Maximize Pair Strength Using GCD](https://leetcode.com/problems/maximize-pair-strength-using-gcd)

[中文文档](/solution/4000-4099/4010.Maximize%20Pair%20Strength%20Using%20GCD/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Chọn chính xác một cặp chỉ số phân biệt <code>i</code> và <code>j</code>. Độ mạnh của cặp được định nghĩa là <code>(nums[i] * nums[j]) / <span data-keyword="gcd-function">gcd(nums[i], nums[j])</span><sup>2</sup></code>.</p>

<p>Trả về độ mạnh lớn nhất trong tất cả các cặp có thể chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>i = 1</code> và <code>j = 2</code> ta có độ mạnh <code>(3 * 5) / gcd(3, 5)<sup>2</sup> = 15 / 1 = 15</code>, đây là giá trị lớn nhất trong tất cả các cặp.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>i = 1</code> và <code>j = 2</code> ta có độ mạnh <code>(6 * 8) / gcd(6, 8)<sup>2</sup> = 48 / 4 = 12</code>, đây là giá trị lớn nhất trong tất cả các cặp.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code>i = 0</code> và <code>j = 1</code> ta có độ mạnh <code>(3 * 3) / gcd(3, 3)<sup>2</sup> = 9 / 9 = 1</code>, đây là giá trị lớn nhất trong tất cả các cặp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Độ mạnh của một cặp chỉ phụ thuộc vào hai giá trị và $\gcd$. Với $n\le 2000$, có khoảng $2\times 10^6$ cặp không có thứ tự.
>
> Ta liệt kê các cặp $i<j$, tính $\frac{\textit{nums}[i]\cdot\textit{nums}[j]}{\gcd^2}$ bằng thuật toán Euclid, rồi giữ lại giá trị lớn nhất.
>
> $O(n^2\log M)$ đã đáp ứng giới hạn, vì vậy không cần nhóm mảng theo các ước chung.

<!-- thinking:end -->

Ta trực tiếp liệt kê tất cả các cặp $(i, j)$ với $i < j$, tính độ mạnh của từng cặp $\frac{\textit{nums}[i] \times \textit{nums}[j]}{\gcd(\textit{nums}[i], \textit{nums}[j])^2}$, rồi lấy giá trị lớn nhất.

Ước chung lớn nhất $\gcd$ có thể được tính bằng thuật toán Euclid.

Độ phức tạp thời gian là $O(n^2 \times \log M)$, trong đó $n$ là độ dài của mảng $\textit{nums}$ và $M$ là giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPairStrength(self, nums: list[int]) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            for j in range(i + 1, n):
                x = nums[i] * nums[j] // gcd(nums[i], nums[j]) ** 2
                ans = max(ans, x)
        return ans
```

#### Java

```java
class Solution {
    public long maxPairStrength(int[] nums) {
        int n = nums.length;
        long ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                long g = gcd(nums[i], nums[j]);
                long x = (long) nums[i] * nums[j] / (g * g);
                ans = Math.max(ans, x);
            }
        }

        return ans;
    }

    private long gcd(long a, long b) {
        while (b != 0) {
            long t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxPairStrength(vector<int>& nums) {
        int n = nums.size();
        long long ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                long long g = gcd(nums[i], nums[j]);
                long long x = 1LL * nums[i] * nums[j] / (g * g);
                ans = max(ans, x);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxPairStrength(nums []int) int64 {
	n := len(nums)
	var ans int64 = 0

	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			g := gcd(int64(nums[i]), int64(nums[j]))
			x := int64(nums[i]) * int64(nums[j]) / (g * g)
			ans = max(ans, x)
		}
	}

	return ans
}

func gcd(a, b int64) int64 {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function maxPairStrength(nums: number[]): number {
    const n = nums.length;
    let ans = 0;

    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const g = gcd(nums[i], nums[j]);
            const x = Math.floor((nums[i] * nums[j]) / (g * g));
            ans = Math.max(ans, x);
        }
    }

    return ans;
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        const t = a % b;
        a = b;
        b = t;
    }
    return a;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
