---
comments: true
difficulty: Easy
rating: 1199
source: Weekly Contest 434 Q1
tags:
    - Array
    - Math
    - Prefix Sum
---

<!-- problem:start -->

# [3432. Count Partitions with Even Sum Difference](https://leetcode.com/problems/count-partitions-with-even-sum-difference)

[中文文档](/solution/3400-3499/3432.Count%20Partitions%20with%20Even%20Sum%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Một <strong>cách chia</strong> được định nghĩa là một chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n - 1</code>, chia mảng thành hai mảng con <strong>khác rỗng</strong> sao cho:</p>

<ul>
    <li>Mảng con bên trái chứa các chỉ số <code>[0, i]</code>.</li>
    <li>Mảng con bên phải chứa các chỉ số <code>[i + 1, n - 1]</code>.</li>
</ul>

<p>Trả về số <strong>cách chia</strong> mà <strong>hiệu</strong> giữa <strong>tổng</strong> của mảng con bên trái và bên phải là <strong>số chẵn</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,10,3,7,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 4 cách chia:</p>

<ul>
    <li><code>[10]</code>, <code>[10, 3, 7, 6]</code> với hiệu tổng là <code>10 - 26 = -16</code>, là số chẵn.</li>
    <li><code>[10, 10]</code>, <code>[3, 7, 6]</code> với hiệu tổng là <code>20 - 16 = 4</code>, là số chẵn.</li>
    <li><code>[10, 10, 3]</code>, <code>[7, 6]</code> với hiệu tổng là <code>23 - 13 = 10</code>, là số chẵn.</li>
    <li><code>[10, 10, 3, 7]</code>, <code>[6]</code> với hiệu tổng là <code>30 - 6 = 24</code>, là số chẵn.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cách chia nào cho hiệu tổng là số chẵn.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi cách chia đều cho hiệu tổng là số chẵn.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == nums.length &lt;= 100</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm các vị trí cắt mà tổng bên trái và bên phải có hiệu là một số chẵn. Tính chẵn lẻ của hiệu được quyết định bởi hai tổng đang xét, nên không cần duyệt lại toàn bộ mảng.
>
> $l-r$ là số chẵn khi và chỉ khi $l$ và $r$ có cùng tính chẵn lẻ. Vì $l-r=2l-\textit{total}$, ta chỉ cần duy trì $l$ và $r$ khi di chuyển vị trí cắt.
>
> Chuyển lần lượt từng phần tử trong $n-1$ phần tử đầu tiên từ $r$ sang $l$, rồi đếm các vị trí cắt thỏa mãn $(l-r)\bmod 2=0$.

<!-- thinking:end -->

Ta dùng hai biến $l$ và $r$ lần lượt biểu diễn tổng của mảng con bên trái và mảng con bên phải. Ban đầu, $l = 0$ và $r = \sum_{i=0}^{n-1} \textit{nums}[i]$.

Tiếp theo, ta duyệt qua $n - 1$ phần tử đầu tiên. Mỗi lần, ta thêm phần tử hiện tại vào mảng con bên trái và trừ nó khỏi mảng con bên phải. Sau đó, ta kiểm tra xem $l - r$ có chẵn hay không. Nếu có, ta tăng đáp án lên một.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPartitions(self, nums: List[int]) -> int:
        l, r = 0, sum(nums)
        ans = 0
        for x in nums[:-1]:
            l += x
            r -= x
            ans += (l - r) % 2 == 0
        return ans
```

#### Java

```java
class Solution {
    public int countPartitions(int[] nums) {
        int l = 0, r = 0;
        for (int x : nums) {
            r += x;
        }
        int ans = 0;
        for (int i = 0; i < nums.length - 1; ++i) {
            l += nums[i];
            r -= nums[i];
            if ((l - r) % 2 == 0) {
                ++ans;
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
    int countPartitions(vector<int>& nums) {
        int l = 0, r = accumulate(nums.begin(), nums.end(), 0);
        int ans = 0;
        for (int i = 0; i < nums.size() - 1; ++i) {
            l += nums[i];
            r -= nums[i];
            if ((l - r) % 2 == 0) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPartitions(nums []int) (ans int) {
    l, r := 0, 0
    for _, x := range nums {
        r += x
    }
    for _, x := range nums[:len(nums)-1] {
        l += x
        r -= x
        if (l-r)%2 == 0 {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function countPartitions(nums: number[]): number {
    let l = 0;
    let r = nums.reduce((a, b) => a + b, 0);
    let ans = 0;
    for (const x of nums.slice(0, -1)) {
        l += x;
        r -= x;
        ans += (l - r) % 2 === 0 ? 1 : 0;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_partitions(nums: Vec<i32>) -> i32 {
        let mut l: i64 = 0;
        let mut r: i64 = nums.iter().map(|&x| x as i64).sum();
        let mut ans: i32 = 0;

        for &x in nums[..nums.len() - 1].iter() {
            l += x as i64;
            r -= x as i64;
            if (l - r) % 2 == 0 {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
