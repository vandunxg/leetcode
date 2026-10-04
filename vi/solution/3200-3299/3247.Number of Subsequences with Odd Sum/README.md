---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3247. Number of Subsequences with Odd Sum 🔒](https://leetcode.com/problems/number-of-subsequences-with-odd-sum)

[中文文档](/solution/3200-3299/3247.Number%20of%20Subsequences%20with%20Odd%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>, hãy trả về số lượng <span data-keyword="subsequence-array">dãy con</span> có tổng các phần tử là số lẻ.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con có tổng là số lẻ là: <code>[<u><strong>1</strong></u>, 1, 1]</code>, <code>[1, <u><strong>1</strong></u>, 1],</code> <code>[1, 1, <u><strong>1</strong></u>]</code>, <code>[<u><strong>1, 1, 1</strong></u>]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con có tổng là số lẻ là: <code>[<u><strong>1</strong></u>, 2, 2]</code>, <code>[<u><strong>1, 2</strong></u>, 2],</code> <code>[<u><strong>1</strong></u>, 2, <b><u>2</u></b>]</code>, <code>[<u><strong>1, 2, 2</strong></u>]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các dãy con có tổng là số lẻ. Vì $n\le 10^5$, không thể liệt kê tất cả dãy con. Tính chẵn lẻ của tổng chỉ phụ thuộc vào số lượng số lẻ được chọn, nên chỉ cần hai trạng thái luân phiên.
>
> $f[0],f[1]$ lần lượt là số dãy con có tổng chẵn và tổng lẻ tính đến hiện tại. Khi gặp số lẻ, hai nhóm đổi trạng thái và thêm dãy con chỉ chứa phần tử đó; khi gặp số chẵn, mỗi nhóm có thể giữ nguyên hoặc nối thêm phần tử, đồng thời nhóm tổng chẵn được cộng thêm dãy con chỉ chứa phần tử đó. Đáp án là giá trị cuối cùng của nhóm tổng lẻ.

<!-- thinking:end -->

Ta định nghĩa $f[0]$ là số lượng dãy con có tổng chẵn tính đến hiện tại, và $f[1]$ là số lượng dãy con có tổng lẻ tính đến hiện tại. Ban đầu, $f[0] = 0$ và $f[1] = 0$.

Duyệt qua mảng $\textit{nums}$, với mỗi số $x$:

Nếu $x$ là số lẻ, quy tắc cập nhật cho $f[0]$ và $f[1]$ là:

$$
\begin{aligned}
f[0] & = (f[0] + f[1]) \bmod 10^9 + 7, \\
f[1] & = (f[0] + f[1] + 1) \bmod 10^9 + 7.
\end{aligned}
$$

Điều đó có nghĩa là số dãy con có tổng chẵn ở thời điểm hiện tại bằng số dãy con có tổng chẵn ở thời điểm trước cộng với số dãy con có tổng lẻ được nối thêm số hiện tại $x$; số dãy con có tổng lẻ ở thời điểm hiện tại bằng số dãy con có tổng chẵn ở thời điểm trước được nối thêm số hiện tại $x$ cộng với số dãy con có tổng lẻ ở thời điểm trước, cộng thêm một dãy con chỉ chứa số hiện tại $x$.

Nếu $x$ là số chẵn, quy tắc cập nhật cho $f[0]$ và $f[1]$ là:

$$
\begin{aligned}
f[0] & = (f[0] + f[0] + 1) \bmod 10^9 + 7, \\
f[1] & = (f[1] + f[1]) \bmod 10^9 + 7.
\end{aligned}
$$

Điều đó có nghĩa là số dãy con có tổng chẵn ở thời điểm hiện tại bằng số dãy con có tổng chẵn ở thời điểm trước cộng với số dãy con có tổng chẵn được nối thêm số hiện tại $x$, cộng thêm một dãy con chỉ chứa số hiện tại $x$; số dãy con có tổng lẻ ở thời điểm hiện tại bằng số dãy con có tổng lẻ được nối thêm số hiện tại $x$ cộng với số dãy con có tổng lẻ ở thời điểm trước.

Cuối cùng, trả về $f[1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsequenceCount(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        f = [0] * 2
        for x in nums:
            if x % 2:
                f[0], f[1] = (f[0] + f[1]) % mod, (f[0] + f[1] + 1) % mod
            else:
                f[0], f[1] = (f[0] + f[0] + 1) % mod, (f[1] + f[1]) % mod
        return f[1]
```

#### Java

```java
class Solution {
    public int subsequenceCount(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int[] f = new int[2];
        for (int x : nums) {
            int[] g = new int[2];
            if (x % 2 == 1) {
                g[0] = (f[0] + f[1]) % mod;
                g[1] = (f[0] + f[1] + 1) % mod;
            } else {
                g[0] = (f[0] + f[0] + 1) % mod;
                g[1] = (f[1] + f[1]) % mod;
            }
            f = g;
        }
        return f[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subsequenceCount(vector<int>& nums) {
        const int mod = 1e9 + 7;
        vector<int> f(2);
        for (int x : nums) {
            vector<int> g(2);
            if (x % 2 == 1) {
                g[0] = (f[0] + f[1]) % mod;
                g[1] = (f[0] + f[1] + 1) % mod;
            } else {
                g[0] = (f[0] + f[0] + 1) % mod;
                g[1] = (f[1] + f[1]) % mod;
            }
            f = g;
        }
        return f[1];
    }
};
```

#### Go

```go
func subsequenceCount(nums []int) int {
    mod := int(1e9 + 7)
    f := [2]int{}
    for _, x := range nums {
        g := [2]int{}
        if x%2 == 1 {
            g[0] = (f[0] + f[1]) % mod
            g[1] = (f[0] + f[1] + 1) % mod
        } else {
            g[0] = (f[0] + f[0] + 1) % mod
            g[1] = (f[1] + f[1]) % mod
        }
        f = g
    }
    return f[1]
}
```

#### TypeScript

```ts
function subsequenceCount(nums: number[]): number {
    const mod = 1e9 + 7;
    let f = [0, 0];
    for (const x of nums) {
        const g = [0, 0];
        if (x % 2 === 1) {
            g[0] = (f[0] + f[1]) % mod;
            g[1] = (f[0] + f[1] + 1) % mod;
        } else {
            g[0] = (f[0] + f[0] + 1) % mod;
            g[1] = (f[1] + f[1]) % mod;
        }
        f = g;
    }
    return f[1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
