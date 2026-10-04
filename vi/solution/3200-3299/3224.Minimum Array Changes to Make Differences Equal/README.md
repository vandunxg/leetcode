---
comments: true
difficulty: Medium
rating: 1996
source: Biweekly Contest 135 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3224. Minimum Array Changes to Make Differences Equal](https://leetcode.com/problems/minimum-array-changes-to-make-differences-equal)

[中文文档](/solution/3200-3299/3224.Minimum%20Array%20Changes%20to%20Make%20Differences%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>, trong đó <code>n</code> là <strong>số chẵn</strong>, và một số nguyên <code>k</code>.</p>

<p>Bạn có thể thực hiện một số thay đổi trên mảng, trong đó mỗi thay đổi cho phép thay thế <strong>bất kỳ</strong> phần tử nào trong mảng bằng <strong>bất kỳ</strong> số nguyên nào trong khoảng từ <code>0</code> đến <code>k</code>.</p>

<p>Bạn cần thực hiện một số thay đổi (có thể không thực hiện thay đổi nào) sao cho mảng cuối cùng thỏa mãn điều kiện sau:</p>

<ul>
    <li>Tồn tại một số nguyên <code>X</code> sao cho <code>abs(a[i] - a[n - i - 1]) = X</code> với mọi <code>(0 &lt;= i &lt; n)</code>.</li>
</ul>

<p>Trả về số lượng thay đổi <strong>ít nhất</strong> cần thực hiện để thỏa mãn điều kiện trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1,2,4,3], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể thực hiện các thay đổi sau:</p>

<ul>
    <li>Thay <code>nums[1]</code> bằng 2. Mảng thu được là <code>nums = [1,<u><strong>2</strong></u>,1,2,4,3]</code>.</li>
    <li>Thay <code>nums[3]</code> bằng 3. Mảng thu được là <code>nums = [1,2,1,<u><strong>3</strong></u>,4,3]</code>.</li>
</ul>

<p>Số nguyên <code>X</code> sẽ bằng 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,2,3,3,6,5,4], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể thực hiện các thao tác sau:</p>

<ul>
    <li>Thay <code>nums[3]</code> bằng 0. Mảng thu được là <code>nums = [0,1,2,<u><strong>0</strong></u>,3,6,5,4]</code>.</li>
    <li>Thay <code>nums[4]</code> bằng 4. Mảng thu được là <code>nums = [0,1,2,0,<strong><u>4</u></strong>,6,5,4]</code>.</li>
</ul>

<p>Số nguyên <code>X</code> sẽ bằng 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>n</code> là số chẵn.</li>
    <li><code>0 &lt;= nums[i] &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp $(\textit{nums}[i],\textit{nums}[n-1-i])$ phải có cùng hiệu $s$, và mỗi giá trị có thể được đưa về $[0,k]$. Với $n,k\le 10^5$, việc tính lại chi phí cho mọi $s$ sẽ tốn $O(nk)$.
>
> Với một cặp $(x,y)$ ($x\le y$), chi phí theo $s$ là hằng số trên từng đoạn: bằng $0$ tại $s=y-x$, bằng $1$ cho đến $\max(y,k-x)$, và bằng $2$ sau đó. Mảng hiệu ghi nhận mỗi đoạn của từng cặp đúng một lần; giá trị nhỏ nhất của tổng tiền tố là đáp án tốt nhất.

<!-- thinking:end -->

Giả sử trong mảng cuối cùng, hiệu giữa cặp $\textit{nums}[i]$ và $\textit{nums}[n-i-1]$ là $s$.

Gọi $x$ là giá trị nhỏ hơn giữa $\textit{nums}[i]$ và $\textit{nums}[n-i-1]$, còn $y$ là giá trị lớn hơn.

Với mỗi cặp số, ta có các trường hợp sau:

- Nếu không cần thay đổi, thì $y - x = s$.
- Nếu thực hiện một thay đổi, thì $s \le \max(y, k - x)$, trong đó giá trị lớn nhất đạt được bằng cách thay $x$ thành $0$ hoặc thay $y$ thành $k$.
- Nếu thực hiện hai thay đổi, thì $s > \max(y, k - x)$.

Cụ thể:

- Trong đoạn $[0, y-x-1]$, cần $1$ thay đổi.
- Tại $[y-x]$, không cần thay đổi.
- Trong đoạn $[y-x+1, \max(y, k-x)]$, cần $1$ thay đổi.
- Trong đoạn $[\max(y, k-x)+1, k]$, cần $2$ thay đổi.

Ta duyệt qua từng cặp số và dùng một mảng hiệu để cập nhật số thay đổi cần thiết trên các đoạn khác nhau ứng với từng cặp.

Cuối cùng, ta tìm giá trị nhỏ nhất trong các tổng tiền tố của mảng hiệu; đó là số thay đổi nhỏ nhất cần thực hiện.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

Các bài toán tương tự:

- [1674. Minimum Moves to Make Array Complementary](https://github.com/doocs/leetcode/tree/main/solution/1600-1699/1674.Minimum%20Moves%20to%20Make%20Array%20Complementary/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minChanges(self, nums: List[int], k: int) -> int:
        d = [0] * (k + 2)
        n = len(nums)
        for i in range(n // 2):
            x, y = nums[i], nums[-i - 1]
            if x > y:
                x, y = y, x
            d[0] += 1
            d[y - x] -= 1
            d[y - x + 1] += 1
            d[max(y, k - x) + 1] -= 1
            d[max(y, k - x) + 1] += 2
        return min(accumulate(d))
```

#### Java

```java
class Solution {
    public int minChanges(int[] nums, int k) {
        int[] d = new int[k + 2];
        int n = nums.length;
        for (int i = 0; i < n / 2; ++i) {
            int x = Math.min(nums[i], nums[n - i - 1]);
            int y = Math.max(nums[i], nums[n - i - 1]);
            d[0] += 1;
            d[y - x] -= 1;
            d[y - x + 1] += 1;
            d[Math.max(y, k - x) + 1] -= 1;
            d[Math.max(y, k - x) + 1] += 2;
        }
        int ans = n, s = 0;
        for (int x : d) {
            s += x;
            ans = Math.min(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minChanges(vector<int>& nums, int k) {
        int d[k + 2];
        memset(d, 0, sizeof(d));
        int n = nums.size();
        for (int i = 0; i < n / 2; ++i) {
            int x = min(nums[i], nums[n - i - 1]);
            int y = max(nums[i], nums[n - i - 1]);
            d[0] += 1;
            d[y - x] -= 1;
            d[y - x + 1] += 1;
            d[max(y, k - x) + 1] -= 1;
            d[max(y, k - x) + 1] += 2;
        }
        int ans = n, s = 0;
        for (int x : d) {
            s += x;
            ans = min(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func minChanges(nums []int, k int) int {
    d := make([]int, k+2)
    n := len(nums)
    for i := 0; i < n/2; i++ {
        x, y := nums[i], nums[n-1-i]
        if x > y {
            x, y = y, x
        }
        d[0] += 1
        d[y-x] -= 1
        d[y-x+1] += 1
        d[max(y, k-x)+1] -= 1
        d[max(y, k-x)+1] += 2
    }
    ans, s := n, 0
    for _, x := range d {
        s += x
        ans = min(ans, s)
    }
    return ans
}
```

#### TypeScript

```ts
function minChanges(nums: number[], k: number): number {
    const d: number[] = Array(k + 2).fill(0);
    const n = nums.length;
    for (let i = 0; i < n >> 1; ++i) {
        const x = Math.min(nums[i], nums[n - 1 - i]);
        const y = Math.max(nums[i], nums[n - 1 - i]);
        d[0] += 1;
        d[y - x] -= 1;
        d[y - x + 1] += 1;
        d[Math.max(y, k - x) + 1] -= 1;
        d[Math.max(y, k - x) + 1] += 2;
    }
    let [ans, s] = [n, 0];
    for (const x of d) {
        s += x;
        ans = Math.min(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_changes(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut d = vec![0; (k + 2) as usize];
        for i in 0..n / 2 {
            let x = nums[i].min(nums[n - i - 1]);
            let y = nums[i].max(nums[n - i - 1]);
            d[0] += 1;
            d[(y - x) as usize] -= 1;
            d[(y - x + 1) as usize] += 1;
            let idx = (y.max(k - x) + 1) as usize;
            d[idx] -= 1;
            d[idx] += 2;
        }
        let mut ans = n as i32;
        let mut s = 0;
        for x in d {
            s += x;
            ans = ans.min(s);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
