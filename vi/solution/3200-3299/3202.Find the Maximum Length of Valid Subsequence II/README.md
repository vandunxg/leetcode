---
comments: true
difficulty: Medium
rating: 1973
source: Weekly Contest 404 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3202. Find the Maximum Length of Valid Subsequence II](https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-ii)

[中文文档](/solution/3200-3299/3202.Find%20the%20Maximum%20Length%20of%20Valid%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.
<p>Một <span data-keyword="subsequence-array">dãy con</span> <code>sub</code> của <code>nums</code> có độ dài <code>x</code> được gọi là <strong>hợp lệ</strong> nếu thỏa mãn:</p>

<ul>
    <li><code>(sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k.</code></li>
</ul>

Trả về độ dài của dãy con <strong>hợp lệ</strong> <strong>dài nhất</strong> của <code>nums</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ dài nhất là <code>[1, 2, 3, 4, 5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,3,1,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ dài nhất là <code>[1, 4, 1, 4]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 10<sup>3</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phép modulo hiện có một $k\le 10^3$ bất kỳ với $n\le 10^3$. Việc liệt kê các dãy con vẫn không khả thi, nhưng tổng của hai phần tử kề nhau bằng nhau vẫn buộc các vị trí lẻ có cùng phần dư và các vị trí chẵn có cùng phần dư, nên trạng thái vẫn chỉ cần hai phần dư cuối cùng.
>
> Với $n\times k\le 10^6$, ta có thể, với mỗi giá trị, liệt kê cặp tổng mục tiêu $j\in[0,k)$, tìm lại $y=(j-x+k)\bmod k$, rồi gán $f[x][y]=f[y][x]+1$. Công thức truy hồi giống như trong trường hợp $k=2$; chỉ có độ dài cạnh của bảng thay đổi.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta biết rằng với một dãy con $a_1, a_2, a_3, \cdots, a_x$, nếu thỏa mãn $(a_1 + a_2) \bmod k = (a_2 + a_3) \bmod k$, thì $a_1 \bmod k = a_3 \bmod k$. Điều này có nghĩa là phần dư khi lấy modulo $k$ của tất cả các phần tử ở vị trí lẻ là như nhau, và phần dư của tất cả các phần tử ở vị trí chẵn cũng như nhau.

Ta có thể giải bài toán này bằng quy hoạch động. Định nghĩa trạng thái $f[x][y]$ là độ dài của dãy con hợp lệ dài nhất mà phần tử cuối cùng có phần dư modulo $k$ bằng $x$, còn phần tử kế cuối có phần dư modulo $k$ bằng $y$. Ban đầu, $f[x][y] = 0$.

Duyệt qua mảng $nums$, với mỗi số $x$, ta lấy $x = x \bmod k$. Sau đó, ta có thể liệt kê các dãy mà hai số liên tiếp modulo $j$ cho cùng một kết quả, với $j \in [0, k)$. Khi đó, phần dư modulo $k$ của số đứng trước là $y = (j - x + k) \bmod k$. Lúc này, $f[x][y] = f[y][x] + 1$.

Đáp án là giá trị lớn nhất trong tất cả $f[x][y]$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(k^2)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $k$ là số nguyên dương được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        f = [[0] * k for _ in range(k)]
        ans = 0
        for x in nums:
            x %= k
            for j in range(k):
                y = (j - x + k) % k
                f[x][y] = f[y][x] + 1
                ans = max(ans, f[x][y])
        return ans
```

#### Java

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int[][] f = new int[k][k];
        int ans = 0;
        for (int x : nums) {
            x %= k;
            for (int j = 0; j < k; ++j) {
                int y = (j - x + k) % k;
                f[x][y] = f[y][x] + 1;
                ans = Math.max(ans, f[x][y]);
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
    int maximumLength(vector<int>& nums, int k) {
        int f[k][k];
        memset(f, 0, sizeof(f));
        int ans = 0;
        for (int x : nums) {
            x %= k;
            for (int j = 0; j < k; ++j) {
                int y = (j - x + k) % k;
                f[x][y] = f[y][x] + 1;
                ans = max(ans, f[x][y]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumLength(nums []int, k int) (ans int) {
    f := make([][]int, k)
    for i := range f {
        f[i] = make([]int, k)
    }
    for _, x := range nums {
        x %= k
        for j := 0; j < k; j++ {
            y := (j - x + k) % k
            f[x][y] = f[y][x] + 1
            ans = max(ans, f[x][y])
        }
    }
    return
}
```

#### TypeScript

```ts
function maximumLength(nums: number[], k: number): number {
    const f: number[][] = Array.from({ length: k }, () => Array(k).fill(0));
    let ans: number = 0;
    for (let x of nums) {
        x %= k;
        for (let j = 0; j < k; ++j) {
            const y = (j - x + k) % k;
            f[x][y] = f[y][x] + 1;
            ans = Math.max(ans, f[x][y]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_length(nums: Vec<i32>, k: i32) -> i32 {
        let k = k as usize;
        let mut f = vec![vec![0; k]; k];
        let mut ans = 0;
        for x in nums {
            let x = (x % k as i32) as usize;
            for j in 0..k {
                let y = (j + k - x) % k;
                f[x][y] = f[y][x] + 1;
                ans = ans.max(f[x][y]);
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaximumLength(int[] nums, int k) {
        int[,] f = new int[k, k];
        int ans = 0;
        foreach (int num in nums) {
            int x = num % k;
            for (int j = 0; j < k; ++j) {
                int y = (j - x + k) % k;
                f[x, y] = f[y, x] + 1;
                ans = Math.Max(ans, f[x, y]);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
