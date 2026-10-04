---
comments: true
difficulty: Medium
rating: 2008
source: Weekly Contest 446 Q3
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3524. Find X Value of Array I](https://leetcode.com/problems/find-x-value-of-array-i)

[中文文档](/solution/3500-3599/3524.Find%20X%20Value%20of%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Bạn được phép thực hiện một thao tác <strong>một lần</strong> trên <code>nums</code>, trong đó ở mỗi thao tác, bạn có thể xóa một tiền tố và một hậu tố <strong>không giao nhau</strong> bất kỳ khỏi <code>nums</code>, sao cho <code>nums</code> vẫn <strong>không rỗng</strong>.</p>

<p>Hãy tìm <strong>giá trị x</strong> của <code>nums</code>, là số cách thực hiện thao tác này sao cho <strong>tích</strong> của các phần tử còn lại có <em>số dư</em> là <code>x</code> khi chia cho <code>k</code>.</p>

<p>Trả về một mảng <code>result</code> có kích thước <code>k</code>, trong đó <code>result[x]</code> là <strong>giá trị x</strong> của <code>nums</code> với <code>0 &lt;= x &lt;= k - 1</code>.</p>

<p><strong>Tiền tố</strong> của một mảng là một <span data-keyword="subarray">mảng con</span> bắt đầu từ đầu mảng và kéo dài đến bất kỳ vị trí nào trong mảng.</p>

<p><strong>Hậu tố</strong> của một mảng là một <span data-keyword="subarray">mảng con</span> bắt đầu tại bất kỳ vị trí nào trong mảng và kéo dài đến cuối mảng.</p>

<p><strong>Lưu ý</strong> rằng tiền tố và hậu tố được chọn để thực hiện thao tác có thể là <strong>rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[9,2,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với <code>x = 0</code>, các thao tác khả dĩ bao gồm mọi cách xóa tiền tố/hậu tố không giao nhau mà không xóa <code>nums[2] == 3</code>.</li>
    <li>Với <code>x = 1</code>, các thao tác khả dĩ là:
    <ul>
        <li>Xóa tiền tố rỗng và hậu tố <code>[2, 3, 4, 5]</code>. Khi đó <code>nums</code> trở thành <code>[1]</code>.</li>
        <li>Xóa tiền tố <code>[1, 2, 3]</code> và hậu tố <code>[5]</code>. Khi đó <code>nums</code> trở thành <code>[4]</code>.</li>
    </ul>
    </li>
    <li>Với <code>x = 2</code>, các thao tác khả dĩ là:
    <ul>
        <li>Xóa tiền tố rỗng và hậu tố <code>[3, 4, 5]</code>. Khi đó <code>nums</code> trở thành <code>[1, 2]</code>.</li>
        <li>Xóa tiền tố <code>[1]</code> và hậu tố <code>[3, 4, 5]</code>. Khi đó <code>nums</code> trở thành <code>[2]</code>.</li>
        <li>Xóa tiền tố <code>[1, 2, 3]</code> và hậu tố rỗng. Khi đó <code>nums</code> trở thành <code>[4, 5]</code>.</li>
        <li>Xóa tiền tố <code>[1, 2, 3, 4]</code> và hậu tố rỗng. Khi đó <code>nums</code> trở thành <code>[5]</code>.</li>
    </ul>
    </li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,8,16,32], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[18,1,2,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với <code>x = 0</code>, các thao tác duy nhất <strong>không</strong> cho kết quả <code>x = 0</code> là:

    <ul>
    <li>Xóa tiền tố rỗng và hậu tố <code>[4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[1, 2]</code>.</li>
    <li>Xóa tiền tố rỗng và hậu tố <code>[2, 4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[1]</code>.</li>
    <li>Xóa tiền tố <code>[1]</code> và hậu tố <code>[4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[2]</code>.</li>
    </ul>
    </li>
    <li>Với <code>x = 1</code>, thao tác khả dĩ duy nhất là:
    <ul>
    <li>Xóa tiền tố rỗng và hậu tố <code>[2, 4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[1]</code>.</li>
    </ul>
    </li>
    <li>Với <code>x = 2</code>, các thao tác khả dĩ là:
    <ul>
    <li>Xóa tiền tố rỗng và hậu tố <code>[4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[1, 2]</code>.</li>
    <li>Xóa tiền tố <code>[1]</code> và hậu tố <code>[4, 8, 16, 32]</code>. Khi đó <code>nums</code> trở thành <code>[2]</code>.</li>
    </ul>
    </li>
    <li>Với <code>x = 3</code>, không có cách nào thực hiện thao tác.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,1,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[9,6]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Việc xóa một tiền tố và một hậu tố để lại một mảng con; ta cần đếm có bao nhiêu mảng con có tích $\equiv x \pmod k$. Vì $n \le 10^5$ và $k \le 5$, dùng DP trên các phần dư thay cho việc liệt kê.
>
> Gọi $f[i][r]$ là số mảng con kết thúc tại $i$ có tích bằng $r$ modulo $k$. Chuyển trạng thái từ $f[i-1]$ bằng cách nhân với $nums[i]$, đồng thời bắt đầu một mảng con mới tại $i$. Cộng theo phần dư sẽ điền vào $\textit{result}$.

<!-- thinking:end -->

Sau khi xóa một tiền tố và một hậu tố không giao nhau bất kỳ, phần còn lại là một mảng con không rỗng. Bài toán yêu cầu đếm các mảng con có tích modulo $k$ lần lượt bằng $0, 1, \ldots, k-1$.

Gọi $f[r]$ là số mảng con kết thúc tại chỉ số hiện tại có tích bằng $r$ modulo $k$. Duyệt $x = \textit{nums}[i]$ từ trái sang phải và dùng $g$ làm trạng thái mới kết thúc tại $i$: chuyển mỗi $f[r]$ sang $g[(r \times x) \bmod k]$, sau đó thêm mảng con chỉ gồm một phần tử $[x]$ vào $g[x \bmod k]$. Cộng $g$ vào đáp án rồi gán $f \leftarrow g$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultArray(self, nums: list[int], k: int) -> list[int]:
        ans = [0] * k
        f = [0] * k
        for x in nums:
            g = [0] * k
            for r, cnt in enumerate(f):
                g[r * x % k] += cnt
            g[x % k] += 1
            for r, cnt in enumerate(g):
                ans[r] += cnt
            f = g
        return ans
```

#### Java

```java
class Solution {
    public long[] resultArray(int[] nums, int k) {
        long[] ans = new long[k];
        long[] f = new long[k];
        for (int x : nums) {
            long[] g = new long[k];
            for (int r = 0; r < k; ++r) {
                g[(int) (1L * r * x % k)] += f[r];
            }
            g[x % k] += 1;
            for (int r = 0; r < k; ++r) {
                ans[r] += g[r];
            }
            f = g;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> resultArray(vector<int>& nums, int k) {
        vector<long long> ans(k);
        vector<long long> f(k);
        for (int x : nums) {
            vector<long long> g(k);
            for (int r = 0; r < k; ++r) {
                g[1LL * r * x % k] += f[r];
            }
            g[x % k] += 1;
            for (int r = 0; r < k; ++r) {
                ans[r] += g[r];
            }
            f.swap(g);
        }
        return ans;
    }
};
```

#### Go

```go
func resultArray(nums []int, k int) []int64 {
    ans := make([]int64, k)
    f := make([]int64, k)
    for _, x := range nums {
        g := make([]int64, k)
        for r, cnt := range f {
            g[r*x%k] += cnt
        }
        g[x%k]++
        for r, cnt := range g {
            ans[r] += cnt
        }
        f = g
    }
    return ans
}
```

#### TypeScript

```ts
function resultArray(nums: number[], k: number): number[] {
    const ans = Array(k).fill(0);
    let f = Array(k).fill(0);
    for (const x of nums) {
        const g = Array(k).fill(0);
        for (let r = 0; r < k; ++r) {
            g[(r * x) % k] += f[r];
        }
        g[x % k] += 1;
        for (let r = 0; r < k; ++r) {
            ans[r] += g[r];
        }
        f = g;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
