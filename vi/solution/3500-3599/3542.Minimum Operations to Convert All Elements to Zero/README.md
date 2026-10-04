---
comments: true
difficulty: Medium
rating: 1889
source: Biweekly Contest 156 Q2
tags:
    - Stack
    - Greedy
    - Array
    - Hash Table
    - Monotonic Stack
---

<!-- problem:start -->

# [3542. Minimum Operations to Convert All Elements to Zero](https://leetcode.com/problems/minimum-operations-to-convert-all-elements-to-zero)

[中文文档](/solution/3500-3599/3542.Minimum%20Operations%20to%20Convert%20All%20Elements%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->
<p>Bạn được cho một mảng <code>nums</code> có kích thước <code>n</code>, gồm các số nguyên <strong>không âm</strong>. Nhiệm vụ của bạn là thực hiện một số thao tác (có thể bằng 0) trên mảng sao cho <strong>tất cả</strong> các phần tử đều trở thành 0.</p>

<p>Trong một thao tác, bạn có thể chọn một <span data-keyword="subarray">mảng con</span> <code>[i, j]</code> (trong đó <code>0 &lt;= i &lt;= j &lt; n</code>) và đặt tất cả các phần tử có giá trị bằng số nguyên <strong>không âm</strong> <strong>nhỏ nhất</strong> trong mảng con đó thành 0.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến tất cả các phần tử trong mảng thành 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chọn mảng con <code>[1,1]</code> (là <code>[2]</code>), trong đó số nguyên không âm nhỏ nhất là 2. Đặt tất cả các phần tử có giá trị 2 thành 0, ta được <code>[0,0]</code>.</li>
    <li>Vì vậy, số thao tác nhỏ nhất cần thực hiện là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chọn mảng con <code>[1,3]</code> (là <code>[1,2,1]</code>), trong đó số nguyên không âm nhỏ nhất là 1. Đặt tất cả các phần tử có giá trị 1 thành 0, ta được <code>[3,0,2,0]</code>.</li>
    <li>Chọn mảng con <code>[2,2]</code> (là <code>[2]</code>), trong đó số nguyên không âm nhỏ nhất là 2. Đặt tất cả các phần tử có giá trị 2 thành 0, ta được <code>[3,0,0,0]</code>.</li>
    <li>Chọn mảng con <code>[0,0]</code> (là <code>[3]</code>), trong đó số nguyên không âm nhỏ nhất là 3. Đặt tất cả các phần tử có giá trị 3 thành 0, ta được <code>[0,0,0,0]</code>.</li>
    <li>Vì vậy, số thao tác nhỏ nhất cần thực hiện là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chọn mảng con <code>[0,5]</code> (là <code>[1,2,1,2,1,2]</code>), trong đó số nguyên không âm nhỏ nhất là 1. Đặt tất cả các phần tử có giá trị 1 thành 0, ta được <code>[0,2,0,2,0,2]</code>.</li>
    <li>Chọn mảng con <code>[1,1]</code> (là <code>[2]</code>), trong đó số nguyên không âm nhỏ nhất là 2. Đặt tất cả các phần tử có giá trị 2 thành 0, ta được <code>[0,0,0,2,0,2]</code>.</li>
    <li>Chọn mảng con <code>[3,3]</code> (là <code>[2]</code>), trong đó số nguyên không âm nhỏ nhất là 2. Đặt tất cả các phần tử có giá trị 2 thành 0, ta được <code>[0,0,0,0,0,2]</code>.</li>
    <li>Chọn mảng con <code>[5,5]</code> (là <code>[2]</code>), trong đó số nguyên không âm nhỏ nhất là 2. Đặt tất cả các phần tử có giá trị 2 thành 0, ta được <code>[0,0,0,0,0,0]</code>.</li>
    <li>Vì vậy, số thao tác nhỏ nhất cần thực hiện là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác đưa toàn bộ một đoạn liên tiếp có giá trị nhỏ nhất hiện tại về 0. Các giá trị bằng nhau bị ngăn cách bởi một giá trị nhỏ hơn cần các thao tác riêng. Việc duyệt các đoạn của từng giá trị có thể dẫn đến độ phức tạp bậc hai.
>
> Ta duy trì một stack tăng nghiêm ngặt. Khi gặp một giá trị mới nhỏ hơn $x$, ta loại bỏ từng phần tử lớn hơn ở đỉnh stack, mỗi phần tử tương ứng với một thao tác; giá trị trùng với đỉnh stack được gộp lại mà không làm tăng đáp án. Các phần tử còn lại trong stack, mỗi phần tử cần thêm một thao tác.

<!-- thinking:end -->

Theo mô tả bài toán, trước tiên ta nên chuyển các số nhỏ nhất thành $0$, sau đó chuyển các số nhỏ thứ hai thành $0$, và cứ tiếp tục như vậy. Trong quá trình này, nếu hai số bị ngăn cách bởi các số nhỏ hơn, chúng cần thêm một thao tác để trở thành $0$.

Ta có thể duy trì một stack tăng dần đơn điệu $\textit{stk}$ từ đáy lên đỉnh, đồng thời duyệt từng số $\textit{x}$ trong mảng $\textit{nums}$:

- Khi phần tử ở đỉnh stack lớn hơn $\textit{x}$, điều đó có nghĩa là $\textit{x}$ ngăn cách phần tử ở đỉnh stack. Ta cần lấy phần tử ở đỉnh ra và tăng đáp án lên $1$, tiếp tục cho đến khi phần tử ở đỉnh không còn lớn hơn $\textit{x}$.
- Nếu $\textit{x}$ khác $0$, và stack rỗng hoặc phần tử ở đỉnh khác $\textit{x}$, ta đưa $\textit{x}$ vào stack.

Sau khi duyệt xong, mỗi phần tử còn lại trong stack đều cần thêm một thao tác để trở thành $0$, vì vậy ta cộng kích thước của stack vào đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        stk = []
        ans = 0
        for x in nums:
            while stk and stk[-1] > x:
                ans += 1
                stk.pop()
            if x and (not stk or stk[-1] != x):
                stk.append(x)
        ans += len(stk)
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        int ans = 0;
        for (int x : nums) {
            while (!stk.isEmpty() && stk.peek() > x) {
                ans++;
                stk.pop();
            }
            if (x != 0 && (stk.isEmpty() || stk.peek() != x)) {
                stk.push(x);
            }
        }
        ans += stk.size();
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        vector<int> stk;
        int ans = 0;
        for (int x : nums) {
            while (!stk.empty() && stk.back() > x) {
                ++ans;
                stk.pop_back();
            }
            if (x != 0 && (stk.empty() || stk.back() != x)) {
                stk.push_back(x);
            }
        }
        ans += stk.size();
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
    stk := []int{}
    ans := 0
    for _, x := range nums {
        for len(stk) > 0 && stk[len(stk)-1] > x {
            ans++
            stk = stk[:len(stk)-1]
        }
        if x != 0 && (len(stk) == 0 || stk[len(stk)-1] != x) {
            stk = append(stk, x)
        }
    }
    ans += len(stk)
    return ans
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const stk: number[] = [];
    let ans = 0;
    for (const x of nums) {
        while (stk.length > 0 && stk[stk.length - 1] > x) {
            ans++;
            stk.pop();
        }
        if (x !== 0 && (stk.length === 0 || stk[stk.length - 1] !== x)) {
            stk.push(x);
        }
    }
    ans += stk.length;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let mut stk = Vec::new();
        let mut ans = 0;
        for &x in nums.iter() {
            while let Some(&last) = stk.last() {
                if last > x {
                    ans += 1;
                    stk.pop();
                } else {
                    break;
                }
            }
            if x != 0 && (stk.is_empty() || *stk.last().unwrap() != x) {
                stk.push(x);
            }
        }
        ans += stk.len() as i32;
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
