---
comments: true
difficulty: Easy
rating: 1263
source: Weekly Contest 461 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3637. Trionic Array I](https://leetcode.com/problems/trionic-array-i)

[中文文档](/solution/3600-3699/3637.Trionic%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p data-end="128" data-start="0">Bạn được cho một mảng số nguyên <code data-end="37" data-start="31">nums</code> có độ dài <code data-end="51" data-start="48">n</code>.</p>

<p data-end="128" data-start="0">Một mảng được gọi là <strong data-end="76" data-start="65">trionic</strong> nếu tồn tại các chỉ số <code data-end="117" data-start="100">0 &lt; p &lt; q &lt; n &minus; 1</code> sao cho:</p>

<ul>
    <li data-end="170" data-start="132"><code data-end="144" data-start="132">nums[0...p]</code> tăng dần <strong>nghiêm ngặt</strong>,</li>
    <li data-end="211" data-start="173"><code data-end="185" data-start="173">nums[p...q]</code> giảm dần <strong>nghiêm ngặt</strong>,</li>
    <li data-end="252" data-start="214"><code data-end="228" data-start="214">nums[q...n &minus; 1]</code> tăng dần <strong>nghiêm ngặt</strong>.</li>
</ul>

<p data-end="315" data-is-last-node="" data-is-only-node="" data-start="254">Trả về <code data-end="267" data-start="261">true</code> nếu <code data-end="277" data-start="271">nums</code> là trionic, ngược lại trả về <code data-end="314" data-start="307">false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,5,4,2,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chọn <code data-end="91" data-start="84">p = 2</code>, <code data-end="100" data-start="93">q = 4</code>:</p>

<ul>
    <li><code data-end="130" data-start="108">nums[0...2] = [1, 3, 5]</code> tăng dần nghiêm ngặt (<code data-end="166" data-start="155">1 &lt; 3 &lt; 5</code>).</li>
    <li><code data-end="197" data-start="175">nums[2...4] = [5, 4, 2]</code> giảm dần nghiêm ngặt (<code data-end="233" data-start="222">5 &gt; 4 &gt; 2</code>).</li>
    <li><code data-end="262" data-start="242">nums[4...5] = [2, 6]</code> tăng dần nghiêm ngặt (<code data-end="294" data-start="287">2 &lt; 6</code>).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cách nào chọn <code>p</code> và <code>q</code> để tạo thành ba đoạn cần thiết.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="41" data-start="26"><code data-end="39" data-start="26">3 &lt;= n &lt;= 100</code></li>
    <li data-end="70" data-start="44"><code data-end="70" data-start="44">-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng trionic gồm một đoạn tăng dần nghiêm ngặt không rỗng, một đoạn giảm dần nghiêm ngặt không rỗng và một đoạn tăng dần nghiêm ngặt không rỗng khác. Việc duyệt qua ba đoạn đơn giản hơn so với liệt kê hai điểm đổi chiều.
>
> Con trỏ $p$ duyệt hết đoạn tăng đầu tiên; nếu dừng ngay đầu mảng thì thất bại. Sau đó, $q$ duyệt đoạn giảm; nếu không di chuyển hoặc kết thúc ở cuối mảng thì đoạn giữa hoặc đoạn cuối bị thiếu.
>
> Đoạn tăng cuối cùng phải kết thúc đúng tại $n-1$. Một lượt duyệt kiểm tra sự tồn tại và tính nghiêm ngặt của cả ba phần.

<!-- thinking:end -->

Trước tiên, ta định nghĩa một con trỏ $p$, ban đầu $p = 0$, trỏ đến phần tử đầu tiên của mảng. Ta di chuyển $p$ sang phải cho đến khi gặp phần tử đầu tiên không thỏa mãn thứ tự tăng dần nghiêm ngặt, tức là $nums[p] \geq nums[p + 1]$. Nếu lúc này $p = 0$, điều đó có nghĩa là phần đầu của mảng không có đoạn tăng dần nghiêm ngặt, nên ta trả về $\text{false}$ ngay.

Tiếp theo, ta định nghĩa một con trỏ khác $q$, ban đầu $q = p$, trỏ đến phần tử đầu tiên của phần thứ hai trong mảng. Ta di chuyển $q$ sang phải cho đến khi gặp phần tử đầu tiên không thỏa mãn thứ tự giảm dần nghiêm ngặt, tức là $nums[q] \leq nums[q + 1]$. Nếu lúc này $q = p$ hoặc $q = n - 1$, điều đó có nghĩa là phần thứ hai của mảng không có đoạn giảm dần nghiêm ngặt hoặc không có phần thứ ba, nên ta trả về $\text{false}$ ngay.

Nếu tất cả điều kiện trên đều được thỏa mãn, điều đó có nghĩa là mảng là trionic, và ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$, chỉ sử dụng thêm không gian hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isTrionic(self, nums: List[int]) -> bool:
        n = len(nums)
        p = 0
        while p < n - 2 and nums[p] < nums[p + 1]:
            p += 1
        if p == 0:
            return False
        q = p
        while q < n - 1 and nums[q] > nums[q + 1]:
            q += 1
        if q == p or q == n - 1:
            return False
        while q < n - 1 and nums[q] < nums[q + 1]:
            q += 1
        return q == n - 1
```

#### Java

```java
class Solution {
    public boolean isTrionic(int[] nums) {
        int n = nums.length;
        int p = 0;
        while (p < n - 2 && nums[p] < nums[p + 1]) {
            p++;
        }
        if (p == 0) {
            return false;
        }
        int q = p;
        while (q < n - 1 && nums[q] > nums[q + 1]) {
            q++;
        }
        if (q == p || q == n - 1) {
            return false;
        }
        while (q < n - 1 && nums[q] < nums[q + 1]) {
            q++;
        }
        return q == n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isTrionic(vector<int>& nums) {
        int n = nums.size();
        int p = 0;
        while (p < n - 2 && nums[p] < nums[p + 1]) {
            p++;
        }
        if (p == 0) {
            return false;
        }
        int q = p;
        while (q < n - 1 && nums[q] > nums[q + 1]) {
            q++;
        }
        if (q == p || q == n - 1) {
            return false;
        }
        while (q < n - 1 && nums[q] < nums[q + 1]) {
            q++;
        }
        return q == n - 1;
    }
};
```

#### Go

```go
func isTrionic(nums []int) bool {
    n := len(nums)
    p := 0
    for p < n-2 && nums[p] < nums[p+1] {
        p++
    }
    if p == 0 {
        return false
    }
    q := p
    for q < n-1 && nums[q] > nums[q+1] {
        q++
    }
    if q == p || q == n-1 {
        return false
    }
    for q < n-1 && nums[q] < nums[q+1] {
        q++
    }
    return q == n-1
}
```

#### TypeScript

```ts
function isTrionic(nums: number[]): boolean {
    const n = nums.length;
    let p = 0;
    while (p < n - 2 && nums[p] < nums[p + 1]) {
        p++;
    }
    if (p === 0) {
        return false;
    }
    let q = p;
    while (q < n - 1 && nums[q] > nums[q + 1]) {
        q++;
    }
    if (q === p || q === n - 1) {
        return false;
    }
    while (q < n - 1 && nums[q] < nums[q + 1]) {
        q++;
    }
    return q === n - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_trionic(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut p = 0usize;

        while p + 2 < n && nums[p] < nums[p + 1] {
            p += 1;
        }
        if p == 0 {
            return false;
        }

        let mut q = p;
        while q + 1 < n && nums[q] > nums[q + 1] {
            q += 1;
        }
        if q == p || q + 1 == n {
            return false;
        }

        while q + 1 < n && nums[q] < nums[q + 1] {
            q += 1;
        }

        q + 1 == n
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
