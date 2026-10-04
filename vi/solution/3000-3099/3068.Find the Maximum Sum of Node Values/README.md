---
comments: true
difficulty: Hard
rating: 2267
source: Biweekly Contest 125 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Tree
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3068. Find the Maximum Sum of Node Values](https://leetcode.com/problems/find-the-maximum-sum-of-node-values)

[中文文档](/solution/3000-3099/3068.Find%20the%20Maximum%20Sum%20of%20Node%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cây <strong>vô hướng</strong> gồm <code>n</code> nút được đánh số từ <code>0</code> đến <code>n - 1</code>. Cho mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh nối nút <code>u<sub>i</sub></code> và nút <code>v<sub>i</sub></code> trong cây. Bạn cũng được cho một số nguyên <strong>dương</strong> <code>k</code> và một mảng số nguyên <strong>không âm</strong> <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums[i]</code> biểu diễn <strong>giá trị</strong> của nút được đánh số <code>i</code>.</p>

<p>Alice muốn tổng giá trị của các nút trong cây là <strong>lớn nhất</strong>. Để đạt được điều đó, Alice có thể thực hiện thao tác sau trên cây <strong>bất kỳ</strong> số lần nào (<strong>kể cả không lần nào</strong>):</p>

<ul>
    <li>Chọn một cạnh bất kỳ <code>[u, v]</code> nối hai nút <code>u</code> và <code>v</code>, rồi cập nhật giá trị của chúng như sau:

    <ul>
        <li><code>nums[u] = nums[u] XOR k</code></li>
        <li><code>nums[v] = nums[v] XOR k</code></li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em><strong>tổng</strong> <strong>giá trị</strong> <strong>lớn nhất</strong> mà Alice có thể đạt được bằng cách thực hiện thao tác trên <strong>bất kỳ</strong> số lần nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3068.Find%20the%20Maximum%20Sum%20of%20Node%20Values/images/screenshot-2023-11-10-012513.png" style="width: 300px; height: 277px;padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,2,1], k = 3, edges = [[0,1],[0,2]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Alice có thể đạt tổng lớn nhất là 6 bằng một thao tác duy nhất:
- Chọn cạnh [0,2]. nums[0] và nums[2] trở thành: 1 XOR 3 = 2, và mảng nums trở thành: [1,2,1] -&gt; [2,2,2].
Tổng giá trị là 2 + 2 + 2 = 6.
Có thể chứng minh rằng 6 là tổng lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3068.Find%20the%20Maximum%20Sum%20of%20Node%20Values/images/screenshot-2024-01-09-220017.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: .5rem; width: 300px; height: 239px;" />
<pre>
<strong>Đầu vào:</strong> nums = [2,3], k = 7, edges = [[0,1]]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Alice có thể đạt tổng lớn nhất là 9 bằng một thao tác duy nhất:
- Chọn cạnh [0,1]. nums[0] trở thành: 2 XOR 7 = 5 và nums[1] trở thành: 3 XOR 7 = 4, còn mảng nums trở thành: [2,3] -&gt; [5,4].
Tổng giá trị là 5 + 4 = 9.
Có thể chứng minh rằng 9 là tổng lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3068.Find%20the%20Maximum%20Sum%20of%20Node%20Values/images/screenshot-2023-11-10-012641.png" style="width: 600px; height: 233px;padding: 10px; background: #fff; border-radius: .5rem;" />
<pre>
<strong>Đầu vào:</strong> nums = [7,7,7,7,7,7], k = 3, edges = [[0,1],[0,2],[0,3],[0,4],[0,5]]
<strong>Đầu ra:</strong> 42
<strong>Giải thích:</strong> Tổng lớn nhất có thể đạt được là 42, tương ứng với việc Alice không thực hiện thao tác nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == nums.length &lt;= 2 * 10<sup>4</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>edges.length == n - 1</code></li>
    <li><code>edges[i].length == 2</code></li>
    <li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện XOR với $k$ trên mọi cạnh của một đường đi chỉ lật giá trị của hai đầu mút. Sau mọi dãy thao tác, số nút bị lật luôn là số chẵn.
>
> Các cạnh của cây có thể bỏ qua: chỉ cần chọn một số chẵn các giá trị để XOR với $k$ nhằm tối đa hóa tổng.
>
> $f_0,f_1$ lưu tổng lớn nhất sau khi lật số phần tử chẵn/lẻ. Mỗi $x$ có thể giữ nguyên hoặc trở thành $x \oplus k$, và ta trả về trạng thái chẵn.

<!-- thinking:end -->

Với một số $x$ bất kỳ, giá trị của nó không đổi sau khi được XOR với $k$ một số lần chẵn. Do đó, với bất kỳ đường đi nào trong cây, nếu thực hiện thao tác trên mọi cạnh của đường đi, giá trị của tất cả các nút trên đường đi, ngoại trừ hai nút đầu và cuối, sẽ không thay đổi.

Ngoài ra, bất kể thực hiện bao nhiêu thao tác, luôn có một số chẵn phần tử được XOR với $k$, còn các phần tử khác giữ nguyên.

Vì vậy, bài toán được chuyển thành: với mảng $\textit{nums}$, chọn một số chẵn phần tử để XOR với $k$ nhằm tối đa hóa tổng.

Ta có thể dùng quy hoạch động để giải bài toán này. Gọi $f_0$ là tổng lớn nhất khi có một số chẵn phần tử được XOR với $k$, và $f_1$ là tổng lớn nhất khi có một số lẻ phần tử được XOR với $k$. Các phương trình chuyển trạng thái là:

$$
\begin{aligned}
f_0 &= \max(f_0 + x, f_1 + (x \oplus k)) \\
f_1 &= \max(f_1 + x, f_0 + (x \oplus k))
\end{aligned}
$$

trong đó $x$ là giá trị của phần tử hiện tại.

Ta duyệt mảng $\textit{nums}$ và cập nhật $f_0$, $f_1$ theo các phương trình chuyển trạng thái trên. Cuối cùng, trả về $f_0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumValueSum(self, nums: List[int], k: int, edges: List[List[int]]) -> int:
        f0, f1 = 0, -inf
        for x in nums:
            f0, f1 = max(f0 + x, f1 + (x ^ k)), max(f1 + x, f0 + (x ^ k))
        return f0
```

#### Java

```java
class Solution {
    public long maximumValueSum(int[] nums, int k, int[][] edges) {
        long f0 = 0, f1 = -0x3f3f3f3f;
        for (int x : nums) {
            long tmp = f0;
            f0 = Math.max(f0 + x, f1 + (x ^ k));
            f1 = Math.max(f1 + x, tmp + (x ^ k));
        }
        return f0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumValueSum(vector<int>& nums, int k, vector<vector<int>>& edges) {
        long long f0 = 0, f1 = -0x3f3f3f3f;
        for (int x : nums) {
            long long tmp = f0;
            f0 = max(f0 + x, f1 + (x ^ k));
            f1 = max(f1 + x, tmp + (x ^ k));
        }
        return f0;
    }
};
```

#### Go

```go
func maximumValueSum(nums []int, k int, edges [][]int) int64 {
    f0, f1 := 0, -0x3f3f3f3f
    for _, x := range nums {
        f0, f1 = max(f0+x, f1+(x^k)), max(f1+x, f0+(x^k))
    }
    return int64(f0)
}
```

#### TypeScript

```ts
function maximumValueSum(nums: number[], k: number, edges: number[][]): number {
    let [f0, f1] = [0, -Infinity];
    for (const x of nums) {
        [f0, f1] = [Math.max(f0 + x, f1 + (x ^ k)), Math.max(f1 + x, f0 + (x ^ k))];
    }
    return f0;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_value_sum(nums: Vec<i32>, k: i32, edges: Vec<Vec<i32>>) -> i64 {
        let mut f0: i64 = 0;
        let mut f1: i64 = i64::MIN;

        for &x in &nums {
            let tmp = f0;
            f0 = std::cmp::max(f0 + x as i64, f1 + (x ^ k) as i64);
            f1 = std::cmp::max(f1 + x as i64, tmp + (x ^ k) as i64);
        }

        f0
    }
}
```

#### C#

```cs
public class Solution {
    public long MaximumValueSum(int[] nums, int k, int[][] edges) {
        long f0 = 0, f1 = -0x3f3f3f3f;
        foreach (int x in nums) {
            long tmp = f0;
            f0 = Math.Max(f0 + x, f1 + (x ^ k));
            f1 = Math.Max(f1 + x, tmp + (x ^ k));
        }
        return f0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
