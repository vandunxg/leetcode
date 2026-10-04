---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [3221. Maximum Array Hopping Score II 🔒](https://leetcode.com/problems/maximum-array-hopping-score-ii)

[中文文档](/solution/3200-3299/3221.Maximum%20Array%20Hopping%20Score%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>, bạn cần đạt <strong>điểm số lớn nhất</strong> bằng cách bắt đầu từ chỉ số 0 và <strong>nhảy</strong> cho đến khi đến phần tử cuối cùng của mảng.</p>

<p>Trong mỗi lần <strong>nhảy</strong>, bạn có thể nhảy từ chỉ số <code>i</code> đến một chỉ số <code>j &gt; i</code>, và nhận được <strong>điểm số</strong> bằng <code>(j - i) * nums[j]</code>.</p>

<p>Trả về <em>điểm số lớn nhất</em> mà bạn có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có hai cách để đến phần tử cuối cùng:</p>

<ul>
    <li><code>0 -&gt; 1 -&gt; 2</code> với điểm số là <code>(1 - 0) * 5 + (2 - 1) * 8 = 13</code>.</li>
    <li><code>0 -&gt; 2</code> với điểm số là <code>(2 - 0) * 8 = 16</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,2,8,9,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">42</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể nhảy theo cách <code>0 -&gt; 4 -&gt; 6</code> với điểm số là&nbsp;<code>(4 - 0) * 9 + (6 - 4) * 3 = 42</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số của mỗi lần nhảy giống với Maximum Array Hopping Score I, nhưng giới hạn đầu vào không cho phép dùng quy hoạch động bậc hai. Cách tối ưu vẫn là nhảy đến giá trị tiếp theo không nhỏ hơn giá trị hiện tại.
>
> Một stack giảm dần sẽ trích xuất chuỗi chỉ số đó; bắt đầu từ $0$, ta cộng $\textit{nums}[j]\times(j-i)$ theo các phần tử trong stack. Độ phức tạp thời gian là tuyến tính và không cần memoization.

<!-- thinking:end -->

Ta nhận thấy rằng tại vị trí hiện tại $i$, ta nên nhảy đến vị trí tiếp theo $j$ có giá trị lớn nhất để đạt điểm số lớn nhất.

Do đó, ta duyệt mảng $\textit{nums}$ và duy trì một stack $\textit{stk}$ giảm dần từ đáy lên đỉnh. Với vị trí hiện tại $i$ đang được duyệt, nếu giá trị tương ứng với phần tử ở đỉnh stack nhỏ hơn hoặc bằng $\textit{nums}[i]$, ta liên tục lấy phần tử ở đỉnh stack ra cho đến khi stack rỗng hoặc giá trị tương ứng với phần tử ở đỉnh stack lớn hơn $\textit{nums}[i]$, sau đó đẩy $i$ vào stack.

Tiếp theo, ta khởi tạo đáp án $\textit{ans}$ và vị trí hiện tại $i = 0$, rồi duyệt các phần tử trong stack. Mỗi lần lấy phần tử ở đỉnh stack là $j$, cập nhật đáp án $\textit{ans} += \textit{nums}[j] \times (j - i)$, sau đó cập nhật $i = j$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        stk = []
        for i, x in enumerate(nums):
            while stk and nums[stk[-1]] <= x:
                stk.pop()
            stk.append(i)
        ans = i = 0
        for j in stk:
            ans += nums[j] * (j - i)
            i = j
        return ans
```

#### Java

```java
class Solution {
    public long maxScore(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < nums.length; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                stk.pop();
            }
            stk.push(i);
        }
        long ans = 0, i = 0;
        while (!stk.isEmpty()) {
            int j = stk.pollLast();
            ans += (j - i) * nums[j];
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums) {
        vector<int> stk;
        for (int i = 0; i < nums.size(); ++i) {
            while (stk.size() && nums[stk.back()] <= nums[i]) {
                stk.pop_back();
            }
            stk.push_back(i);
        }
        long long ans = 0, i = 0;
        for (int j : stk) {
            ans += (j - i) * nums[j];
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(nums []int) (ans int64) {
    stk := []int{}
    for i, x := range nums {
        for len(stk) > 0 && nums[stk[len(stk)-1]] <= x {
            stk = stk[:len(stk)-1]
        }
        stk = append(stk, i)
    }
    i := 0
    for _, j := range stk {
        ans += int64((j - i) * nums[j])
        i = j
    }
    return
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const stk: number[] = [];
    for (let i = 0; i < nums.length; ++i) {
        while (stk.length && nums[stk.at(-1)!] <= nums[i]) {
            stk.pop();
        }
        stk.push(i);
    }
    let ans = 0;
    let i = 0;
    for (const j of stk) {
        ans += (j - i) * nums[j];
        i = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
