---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [3555. Smallest Subarray to Sort in Every Sliding Window 🔒](https://leetcode.com/problems/smallest-subarray-to-sort-in-every-sliding-window)

[中文文档](/solution/3500-3599/3555.Smallest%20Subarray%20to%20Sort%20in%20Every%20Sliding%20Window/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Với mỗi <span data-keyword="subarray">mảng con</span> liên tiếp có độ dài <code>k</code>, hãy xác định độ dài <strong>nhỏ nhất</strong> của một đoạn liên tiếp cần được sắp xếp để toàn bộ cửa sổ trở thành <strong>không giảm</strong>; nếu cửa sổ đã được sắp xếp, độ dài cần thiết là bằng không.</p>

<p>Trả về một mảng có độ dài <code>n &minus; k + 1</code>, trong đó mỗi phần tử tương ứng với đáp án cho cửa sổ của nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,4,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>nums[0...2] = [1, 3, 2]</code>. Sắp xếp <code>[3, 2]</code> để được <code>[1, 2, 3]</code>, đáp án là 2.</li>
    <li><code>nums[1...3] = [3, 2, 4]</code>. Sắp xếp <code>[3, 2]</code> để được <code>[2, 3, 4]</code>, đáp án là 2.</li>
    <li><code>nums[2...4] = [2, 4, 5]</code> đã được sắp xếp, nên đáp án là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,3,2,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>nums[0...3] = [5, 4, 3, 2]</code>. Toàn bộ mảng con phải được sắp xếp, nên đáp án là 4.</li>
    <li><code>nums[1...4] = [4, 3, 2, 1]</code>. Toàn bộ mảng con phải được sắp xếp, nên đáp án là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 1000</code></li>
    <li><code>1 &lt;= k &lt;= nums.length</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Duy trì giá trị lớn nhất bên trái và nhỏ nhất bên phải

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi cửa sổ có độ dài $k$, ta cần tìm mảng con ngắn nhất mà khi sắp xếp sẽ khiến cửa sổ không giảm — đây chính là bài toán tìm mảng con chưa được sắp xếp ngắn nhất. Trong một cửa sổ, giá trị lớn nhất khi duyệt từ trái sang phải và giá trị nhỏ nhất khi duyệt từ phải sang trái giúp xác định hai đầu mút.
>
> Khi $n \cdot k$ nằm trong giới hạn cho phép, ta duyệt từng cửa sổ trong $O(k)$. Nếu cửa sổ đã được sắp xếp, hai đầu mút vẫn là $-1$ và đáp án là $0$.

<!-- thinking:end -->

Ta có thể liệt kê mọi mảng con có độ dài $k$. Với mỗi mảng con $nums[i...i + k - 1]$, ta cần tìm đoạn liên tiếp nhỏ nhất sao cho sau khi sắp xếp đoạn đó, toàn bộ mảng con trở thành không giảm.

Với mảng con $nums[i...i + k - 1]$, ta có thể duyệt từ trái sang phải và duy trì giá trị lớn nhất $mx$. Nếu giá trị hiện tại nhỏ hơn $mx$, điều đó có nghĩa là giá trị hiện tại chưa ở đúng vị trí, nên ta cập nhật biên phải $r$ thành vị trí hiện tại. Tương tự, ta có thể duyệt từ phải sang trái và duy trì giá trị nhỏ nhất $mi$. Nếu giá trị hiện tại lớn hơn $mi$, điều đó có nghĩa là giá trị hiện tại chưa ở đúng vị trí, nên ta cập nhật biên trái $l$ thành vị trí hiện tại. Ban đầu, cả $l$ và $r$ đều được đặt là $-1$. Nếu cả $l$ và $r$ đều không được cập nhật, mảng con đã được sắp xếp, nên ta trả về $0$; ngược lại, ta trả về $r - l + 1$.

Độ phức tạp thời gian là $O(n \times k)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSubarraySort(self, nums: List[int], k: int) -> List[int]:
        def f(i: int, j: int) -> int:
            mi, mx = inf, -inf
            l = r = -1
            for k in range(i, j + 1):
                if mx > nums[k]:
                    r = k
                else:
                    mx = nums[k]
                p = j - k + i
                if mi < nums[p]:
                    l = p
                else:
                    mi = nums[p]
            return 0 if r == -1 else r - l + 1

        n = len(nums)
        return [f(i, i + k - 1) for i in range(n - k + 1)]
```

#### Java

```java
class Solution {
    private int[] nums;
    private final int inf = 1 << 30;

    public int[] minSubarraySort(int[] nums, int k) {
        this.nums = nums;
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        for (int i = 0; i < n - k + 1; ++i) {
            ans[i] = f(i, i + k - 1);
        }
        return ans;
    }

    private int f(int i, int j) {
        int mi = inf, mx = -inf;
        int l = -1, r = -1;
        for (int k = i; k <= j; ++k) {
            if (nums[k] < mx) {
                r = k;
            } else {
                mx = nums[k];
            }
            int p = j - k + i;
            if (nums[p] > mi) {
                l = p;
            } else {
                mi = nums[p];
            }
        }
        return r == -1 ? 0 : r - l + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minSubarraySort(vector<int>& nums, int k) {
        const int inf = 1 << 30;
        int n = nums.size();
        auto f = [&](int i, int j) -> int {
            int mi = inf, mx = -inf;
            int l = -1, r = -1;
            for (int k = i; k <= j; ++k) {
                if (nums[k] < mx) {
                    r = k;
                } else {
                    mx = nums[k];
                }
                int p = j - k + i;
                if (nums[p] > mi) {
                    l = p;
                } else {
                    mi = nums[p];
                }
            }
            return r == -1 ? 0 : r - l + 1;
        };
        vector<int> ans;
        for (int i = 0; i < n - k + 1; ++i) {
            ans.push_back(f(i, i + k - 1));
        }
        return ans;
    }
};
```

#### Go

```go
func minSubarraySort(nums []int, k int) []int {
    const inf = 1 << 30
    n := len(nums)
    f := func(i, j int) int {
        mi := inf
        mx := -inf
        l, r := -1, -1
        for p := i; p <= j; p++ {
            if nums[p] < mx {
                r = p
            } else {
                mx = nums[p]
            }
            q := j - p + i
            if nums[q] > mi {
                l = q
            } else {
                mi = nums[q]
            }
        }
        if r == -1 {
            return 0
        }
        return r - l + 1
    }

    ans := make([]int, 0, n-k+1)
    for i := 0; i <= n-k; i++ {
        ans = append(ans, f(i, i+k-1))
    }
    return ans
}
```

#### TypeScript

```ts
function minSubarraySort(nums: number[], k: number): number[] {
    const inf = Infinity;
    const n = nums.length;
    const f = (i: number, j: number): number => {
        let mi = inf;
        let mx = -inf;
        let l = -1,
            r = -1;
        for (let p = i; p <= j; ++p) {
            if (nums[p] < mx) {
                r = p;
            } else {
                mx = nums[p];
            }
            const q = j - p + i;
            if (nums[q] > mi) {
                l = q;
            } else {
                mi = nums[q];
            }
        }
        return r === -1 ? 0 : r - l + 1;
    };

    const ans: number[] = [];
    for (let i = 0; i <= n - k; ++i) {
        ans.push(f(i, i + k - 1));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
