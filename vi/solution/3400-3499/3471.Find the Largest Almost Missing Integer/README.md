---
comments: true
difficulty: Easy
rating: 1308
source: Weekly Contest 439 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3471. Find the Largest Almost Missing Integer](https://leetcode.com/problems/find-the-largest-almost-missing-integer)

[中文文档](/solution/3400-3499/3471.Find%20the%20Largest%20Almost%20Missing%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>gần thiếu</strong> trong <code>nums</code> nếu <code>x</code> xuất hiện trong <em>đúng</em> một mảng con có kích thước <code>k</code> của <code>nums</code>.</p>

<p>Trả về số nguyên <b>lớn nhất</b> <strong>gần thiếu</strong> trong <code>nums</code>. Nếu không tồn tại số nào như vậy, trả về <code>-1</code>.</p>
<strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,9,2,1,7], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>1 xuất hiện trong 2 mảng con có kích thước 3: <code>[9, 2, 1]</code> và <code>[2, 1, 7]</code>.</li>
	<li>2 xuất hiện trong 3 mảng con có kích thước 3: <code>[3, 9, 2]</code>, <code>[9, 2, 1]</code>, <code>[2, 1, 7]</code>.</li>
	<li index="2">3 xuất hiện trong 1 mảng con có kích thước 3: <code>[3, 9, 2]</code>.</li>
	<li index="3">7 xuất hiện trong 1 mảng con có kích thước 3: <code>[2, 1, 7]</code>.</li>
	<li index="4">9 xuất hiện trong 2 mảng con có kích thước 3: <code>[3, 9, 2]</code>, và <code>[9, 2, 1]</code>.</li>
</ul>

<p>Ta trả về 7 vì đây là số nguyên lớn nhất xuất hiện trong đúng một mảng con có kích thước <code>k</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,9,7,2,1,7], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>1 xuất hiện trong 2 mảng con có kích thước 4: <code>[9, 7, 2, 1]</code>, <code>[7, 2, 1, 7]</code>.</li>
	<li>2 xuất hiện trong 3 mảng con có kích thước 4: <code>[3, 9, 7, 2]</code>, <code>[9, 7, 2, 1]</code>, <code>[7, 2, 1, 7]</code>.</li>
	<li>3 xuất hiện trong 1 mảng con có kích thước 4: <code>[3, 9, 7, 2]</code>.</li>
	<li>7 xuất hiện trong 3 mảng con có kích thước 4: <code>[3, 9, 7, 2]</code>, <code>[9, 7, 2, 1]</code>, <code>[7, 2, 1, 7]</code>.</li>
	<li>9 xuất hiện trong 2 mảng con có kích thước 4: <code>[3, 9, 7, 2]</code>, <code>[9, 7, 2, 1]</code>.</li>
</ul>

<p>Ta trả về 3 vì đây là số nguyên lớn nhất và duy nhất xuất hiện trong đúng một mảng con có kích thước <code>k</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,0], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên nào chỉ xuất hiện trong một mảng con có kích thước 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 50</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Một số gần thiếu xuất hiện trong đúng một cửa sổ có độ dài $k$. Việc đếm tần suất trên toàn mảng bỏ qua sự chồng lấn giữa các cửa sổ.
>
> Khi $k=1$, mỗi phần tử là một cửa sổ, nên ta lấy giá trị duy nhất lớn nhất. Khi $k=n$, chỉ có một cửa sổ, nên ta lấy giá trị lớn nhất trong mảng.
>
> Khi $1<k<n$, các giá trị ở giữa nằm trong ít nhất hai cửa sổ; chỉ hai đầu mảng mới có thể xuất hiện đúng một lần. Ta kiểm tra rằng $\textit{nums}[0]$ và $\textit{nums}[n-1]$ không xuất hiện lại, rồi trả về giá trị lớn hơn, hoặc $-1$.

<!-- thinking:end -->

Nếu $k = 1$, mỗi phần tử trong mảng tạo thành một mảng con có kích thước $1$. Khi đó, ta chỉ cần tìm giá trị lớn nhất trong số các phần tử xuất hiện đúng một lần trong mảng.

Nếu $k = n$, toàn bộ mảng tạo thành một mảng con có kích thước $n$. Khi đó, ta chỉ cần trả về giá trị lớn nhất trong mảng.

Nếu $1 < k < n$, chỉ $\textit{nums}[0]$ và $\textit{nums}[n-1]$ có thể là các số nguyên gần thiếu. Nếu chúng xuất hiện ở nơi khác trong mảng, chúng không phải là số nguyên gần thiếu. Vì vậy, ta chỉ cần kiểm tra xem $\textit{nums}[0]$ và $\textit{nums}[n-1]$ có xuất hiện ở nơi khác trong mảng hay không, rồi trả về giá trị lớn hơn trong hai giá trị này.

Nếu không tồn tại số nguyên gần thiếu, trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestInteger(self, nums: List[int], k: int) -> int:
        def f(k: int) -> int:
            for i, x in enumerate(nums):
                if i != k and x == nums[k]:
                    return -1
            return nums[k]

        if k == 1:
            cnt = Counter(nums)
            return max((x for x, v in cnt.items() if v == 1), default=-1)
        if k == len(nums):
            return max(nums)
        return max(f(0), f(len(nums) - 1))
```

#### Java

```java
class Solution {
    private int[] nums;

    public int largestInteger(int[] nums, int k) {
        this.nums = nums;
        if (k == 1) {
            Map<Integer, Integer> cnt = new HashMap<>();
            for (int x : nums) {
                cnt.merge(x, 1, Integer::sum);
            }
            int ans = -1;
            for (var e : cnt.entrySet()) {
                if (e.getValue() == 1) {
                    ans = Math.max(ans, e.getKey());
                }
            }
            return ans;
        }
        if (k == nums.length) {
            return Arrays.stream(nums).max().getAsInt();
        }
        return Math.max(f(0), f(nums.length - 1));
    }

    private int f(int k) {
        for (int i = 0; i < nums.length; ++i) {
            if (i != k && nums[i] == nums[k]) {
                return -1;
            }
        }
        return nums[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestInteger(vector<int>& nums, int k) {
        if (k == 1) {
            unordered_map<int, int> cnt;
            for (int x : nums) {
                ++cnt[x];
            }
            int ans = -1;
            for (auto& [x, v] : cnt) {
                if (v == 1) {
                    ans = max(ans, x);
                }
            }
            return ans;
        }
        int n = nums.size();
        if (k == n) {
            return ranges::max(nums);
        }
        auto f = [&](int k) -> int {
            for (int i = 0; i < n; ++i) {
                if (i != k && nums[i] == nums[k]) {
                    return -1;
                }
            }
            return nums[k];
        };
        return max(f(0), f(n - 1));
    }
};
```

#### Go

```go
func largestInteger(nums []int, k int) int {
    if k == 1 {
        cnt := make(map[int]int)
        for _, x := range nums {
            cnt[x]++
        }
        ans := -1
        for x, v := range cnt {
            if v == 1 {
                ans = max(ans, x)
            }
        }
        return ans
    }

    n := len(nums)
    if k == n {
        return slices.Max(nums)
    }

    f := func(k int) int {
        for i, x := range nums {
            if i != k && x == nums[k] {
                return -1
            }
        }
        return nums[k]
    }

    return max(f(0), f(n-1))
}
```

#### TypeScript

```ts
function largestInteger(nums: number[], k: number): number {
    if (k === 1) {
        const cnt = new Map<number, number>();
        for (const x of nums) {
            cnt.set(x, (cnt.get(x) || 0) + 1);
        }
        let ans = -1;
        for (const [x, v] of cnt.entries()) {
            if (v === 1 && x > ans) {
                ans = x;
            }
        }
        return ans;
    }

    const n = nums.length;
    if (k === n) {
        return Math.max(...nums);
    }

    const f = (k: number): number => {
        for (let i = 0; i < n; i++) {
            if (i !== k && nums[i] === nums[k]) {
                return -1;
            }
        }
        return nums[k];
    };

    return Math.max(f(0), f(n - 1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
