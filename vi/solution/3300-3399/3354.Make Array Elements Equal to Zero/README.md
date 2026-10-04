---
comments: true
difficulty: Easy
rating: 1397
source: Weekly Contest 424 Q1
tags:
    - Array
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [3354. Make Array Elements Equal to Zero](https://leetcode.com/problems/make-array-elements-equal-to-zero)

[Tài liệu tiếng Trung](/solution/3300-3399/3354.Make%20Array%20Elements%20Equal%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trước tiên, chọn một vị trí bắt đầu <code>curr</code> sao cho <code>nums[curr] == 0</code>, rồi chọn một <strong>hướng</strong> di chuyển là sang trái hoặc sang phải.</p>

<p>Sau đó, lặp lại quy trình sau:</p>

<ul>
    <li>Nếu <code>curr</code> nằm ngoài phạm vi <code>[0, n - 1]</code>, quy trình kết thúc.</li>
    <li>Nếu <code>nums[curr] == 0</code>, di chuyển theo hướng hiện tại bằng cách <strong>tăng</strong> <code>curr</code> nếu đang đi sang phải, hoặc <strong>giảm</strong> <code>curr</code> nếu đang đi sang trái.</li>
    <li>Nếu không, nếu <code>nums[curr] &gt; 0</code>:
    <ul>
        <li>Giảm <code>nums[curr]</code> đi 1.</li>
        <li><strong>Đảo ngược</strong>&nbsp;hướng di chuyển (trái thành phải và ngược lại).</li>
        <li>Bước một bước theo hướng mới.</li>
    </ul>
    </li>
</ul>

<p>Một lựa chọn vị trí ban đầu <code>curr</code> và hướng di chuyển được xem là <strong>hợp lệ</strong> nếu mọi phần tử trong <code>nums</code> đều trở thành 0 khi quy trình kết thúc.</p>

<p>Trả về số lượng lựa chọn <strong>hợp lệ</strong> có thể có.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,2,0,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các lựa chọn hợp lệ duy nhất là:</p>

<ul>
    <li>Chọn <code>curr = 3</code> và hướng di chuyển sang trái.

    <ul>
        <li><code>[1,0,2,<strong><u>0</u></strong>,3] -&gt; [1,0,<strong><u>2</u></strong>,0,3] -&gt; [1,0,1,<strong><u>0</u></strong>,3] -&gt; [1,0,1,0,<strong><u>3</u></strong>] -&gt; [1,0,1,<strong><u>0</u></strong>,2] -&gt; [1,0,<strong><u>1</u></strong>,0,2] -&gt; [1,0,0,<strong><u>0</u></strong>,2] -&gt; [1,0,0,0,<strong><u>2</u></strong>] -&gt; [1,0,0,<strong><u>0</u></strong>,1] -&gt; [1,0,<strong><u>0</u></strong>,0,1] -&gt; [1,<strong><u>0</u></strong>,0,0,1] -&gt; [<strong><u>1</u></strong>,0,0,0,1] -&gt; [0,<strong><u>0</u></strong>,0,0,1] -&gt; [0,0,<strong><u>0</u></strong>,0,1] -&gt; [0,0,0,<strong><u>0</u></strong>,1] -&gt; [0,0,0,0,<strong><u>1</u></strong>] -&gt; [0,0,0,0,0]</code>.</li>
    </ul>
    </li>
    <li>Chọn <code>curr = 3</code> và hướng di chuyển sang phải.
    <ul>
        <li><code>[1,0,2,<strong><u>0</u></strong>,3] -&gt; [1,0,2,0,<strong><u>3</u></strong>] -&gt; [1,0,2,<strong><u>0</u></strong>,2] -&gt; [1,0,<strong><u>2</u></strong>,0,2] -&gt; [1,0,1,<strong><u>0</u></strong>,2] -&gt; [1,0,1,0,<strong><u>2</u></strong>] -&gt; [1,0,1,<strong><u>0</u></strong>,1] -&gt; [1,0,<strong><u>1</u></strong>,0,1] -&gt; [1,0,0,<strong><u>0</u></strong>,1] -&gt; [1,0,0,0,<strong><u>1</u></strong>] -&gt; [1,0,0,<strong><u>0</u></strong>,0] -&gt; [1,0,<strong><u>0</u></strong>,0,0] -&gt; [1,<strong><u>0</u></strong>,0,0,0] -&gt; [<strong><u>1</u></strong>,0,0,0,0] -&gt; [0,0,0,0,0].</code></li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4,0,4,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có lựa chọn hợp lệ nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 100</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 100</code></li>
    <li>Tồn tại ít nhất một chỉ số <code>i</code> sao cho <code>nums[i] == 0</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu từ một giá trị $0$, ta giảm giá trị dương tiếp theo rồi quay đầu. Ta đếm các vị trí bắt đầu và hướng di chuyển giúp đưa cả mảng về 0. Chỉ cần duyệt tuyến tính qua mọi vị trí có giá trị $0$.
>
> Quy trình thành công khi tổng của hai phía cân bằng: hai tổng bằng nhau thì cả hai hướng đều hợp lệ; nếu chênh lệch bằng $1$, chỉ có thể bắt đầu theo phía có tổng lớn hơn.
>
> Dùng tổng tiền tố $l$ và tổng toàn mảng $s$, ta có thể kiểm tra mọi vị trí có giá trị $0$ mà không cần mô phỏng quá trình di chuyển.

<!-- thinking:end -->

Giả sử ban đầu ta di chuyển sang trái và gặp một phần tử khác 0. Khi đó, ta cần giảm phần tử này đi 1, sau đó đổi hướng di chuyển và tiếp tục di chuyển.

Vì vậy, ta có thể duy trì tổng các phần tử ở bên trái mỗi phần tử có giá trị 0 là $l$, còn tổng các phần tử ở bên phải là $s - l$. Nếu $l = s - l$, nghĩa là tổng các phần tử bên trái bằng tổng các phần tử bên phải, ta có thể chọn phần tử 0 hiện tại và di chuyển theo cả hai hướng, nên tăng đáp án thêm $2$. Nếu $|l - (s - l)| = 1$ và tổng các phần tử bên trái lớn hơn, ta có thể chọn phần tử 0 hiện tại và di chuyển sang trái, nên tăng đáp án thêm $1$. Nếu tổng các phần tử bên phải lớn hơn, ta chọn phần tử 0 hiện tại và di chuyển sang phải, cũng tăng đáp án thêm $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countValidSelections(self, nums: List[int]) -> int:
        s = sum(nums)
        ans = l = 0
        for x in nums:
            if x:
                l += x
            elif l * 2 == s:
                ans += 2
            elif abs(l * 2 - s) == 1:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countValidSelections(int[] nums) {
        int s = Arrays.stream(nums).sum();
        int ans = 0, l = 0;
        for (int x : nums) {
            if (x != 0) {
                l += x;
            } else if (l * 2 == s) {
                ans += 2;
            } else if (Math.abs(l * 2 - s) <= 1) {
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
    int countValidSelections(vector<int>& nums) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        int ans = 0, l = 0;
        for (int x : nums) {
            if (x) {
                l += x;
            } else if (l * 2 == s) {
                ans += 2;
            } else if (abs(l * 2 - s) <= 1) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countValidSelections(nums []int) (ans int) {
    l, s := 0, 0
    for _, x := range nums {
        s += x
    }
    for _, x := range nums {
        if x != 0 {
            l += x
        } else if l*2 == s {
            ans += 2
        } else if abs(l*2-s) <= 1 {
            ans++
        }
    }
    return
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

#### TypeScript

```ts
function countValidSelections(nums: number[]): number {
    const s = nums.reduce((acc, x) => acc + x, 0);
    let [ans, l] = [0, 0];
    for (const x of nums) {
        if (x) {
            l += x;
        } else if (l * 2 === s) {
            ans += 2;
        } else if (Math.abs(l * 2 - s) <= 1) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_valid_selections(nums: Vec<i32>) -> i32 {
        let s: i32 = nums.iter().sum();
        let mut ans = 0;
        let mut l = 0;
        for &x in &nums {
            if x != 0 {
                l += x;
            } else if l * 2 == s {
                ans += 2;
            } else if (l * 2 - s).abs() <= 1 {
                ans += 1;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountValidSelections(int[] nums) {
        int s = nums.Sum();
        int ans = 0, l = 0;
        foreach (int x in nums) {
            if (x != 0) {
                l += x;
            } else if (l * 2 == s) {
                ans += 2;
            } else if (Math.Abs(l * 2 - s) <= 1) {
                ans += 1;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
