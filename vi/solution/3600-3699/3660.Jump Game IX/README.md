---
comments: true
difficulty: Medium
rating: 2187
source: Weekly Contest 464 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3660. Jump Game IX](https://leetcode.com/problems/jump-game-ix)

[中文文档](/solution/3600-3699/3660.Jump%20Game%20IX/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Từ bất kỳ chỉ số <code>i</code> nào, bạn có thể nhảy đến một chỉ số khác <code>j</code> theo các quy tắc sau:</p>

<ul>
	<li>Chỉ được phép nhảy đến chỉ số <code>j</code> với <code>j &gt; i</code> khi <code>nums[j] &lt; nums[i]</code>.</li>
	<li>Chỉ được phép nhảy đến chỉ số <code>j</code> với <code>j &lt; i</code> khi <code>nums[j] &gt; nums[i]</code>.</li>
</ul>

<p>Với mỗi chỉ số <code>i</code>, hãy tìm <strong>giá trị</strong> <strong>lớn nhất</strong> trong <code>nums</code> có thể đạt được bằng cách thực hiện <strong>bất kỳ</strong> chuỗi bước nhảy hợp lệ nào bắt đầu từ <code>i</code>.</p>

<p>Trả về một mảng <code>ans</code>, trong đó <code>ans[i]</code> là <strong>giá trị</strong> <strong>lớn nhất</strong> có thể đạt được khi bắt đầu từ chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>: Không có bước nhảy nào làm tăng giá trị.</li>
	<li>Với <code>i = 1</code>: Nhảy đến <code>j = 0</code> vì <code>nums[j] = 2</code> lớn hơn <code>nums[i]</code>.</li>
	<li>Với <code>i = 2</code>: Vì <code>nums[2] = 3</code> là giá trị lớn nhất trong <code>nums</code>, không có bước nhảy nào làm tăng giá trị.</li>
</ul>

<p>Do đó, <code>ans = [2, 2, 3]</code>.</p>

<ul>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,3,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>: Nhảy về phía trước đến <code>j = 2</code> vì <code>nums[j] = 1</code> nhỏ hơn <code>nums[i] = 2</code>, sau đó từ <code>i = 2</code> nhảy đến <code>j = 1</code> vì <code>nums[j] = 3</code> lớn hơn <code>nums[2]</code>.</li>
	<li>Với <code>i = 1</code>: Vì <code>nums[1] = 3</code> là giá trị lớn nhất trong <code>nums</code>, không có bước nhảy nào làm tăng giá trị.</li>
	<li>Với <code>i = 2</code>: Nhảy đến <code>j = 1</code> vì <code>nums[j] = 3</code> lớn hơn <code>nums[2] = 1</code>.</li>
</ul>

<p>Do đó, <code>ans = [3, 3, 3]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup>​​​​​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Lập trình động

<!-- thinking:start -->

> **Tư duy**
>
> Từ mỗi $i$, thực hiện bước nhảy theo quy tắc đã cho và tìm giá trị lớn nhất có thể đạt được. Việc mô phỏng mọi chuỗi bước nhảy sẽ có độ phức tạp bậc hai.
>
> Giá trị lớn nhất trên prefix $\textit{preMax}[i]$ là bước nâng có thể thực hiện về bên trái; giá trị nhỏ nhất trên suffix $\textit{sufMin}$ quyết định liệu $i$ có thể vượt qua chính nó để đi sang bên phải hay không.
>
> Duyệt từ phải sang trái, nếu $\textit{preMax}[i]>\textit{sufMin}$ thì $i$ có thể đạt được mọi giá trị mà $i+1$ đạt được, nên $\textit{ans}[i]=\textit{ans}[i+1]$; ngược lại, đáp án là $\textit{preMax}[i]$. Sau đó cập nhật $\textit{sufMin}$.

<!-- thinking:end -->

Nếu $i = n - 1$, nó có thể nhảy đến giá trị lớn nhất trong $\textit{nums}$, nên $\textit{ans}[i] = \max(\textit{nums})$. Với các vị trí $i$ khác, ta có thể tính toán bằng cách duy trì một mảng giá trị lớn nhất trên prefix và một biến giá trị nhỏ nhất trên suffix.

Các bước cụ thể như sau:

1. Tạo một mảng $\textit{preMax}$, trong đó $\textit{preMax}[i]$ biểu diễn giá trị lớn nhất trong đoạn $[0, i]$ khi duyệt từ trái sang phải.
2. Tạo một biến $\textit{sufMin}$, biểu diễn giá trị nhỏ nhất ở bên phải phần tử hiện tại khi duyệt từ phải sang trái. Ban đầu $\textit{sufMin} = \infty$.
3. Trước tiên, tiền xử lý mảng $\textit{preMax}$.
4. Tiếp theo, duyệt mảng từ phải sang trái. Với mỗi vị trí $i$, nếu $\textit{preMax}[i] > \textit{sufMin}$, điều đó có nghĩa là ta có thể nhảy từ $i$ đến vị trí chứa $\textit{preMax}$, sau đó nhảy đến vị trí chứa $\textit{sufMin}$, và cuối cùng nhảy đến $i + 1$. Vì vậy, các số có thể đạt được từ $i + 1$ cũng có thể đạt được từ $i$, nên $\textit{ans}[i] = \textit{ans}[i + 1]$; ngược lại, cập nhật thành $\textit{preMax}[i]$. Sau đó cập nhật $\textit{sufMin}$.
5. Cuối cùng, trả về mảng kết quả $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValue(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [0] * n
        pre_max = [nums[0]] * n
        for i in range(1, n):
            pre_max[i] = max(pre_max[i - 1], nums[i])
        suf_min = inf
        for i in range(n - 1, -1, -1):
            ans[i] = ans[i + 1] if pre_max[i] > suf_min else pre_max[i]
            suf_min = min(suf_min, nums[i])
        return ans
```

#### Java

```java
class Solution {
    public int[] maxValue(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int[] preMax = new int[n];
        preMax[0] = nums[0];
        for (int i = 1; i < n; ++i) {
            preMax[i] = Math.max(preMax[i - 1], nums[i]);
        }
        int sufMin = 1 << 30;
        for (int i = n - 1; i >= 0; --i) {
            ans[i] = preMax[i] > sufMin ? ans[i + 1] : preMax[i];
            sufMin = Math.min(sufMin, nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxValue(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        vector<int> preMax(n, nums[0]);
        for (int i = 1; i < n; ++i) {
            preMax[i] = max(preMax[i - 1], nums[i]);
        }
        int sufMin = 1 << 30;
        for (int i = n - 1; i >= 0; --i) {
            ans[i] = preMax[i] > sufMin ? ans[i + 1] : preMax[i];
            sufMin = min(sufMin, nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxValue(nums []int) []int {
	n := len(nums)
	ans := make([]int, n)
	preMax := make([]int, n)
	preMax[0] = nums[0]
	for i := 1; i < n; i++ {
		preMax[i] = max(preMax[i-1], nums[i])
	}
	sufMin := 1 << 30
	for i := n - 1; i >= 0; i-- {
		if preMax[i] > sufMin {
			ans[i] = ans[i+1]
		} else {
			ans[i] = preMax[i]
		}
		sufMin = min(sufMin, nums[i])
	}
	return ans
}
```

#### TypeScript

```ts
function maxValue(nums: number[]): number[] {
    const n = nums.length;
    const ans = Array(n).fill(0);
    const preMax = Array(n).fill(nums[0]);
    for (let i = 1; i < n; i++) {
        preMax[i] = Math.max(preMax[i - 1], nums[i]);
    }
    let sufMin = 1 << 30;
    for (let i = n - 1; i >= 0; i--) {
        ans[i] = preMax[i] > sufMin ? ans[i + 1] : preMax[i];
        sufMin = Math.min(sufMin, nums[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_value(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut ans = vec![0; n];
        let mut pre_max = vec![nums[0]; n];

        for i in 1..n {
            pre_max[i] = pre_max[i - 1].max(nums[i]);
        }

        let mut suf_min = i32::MAX;

        for i in (0..n).rev() {
            ans[i] = if i == n - 1 || pre_max[i] <= suf_min {
                pre_max[i]
            } else {
                ans[i + 1]
            };
            suf_min = suf_min.min(nums[i]);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
