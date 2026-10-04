---
comments: true
difficulty: Hard
rating: 2085
source: Weekly Contest 423 Q3
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3351. Sum of Good Subsequences](https://leetcode.com/problems/sum-of-good-subsequences)

[中文文档](/solution/3300-3399/3351.Sum%20of%20Good%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Một <strong>dãy con tốt </strong><span data-keyword="subsequence-array">dãy con</span> được định nghĩa là dãy con của <code>nums</code> trong đó độ chênh lệch tuyệt đối giữa mọi cặp phần tử <strong>liên tiếp</strong> trong dãy con bằng <strong>đúng</strong> 1.</p>

<p>Hãy trả về <strong>tổng</strong> của tất cả các <em>dãy con tốt</em> <strong>có thể</strong> tạo được từ <code>nums</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý </strong>rằng dãy con có kích thước 1 được xem là dãy con tốt theo định nghĩa.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các dãy con tốt là: <code>[1]</code>, <code>[2]</code>, <code>[1]</code>, <code>[1,2]</code>, <code>[2,1]</code>, <code>[1,2,1]</code>.</li>
    <li>Tổng các phần tử trong những dãy con này là 14.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">40</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các dãy con tốt là: <code>[3]</code>, <code>[4]</code>, <code>[5]</code>, <code>[3,4]</code>, <code>[4,5]</code>, <code>[3,4,5]</code>.</li>
    <li>Tổng các phần tử trong những dãy con này là 40.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con tốt có độ chênh lệch tuyệt đối giữa các phần tử liên tiếp bằng $1$. Với $n \le 10^5$, không thể liệt kê các dãy con; ta dùng quy hoạch động trên các giá trị.
>
> Đặt $g[x]$ là số dãy con tốt kết thúc bằng $x$, còn $f[x]$ là tổng các phần tử của chúng. Một $x$ mới tạo thành dãy đơn phần tử hoặc nối vào các dãy hiện có kết thúc bằng $x-1$ hoặc $x+1$.
>
> Việc nối thêm đóng góp “tổng cũ + số lượng cũ $\times x$”. Các cập nhật tuân theo thứ tự của mảng đầu vào nên các bản sao $x$ xuất hiện trước đó được tính đến. Đáp án là tổng của mọi $f$, lấy modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfGoodSubsequences(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        f = defaultdict(int)
        g = defaultdict(int)
        for x in nums:
            f[x] += x
            g[x] += 1
            f[x] += f[x - 1] + g[x - 1] * x
            g[x] += g[x - 1]
            f[x] += f[x + 1] + g[x + 1] * x
            g[x] += g[x + 1]
        return sum(f.values()) % mod
```

#### Java

```java
class Solution {
    public int sumOfGoodSubsequences(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        long[] f = new long[mx + 1];
        long[] g = new long[mx + 1];
        for (int x : nums) {
            f[x] += x;
            g[x] += 1;
            if (x > 0) {
                f[x] = (f[x] + f[x - 1] + g[x - 1] * x % mod) % mod;
                g[x] = (g[x] + g[x - 1]) % mod;
            }
            if (x + 1 <= mx) {
                f[x] = (f[x] + f[x + 1] + g[x + 1] * x % mod) % mod;
                g[x] = (g[x] + g[x + 1]) % mod;
            }
        }
        long ans = 0;
        for (long x : f) {
            ans = (ans + x) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfGoodSubsequences(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int mx = ranges::max(nums);

        vector<long long> f(mx + 1), g(mx + 1);
        for (int x : nums) {
            f[x] += x;
            g[x] += 1;

            if (x > 0) {
                f[x] = (f[x] + f[x - 1] + g[x - 1] * x % mod) % mod;
                g[x] = (g[x] + g[x - 1]) % mod;
            }

            if (x + 1 <= mx) {
                f[x] = (f[x] + f[x + 1] + g[x + 1] * x % mod) % mod;
                g[x] = (g[x] + g[x + 1]) % mod;
            }
        }

        return accumulate(f.begin(), f.end(), 0LL) % mod;
    }
};
```

#### Go

```go
func sumOfGoodSubsequences(nums []int) (ans int) {
    mod := int(1e9 + 7)
    mx := slices.Max(nums)

    f := make([]int, mx+1)
    g := make([]int, mx+1)

    for _, x := range nums {
        f[x] += x
        g[x] += 1

        if x > 0 {
            f[x] = (f[x] + f[x-1] + g[x-1]*x%mod) % mod
            g[x] = (g[x] + g[x-1]) % mod
        }

        if x+1 <= mx {
            f[x] = (f[x] + f[x+1] + g[x+1]*x%mod) % mod
            g[x] = (g[x] + g[x+1]) % mod
        }
    }

    for _, x := range f {
        ans = (ans + x) % mod
    }
    return
}
```

#### TypeScript

```ts
function sumOfGoodSubsequences(nums: number[]): number {
    const mod = 10 ** 9 + 7;
    const mx = Math.max(...nums);
    const f: number[] = Array(mx + 1).fill(0);
    const g: number[] = Array(mx + 1).fill(0);
    for (const x of nums) {
        f[x] += x;
        g[x] += 1;
        if (x > 0) {
            f[x] = (f[x] + f[x - 1] + ((g[x - 1] * x) % mod)) % mod;
            g[x] = (g[x] + g[x - 1]) % mod;
        }
        if (x + 1 <= mx) {
            f[x] = (f[x] + f[x + 1] + ((g[x + 1] * x) % mod)) % mod;
            g[x] = (g[x] + g[x + 1]) % mod;
        }
    }
    return f.reduce((acc, cur) => (acc + cur) % mod, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
