---
comments: true
difficulty: Medium
rating: 1663
source: Weekly Contest 404 Q2
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3201. Find the Maximum Length of Valid Subsequence I](https://leetcode.com/problems/find-the-maximum-length-of-valid-subsequence-i)

[中文文档](/solution/3200-3299/3201.Find%20the%20Maximum%20Length%20of%20Valid%20Subsequence%20I/README.md)

## Mô tả

<!-- description:start -->

Bạn được cho một mảng số nguyên <code>nums</code>.
<p>Một <span data-keyword="subsequence-array">dãy con</span> <code>sub</code> của <code>nums</code> có độ dài <code>x</code> được gọi là <strong>hợp lệ</strong> nếu thỏa mãn:</p>

<ul>
    <li><code>(sub[0] + sub[1]) % 2 == (sub[1] + sub[2]) % 2 == ... == (sub[x - 2] + sub[x - 1]) % 2.</code></li>
</ul>

<p>Trả về độ dài của dãy con <strong>hợp lệ</strong> <strong>dài nhất</strong> của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ dài nhất là <code>[1, 2, 3, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,1,2,1,2]</span></p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ dài nhất là <code>[1, 2, 1, 2, 1, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con hợp lệ dài nhất là <code>[1, 3]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tính hợp lệ chỉ ràng buộc tổng của các cặp phần tử liên tiếp theo modulo $k$. Ở đây $k=2$ và $n\le 2\times 10^5$, nên việc liệt kê các dãy con hoặc duyệt lại mọi phần tử đứng trước là quá chậm.
>
> $(a+b)\equiv(b+c)\pmod 2$ suy ra $a\equiv c\pmod 2$: các vị trí lẻ có cùng một phần dư, và các vị trí chẵn cũng vậy. Do đó, hình dạng của một dãy hợp lệ được xác định bởi hai phần dư cuối cùng $(x,y)$. Ta duy trì $f[x][y]$ là độ dài lớn nhất của dãy kết thúc bằng phần dư $x$ sau khi phần dư trước đó là $y$. Với mỗi $x$, ta duyệt tổng cặp mục tiêu $j$, tính ngược $y=(j-x)\bmod 2$, rồi chuyển trạng thái từ $f[y][x]$. Chỉ cần một lượt duyệt tuyến tính.

<!-- thinking:end -->

Ta đặt $k = 2$.

Dựa trên mô tả bài toán, ta biết rằng với một dãy con $a_1, a_2, a_3, \cdots, a_x$, nếu dãy thỏa mãn $(a_1 + a_2) \bmod k = (a_2 + a_3) \bmod k$, thì $a_1 \bmod k = a_3 \bmod k$. Điều này có nghĩa là kết quả của phép lấy modulo $k$ của tất cả các phần tử ở vị trí lẻ là như nhau, và kết quả của tất cả các phần tử ở vị trí chẵn cũng vậy.

Ta có thể giải bài toán này bằng quy hoạch động. Định nghĩa trạng thái $f[x][y]$ là độ dài của dãy con hợp lệ dài nhất trong đó phần tử cuối cùng có modulo $k$ bằng $x$, và phần tử áp chót có modulo $k$ bằng $y$. Ban đầu, $f[x][y] = 0$.

Duyệt qua mảng $nums$, với mỗi số $x$, ta lấy $x = x \bmod k$. Sau đó, ta có thể duyệt các dãy mà modulo của tổng hai số liên tiếp cho cùng một kết quả $j$, trong đó $j \in [0, k)$. Khi đó, modulo $k$ của số trước đó sẽ là $y = (j - x + k) \bmod k$. Lúc này, $f[x][y] = f[y][x] + 1$.

Đáp án là giá trị lớn nhất trong tất cả các $f[x][y]$.

Độ phức tạp thời gian là $O(n \times k)$, và độ phức tạp không gian là $O(k^2)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, và $k=2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, nums: List[int]) -> int:
        k = 2
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
    public int maximumLength(int[] nums) {
        int k = 2;
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
    int maximumLength(vector<int>& nums) {
        int k = 2;
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
func maximumLength(nums []int) (ans int) {
    k := 2
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
function maximumLength(nums: number[]): number {
    const k = 2;
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
    pub fn maximum_length(nums: Vec<i32>) -> i32 {
        let mut f = [[0; 2]; 2];
        let mut ans = 0;
        for x in nums {
            let x = (x % 2) as usize;
            for j in 0..2 {
                let y = ((j + 2 - x) % 2) as usize;
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
    public int MaximumLength(int[] nums) {
        int k = 2;
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
