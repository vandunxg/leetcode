---
comments: true
difficulty: Medium
rating: 1453
source: Biweekly Contest 162 Q2
tags:
    - Array
    - Binary Search
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [3634. Minimum Removals to Balance Array](https://leetcode.com/problems/minimum-removals-to-balance-array)

[中文文档](/solution/3600-3699/3634.Minimum%20Removals%20to%20Balance%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một mảng được gọi là <strong>cân bằng</strong> nếu giá trị của phần tử <strong>lớn nhất</strong> <strong>không vượt quá</strong> <code>k</code> lần giá trị của phần tử <strong>nhỏ nhất</strong>.</p>

<p>Bạn có thể xóa <strong>bất kỳ</strong> số lượng phần tử nào khỏi <code>nums</code>​​​​​​​ nhưng không được làm mảng <strong>rỗng</strong>.</p>

<p>Hãy trả về số lượng phần tử <strong>ít nhất</strong> cần xóa để mảng còn lại là mảng cân bằng.</p>

<p><strong>Lưu ý:</strong> Mảng có kích thước 1 được xem là cân bằng vì phần tử lớn nhất và nhỏ nhất bằng nhau, nên điều kiện luôn đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>nums[2] = 5</code> để được <code>nums = [2, 1]</code>.</li>
    <li>Lúc này <code>max = 2</code>, <code>min = 1</code> và <code>max &lt;= min * k</code> vì <code>2 &lt;= 1 * 2</code>. Do đó, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,6,2,9], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>nums[0] = 1</code> và <code>nums[3] = 9</code> để được <code>nums = [6, 2]</code>.</li>
    <li>Lúc này <code>max = 6</code>, <code>min = 2</code> và <code>max &lt;= min * k</code> vì <code>6 &lt;= 2 * 3</code>. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Vì <code>nums</code> đã cân bằng do <code>6 &lt;= 4 * 2</code>, nên không cần xóa phần tử nào.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mảng còn lại cân bằng khi và chỉ khi phần tử lớn nhất không vượt quá $k$ lần phần tử nhỏ nhất. Xóa phần tử tương đương với việc giữ lại một đoạn liên tiếp sau khi sắp xếp. Việc liệt kê các tập con là không thể thực hiện được với $n\le 10^5$.
>
> Sau khi sắp xếp, nếu $i$ là đầu trái (giá trị nhỏ nhất), thì đầu phải không thể vượt quá $k\cdot \textit{nums}[i]$. Tìm kiếm nhị phân giúp tìm chỉ số $j$ đầu tiên nằm sau giới hạn đó; đoạn $[i,j)$ có thể được giữ lại.
>
> Theo dõi cửa sổ dài nhất; đáp án là $n$ trừ đi độ dài cửa sổ đó. Việc sắp xếp khiến hai đầu cửa sổ chính là hai giá trị cực trị.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng, sau đó duyệt từng phần tử $\textit{nums}[i]$ từ nhỏ đến lớn và xem nó là giá trị nhỏ nhất của mảng cân bằng. Giá trị lớn nhất $\textit{max}$ của mảng cân bằng phải thỏa mãn $\textit{max} \leq \textit{nums}[i] \times k$. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $j$ của phần tử đầu tiên lớn hơn $\textit{nums}[i] \times k$. Khi đó, độ dài của mảng cân bằng là $j - i$. Ta lưu lại độ dài lớn nhất $\textit{cnt}$, và đáp án cuối cùng là độ dài mảng trừ đi $\textit{cnt}$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRemoval(self, nums: List[int], k: int) -> int:
        nums.sort()
        cnt = 0
        for i, x in enumerate(nums):
            j = bisect_right(nums, k * x)
            cnt = max(cnt, j - i)
        return len(nums) - cnt
```

#### Java

```java
class Solution {
    public int minRemoval(int[] nums, int k) {
        Arrays.sort(nums);
        int cnt = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            int j = n;
            if (1L * nums[i] * k <= nums[n - 1]) {
                j = Arrays.binarySearch(nums, nums[i] * k + 1);
                j = j < 0 ? -j - 1 : j;
            }
            cnt = Math.max(cnt, j - i);
        }
        return n - cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minRemoval(vector<int>& nums, int k) {
        ranges::sort(nums);
        int cnt = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            int j = n;
            if (1LL * nums[i] * k <= nums[n - 1]) {
                j = upper_bound(nums.begin(), nums.end(), 1LL * nums[i] * k) - nums.begin();
            }
            cnt = max(cnt, j - i);
        }
        return n - cnt;
    }
};
```

#### Go

```go
func minRemoval(nums []int, k int) int {
    sort.Ints(nums)
    n := len(nums)
    cnt := 0
    for i := 0; i < n; i++ {
        j := n
        if int64(nums[i])*int64(k) <= int64(nums[n-1]) {
            target := int64(nums[i])*int64(k) + 1
            j = sort.Search(n, func(x int) bool {
                return int64(nums[x]) >= target
            })
        }
        cnt = max(cnt, j-i)
    }
    return n - cnt
}
```

#### TypeScript

```ts
function minRemoval(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let cnt = 0;
    for (let i = 0; i < n; ++i) {
        let j = n;
        if (nums[i] * k <= nums[n - 1]) {
            const target = nums[i] * k + 1;
            j = _.sortedIndexBy(nums, target, x => x);
        }
        cnt = Math.max(cnt, j - i);
    }
    return n - cnt;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_removal(mut nums: Vec<i32>, k: i32) -> i32 {
        nums.sort();
        let mut cnt = 0;
        let n = nums.len();
        for i in 0..n {
            let mut j = n;
            let target = nums[i] as i64 * k as i64;
            if target <= nums[n - 1] as i64 {
                j = nums.partition_point(|&x| x as i64 <= target);
            }
            cnt = cnt.max(j - i);
        }
        (n - cnt) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp trước đó thực hiện tìm kiếm nhị phân cho mỗi đầu trái và phải trả thêm chi phí $\log n$. Đầu phải khả thi tăng đơn điệu theo đầu trái, nên ta có thể dùng hai con trỏ thay cho các lần tìm kiếm.
>
> Tăng $r$ khi $\textit{nums}[r]\le \textit{nums}[l]\cdot k$. Mỗi lần tăng $l$, $r$ chỉ di chuyển về bên phải. Đáp án là giá trị nhỏ nhất của $n-(r-l)$.
>
> Vẫn cần sắp xếp, nhưng việc duyệt chỉ tốn thời gian tuyến tính và tránh phải xử lý tràn số quanh cận tìm kiếm nhị phân.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng, sau đó dùng hai con trỏ để duy trì một cửa sổ trượt. Con trỏ trái $l$ duyệt từng phần tử $\textit{nums}[l]$ từ trái sang phải và xem nó là giá trị nhỏ nhất của mảng cân bằng. Con trỏ phải $r$ tiếp tục di chuyển sang phải cho đến khi $\textit{nums}[r]$ lớn hơn $\textit{nums}[l] \times k$. Khi đó, độ dài của mảng cân bằng là $r - l$, và số phần tử cần xóa là $n - (r - l)$. Ta lưu lại số lần xóa nhỏ nhất làm đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minRemoval(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = n = len(nums)
        r = 0
        for l in range(n):
            while r < n and nums[r] <= nums[l] * k:
                r += 1
            ans = min(ans, n - (r - l))
        return ans
```

#### Java

```java
class Solution {
    public int minRemoval(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int ans = n;
        int r = 0;
        for (int l = 0; l < n; l++) {
            while (r < n && nums[r] <= (long) nums[l] * k) {
                r++;
            }
            ans = Math.min(ans, n - (r - l));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minRemoval(vector<int>& nums, int k) {
        ranges::sort(nums);
        int n = nums.size();
        int ans = n;
        int r = 0;
        for (int l = 0; l < n; l++) {
            while (r < n && nums[r] <= (long long) nums[l] * k) {
                r++;
            }
            ans = min(ans, n - (r - l));
        }
        return ans;
    }
};
```

#### Go

```go
func minRemoval(nums []int, k int) int {
    sort.Ints(nums)
    n := len(nums)
    ans := n
    r := 0
    for l := 0; l < n; l++ {
        for r < n && nums[r] <= nums[l]*k {
            r++
        }
        ans = min(ans, n-(r-l))
    }
    return ans
}
```

#### TypeScript

```ts
function minRemoval(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = n;
    let r = 0;
    for (let l = 0; l < n; l++) {
        while (r < n && nums[r] <= nums[l] * k) {
            r++;
        }
        ans = Math.min(ans, n - (r - l));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_removal(mut nums: Vec<i32>, k: i32) -> i32 {
        nums.sort();
        let n = nums.len();
        let mut ans = n;
        let mut r = 0;
        let k = k as i64;
        for l in 0..n {
            while r < n && nums[r] as i64 <= nums[l] as i64 * k {
                r += 1;
            }
            ans = ans.min(n - (r - l));
        }
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
