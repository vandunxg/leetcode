---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [312. Burst Balloons](https://leetcode.com/problems/burst-balloons)

[中文文档](/solution/0300-0399/0312.Burst%20Balloons/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho <code>n</code> quả bóng, đánh chỉ số từ <code>0</code> đến <code>n - 1</code>. Mỗi quả bóng có ghi một số, được biểu diễn bằng mảng <code>nums</code>. Nhiệm vụ của bạn là làm nổ tất cả các quả bóng.</p>

<p>Nếu làm nổ quả bóng thứ <code>i<sup>th</sup></code>, bạn nhận được <code>nums[i - 1] * nums[i] * nums[i + 1]</code> đồng. Nếu <code>i - 1</code> hoặc <code>i + 1</code> nằm ngoài mảng, hãy xem như vị trí đó có một quả bóng mang số <code>1</code>.</p>

<p>Hãy trả về <em>số đồng tối đa bạn có thể thu được khi chọn thứ tự làm nổ bóng một cách hợp lý</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,5,8]
<strong>Đầu ra:</strong> 167
<strong>Giải thích:</strong>
nums = [3,1,5,8] --&gt; [3,5,8] --&gt; [3,8] --&gt; [8] --&gt; []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5]
<strong>Đầu ra:</strong> 10
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự làm nổ bóng làm thay đổi các quả bóng kề bên, nên tìm kiếm theo từng thứ tự nổ sẽ duyệt lại cùng một khoảng mở nhiều lần. Thêm $1$ vào hai đầu giúp cố định các hệ số nhân ở biên.
>
> Nếu $k$ là quả bóng cuối cùng bị làm nổ trong khoảng mở $(i,j)$, hai phía không ảnh hưởng lẫn nhau và ta cộng thêm $arr[i]\cdot arr[k]\cdot arr[j]$. Gọi $f[i][j]$ là số đồng tối đa nhận được khi làm nổ hết bóng trong $(i,j)$. Ta điền bảng theo độ rộng tăng dần ($i$ giảm dần, $j$ tăng dần). Đáp án là $f[0][n+1]$.

<!-- thinking:end -->

Gọi độ dài mảng `nums` là $n$. Theo đề bài, ta có thể thêm $1$ vào hai đầu mảng `nums` và gọi mảng mới là `arr`.

Tiếp theo, ta định nghĩa $f[i][j]$ là số đồng tối đa nhận được khi làm nổ tất cả bóng trong đoạn $[i, j]$. Do đó, đáp án là $f[0][n+1]$.

Để tính $f[i][j]$, ta xét mọi vị trí $k$ trong đoạn $[i, j]$. Giả sử quả bóng ở vị trí $k$ là quả cuối cùng bị làm nổ, ta có công thức chuyển trạng thái sau:

$$
f[i][j] = \max(f[i][j], f[i][k] + f[k][j] + arr[i] \times arr[k] \times arr[j])
$$

Khi cài đặt, công thức chuyển trạng thái của $f[i][j]$ phụ thuộc vào $f[i][k]$ và $f[k][j]$, với $i < k < j$. Vì vậy, ta duyệt $i$ từ lớn xuống nhỏ và $j$ từ nhỏ lên lớn, để khi tính $f[i][j]$, các giá trị $f[i][k]$ và $f[k][j]$ đã được tính trước.

Cuối cùng, ta trả về $f[0][n+1]$.

Độ phức tạp thời gian là $O(n^3)$, độ phức tạp không gian là $O(n^2)$, với $n$ là độ dài mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        n = len(nums)
        arr = [1] + nums + [1]
        f = [[0] * (n + 2) for _ in range(n + 2)]
        for i in range(n - 1, -1, -1):
            for j in range(i + 2, n + 2):
                for k in range(i + 1, j):
                    f[i][j] = max(f[i][j], f[i][k] + f[k][j] + arr[i] * arr[k] * arr[j])
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n + 2];
        arr[0] = 1;
        arr[n + 1] = 1;
        System.arraycopy(nums, 0, arr, 1, n);
        int[][] f = new int[n + 2][n + 2];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 2; j <= n + 1; j++) {
                for (int k = i + 1; k < j; k++) {
                    f[i][j] = Math.max(f[i][j], f[i][k] + f[k][j] + arr[i] * arr[k] * arr[j]);
                }
            }
        }
        return f[0][n + 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        vector<int> arr(n + 2, 1);
        for (int i = 0; i < n; ++i) {
            arr[i + 1] = nums[i];
        }

        vector<vector<int>> f(n + 2, vector<int>(n + 2, 0));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 2; j <= n + 1; ++j) {
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = max(f[i][j], f[i][k] + f[k][j] + arr[i] * arr[k] * arr[j]);
                }
            }
        }
        return f[0][n + 1];
    }
};
```

#### Go

```go
func maxCoins(nums []int) int {
    n := len(nums)
    arr := make([]int, n+2)
    arr[0] = 1
    arr[n+1] = 1
    copy(arr[1:], nums)

    f := make([][]int, n+2)
    for i := range f {
        f[i] = make([]int, n+2)
    }

    for i := n - 1; i >= 0; i-- {
        for j := i + 2; j <= n+1; j++ {
            for k := i + 1; k < j; k++ {
                f[i][j] = max(f[i][j], f[i][k] + f[k][j] + arr[i]*arr[k]*arr[j])
            }
        }
    }

    return f[0][n+1]
}
```

#### TypeScript

```ts
function maxCoins(nums: number[]): number {
    const n = nums.length;
    const arr = Array(n + 2).fill(1);
    for (let i = 0; i < n; i++) {
        arr[i + 1] = nums[i];
    }

    const f: number[][] = Array.from({ length: n + 2 }, () => Array(n + 2).fill(0));
    for (let i = n - 1; i >= 0; i--) {
        for (let j = i + 2; j <= n + 1; j++) {
            for (let k = i + 1; k < j; k++) {
                f[i][j] = Math.max(f[i][j], f[i][k] + f[k][j] + arr[i] * arr[k] * arr[j]);
            }
        }
    }
    return f[0][n + 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn max_coins(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut arr = vec![1; n + 2];
        for i in 0..n {
            arr[i + 1] = nums[i];
        }

        let mut f = vec![vec![0; n + 2]; n + 2];
        for i in (0..n).rev() {
            for j in i + 2..n + 2 {
                for k in i + 1..j {
                    f[i][j] = f[i][j].max(f[i][k] + f[k][j] + arr[i] * arr[k] * arr[j]);
                }
            }
        }
        f[0][n + 1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
