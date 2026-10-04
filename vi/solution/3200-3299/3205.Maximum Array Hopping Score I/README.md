---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [3205. Maximum Array Hopping Score I 🔒](https://leetcode.com/problems/maximum-array-hopping-score-i)

[中文文档](/solution/3200-3299/3205.Maximum%20Array%20Hopping%20Score%20I/README.md)

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
    <li><code>0 -&gt; 1 -&gt; 2</code> với điểm số là&nbsp;<code>(1 - 0) * 5 + (2 - 1) * 8 = 13</code>.</li>
    <li><code>0 -&gt; 2</code> với điểm số là&nbsp;<code>(2 - 0) * 8 =&nbsp;16</code>.</li>
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
    <li><code>2 &lt;= nums.length &lt;= 10<sup>3</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Từ chỉ số $0$, ta nhảy sang phải đến một $j$ và nhận điểm $(j-i)\times\textit{nums}[j]$, đồng thời tối đa hóa tổng điểm. Vì $n\le 10^3$, ta không thể liệt kê các chuỗi bước nhảy theo cấp số mũ, nhưng $O(n^2)$ trạng thái vẫn phù hợp với giới hạn.
>
> Bài toán con chỉ phụ thuộc vào vị trí bắt đầu $i$: khi đã ở $i$, phần tiếp theo tốt nhất không phụ thuộc vào cách ta đến đó. Định nghĩa $\textit{dfs}(i)$ là điểm số tốt nhất từ $i$, lấy giá trị lớn nhất của $(j-i)\times\textit{nums}[j]+\textit{dfs}(j)$ với mọi $j>i$, rồi dùng memoization.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i)$, biểu diễn điểm số lớn nhất có thể đạt được khi bắt đầu từ chỉ số $i$. Do đó, đáp án là $\textit{dfs}(0)$.

Quá trình thực thi hàm $\textit{dfs}(i)$ như sau:

Ta liệt kê vị trí nhảy tiếp theo $j$. Khi đó, điểm số có thể đạt được khi bắt đầu từ chỉ số $i$ là $(j - i) \times \textit{nums}[j]$, cộng với điểm số lớn nhất có thể đạt được khi bắt đầu từ chỉ số $j$, nên tổng điểm là $(j - i) \times \textit{nums}[j] + \textit{dfs}(j)$. Ta liệt kê mọi $j$ có thể và lấy điểm số lớn nhất.

Để tránh tính toán dư thừa, ta sử dụng tìm kiếm với memoization. Ta lưu lại giá trị đã tính của $\textit{dfs}(i)$ để có thể trả về trực tiếp vào lần sau.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            return max(
                [(j - i) * nums[j] + dfs(j) for j in range(i + 1, len(nums))] or [0]
            )

        return dfs(0)
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] nums;
    private int n;

    public int maxScore(int[] nums) {
        n = nums.length;
        f = new Integer[n];
        this.nums = nums;
        return dfs(0);
    }

    private int dfs(int i) {
        if (f[i] != null) {
            return f[i];
        }
        f[i] = 0;
        for (int j = i + 1; j < n; ++j) {
            f[i] = Math.max(f[i], (j - i) * nums[j] + dfs(j));
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (f[i]) {
                return f[i];
            }
            for (int j = i + 1; j < n; ++j) {
                f[i] = max(f[i], (j - i) * nums[j] + dfs(j));
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func maxScore(nums []int) int {
    n := len(nums)
    f := make([]int, n)
    var dfs func(int) int
    dfs = func(i int) int {
        if f[i] > 0 {
            return f[i]
        }
        for j := i + 1; j < n; j++ {
            f[i] = max(f[i], (j-i)*nums[j]+dfs(j))
        }
        return f[i]
    }
    return dfs(0)
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(0);
    const dfs = (i: number): number => {
        if (f[i]) {
            return f[i];
        }
        for (let j = i + 1; j < n; ++j) {
            f[i] = Math.max(f[i], (j - i) * nums[j] + dfs(j));
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Công thức truy hồi trong Lời giải 1 vốn đã tối ưu; call stack và cache của đệ quy là không cần thiết. Ta chuyển cùng phép chuyển trạng thái sang dạng bottom-up: $f[j]$ là điểm số tốt nhất từ $0$ đến $j$, và với mỗi $j$, ta liệt kê các chỉ số trước đó $i<j$. Độ phức tạp thời gian vẫn là $O(n^2)$, nhưng nay được tính lặp.

<!-- thinking:end -->

Ta có thể chuyển tìm kiếm với memoization ở Lời giải 1 thành quy hoạch động.

Định nghĩa $f[j]$ là điểm số lớn nhất có thể đạt được khi bắt đầu từ chỉ số $0$ và kết thúc tại chỉ số $j$. Do đó, đáp án là $f[n - 1]$.

Công thức chuyển trạng thái là:

$$
f[j] = \max_{0 \leq i < j} \{ f[i] + (j - i) \times \textit{nums}[j] \}
$$

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, nums: List[int]) -> int:
        n = len(nums)
        f = [0] * n
        for j in range(1, n):
            for i in range(j):
                f[j] = max(f[j], f[i] + (j - i) * nums[j])
        return f[n - 1]
```

#### Java

```java
class Solution {
    public int maxScore(int[] nums) {
        int n = nums.length;
        int[] f = new int[n];
        for (int j = 1; j < n; ++j) {
            for (int i = 0; i < j; ++i) {
                f[j] = Math.max(f[j], f[i] + (j - i) * nums[j]);
            }
        }
        return f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);
        for (int j = 1; j < n; ++j) {
            for (int i = 0; i < j; ++i) {
                f[j] = max(f[j], f[i] + (j - i) * nums[j]);
            }
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func maxScore(nums []int) int {
    n := len(nums)
    f := make([]int, n)
    for j := 1; j < n; j++ {
        for i := 0; i < j; i++ {
            f[j] = max(f[j], f[i]+(j-i)*nums[j])
        }
    }
    return f[n-1]
}
```

#### TypeScript

```ts
function maxScore(nums: number[]): number {
    const n = nums.length;
    const f: number[] = Array(n).fill(0);
    for (let j = 1; j < n; ++j) {
        for (let i = 0; i < j; ++i) {
            f[j] = Math.max(f[j], f[i] + (j - i) * nums[j]);
        }
    }
    return f[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Ngăn xếp đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 vẫn có độ phức tạp bậc hai. Từ $i$, việc nhảy đến một $j$ không phải là vị trí kế tiếp có giá trị không nhỏ hơn giá trị hiện tại sẽ không thể tốt hơn việc chèn bước nhảy trung gian đó, nên đường đi tối ưu chỉ đi qua dãy các chỉ số lớn hơn kế tiếp. Monotonic stack trích xuất dãy chỉ số này; sau đó tính điểm cho các bước nhảy kề nhau trong thời gian tuyến tính.

<!-- thinking:end -->

Ta nhận thấy rằng tại vị trí hiện tại $i$, ta nên nhảy đến vị trí tiếp theo $j$ có giá trị lớn nhất để đạt điểm số lớn nhất.

Do đó, ta duyệt mảng $\textit{nums}$ và duy trì một stack $\textit{stk}$ giảm dần từ đáy lên đỉnh. Với vị trí hiện tại $i$ đang được duyệt, nếu giá trị tương ứng với phần tử ở đỉnh stack nhỏ hơn hoặc bằng $\textit{nums}[i]$, ta liên tục lấy phần tử ở đỉnh stack ra cho đến khi stack rỗng hoặc giá trị tương ứng với phần tử ở đỉnh stack lớn hơn $\textit{nums}[i]$, sau đó đẩy $i$ vào stack.

Tiếp theo, ta khởi tạo đáp án $\textit{ans}$ và vị trí hiện tại $i = 0$, rồi duyệt các phần tử trong stack. Mỗi lần lấy phần tử ở đỉnh stack là $j$, cập nhật đáp án $\textit{ans} += \textit{nums}[j] \times (j - i)$, sau đó cập nhật $i = j$.

Cuối cùng, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

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
    public int maxScore(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < nums.length; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                stk.pop();
            }
            stk.push(i);
        }
        int ans = 0, i = 0;
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
    int maxScore(vector<int>& nums) {
        vector<int> stk;
        for (int i = 0; i < nums.size(); ++i) {
            while (stk.size() && nums[stk.back()] <= nums[i]) {
                stk.pop_back();
            }
            stk.push_back(i);
        }
        int ans = 0, i = 0;
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
func maxScore(nums []int) (ans int) {
    stk := []int{}
    for i, x := range nums {
        for len(stk) > 0 && nums[stk[len(stk)-1]] <= x {
            stk = stk[:len(stk)-1]
        }
        stk = append(stk, i)
    }
    i := 0
    for _, j := range stk {
        ans += (j - i) * nums[j]
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
