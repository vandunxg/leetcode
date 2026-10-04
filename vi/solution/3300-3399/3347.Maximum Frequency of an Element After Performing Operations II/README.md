---
comments: true
difficulty: Hard
rating: 2155
source: Biweekly Contest 143 Q3
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [3347. Maximum Frequency of an Element After Performing Operations II](https://leetcode.com/problems/maximum-frequency-of-an-element-after-performing-operations-ii)

[Tài liệu tiếng Trung](/solution/3300-3399/3347.Maximum%20Frequency%20of%20an%20Element%20After%20Performing%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>numOperations</code>.</p>

<p>Bạn phải thực hiện <strong>phép toán</strong> <code>numOperations</code> lần trên <code>nums</code>, trong đó ở mỗi phép toán, bạn sẽ:</p>

<ul>
    <li>Chọn một chỉ số <code>i</code> <strong>chưa từng</strong> được chọn trong các phép toán trước đó.</li>
    <li>Cộng một số nguyên trong đoạn <code>[-k, k]</code> vào <code>nums[i]</code>.</li>
</ul>

<p>Trả về <strong>giá trị lớn nhất</strong> có thể đạt được đối với <span data-keyword="frequency-array">tần suất</span> của bất kỳ phần tử nào trong <code>nums</code> sau khi thực hiện các <strong>phép toán</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,5], k = 1, numOperations = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đạt được tần suất lớn nhất là hai bằng cách:</p>

<ul>
    <li>Cộng 0 vào <code>nums[1]</code>, sau đó <code>nums</code> trở thành <code>[1, 4, 5]</code>.</li>
    <li>Cộng -1 vào <code>nums[2]</code>, sau đó <code>nums</code> trở thành <code>[1, 4, 4]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,11,20,20], k = 5, numOperations = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đạt được tần suất lớn nhất là hai bằng cách:</p>

<ul>
    <li>Cộng 0 vào <code>nums[1]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= numOperations &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc giống phần I, nhưng các giá trị và $k$ có thể đạt $10^9$, nên mảng hiệu phải dùng sorted map thay vì một bảng dày.
>
> Phạm vi bao phủ không thay đổi: ta vẫn đánh dấu $x-k$, $x+k+1$ và $x$, rồi duyệt các tổng prefix.
>
> Tần suất một lần nữa là $\min(s,\textit{cnt}[x]+\textit{numOperations})$, nên code giống phần I.

<!-- thinking:end -->

Theo mô tả bài toán, với mỗi phần tử $x$ trong mảng $\textit{nums}$, ta có thể đổi nó thành bất kỳ số nguyên nào trong đoạn $[x-k, x+k]$. Ta muốn thực hiện các phép toán trên một số phần tử trong $\textit{nums}$ để tối đa hóa tần suất của một số nguyên nào đó trong mảng.

Bài toán có thể được biến đổi thành việc gộp tất cả các phần tử trong đoạn $[x-k, x+k]$ tương ứng với mỗi phần tử $x$, rồi tìm số nguyên nằm trong nhiều đoạn đã gộp nhất. Ta có thể thực hiện điều này bằng mảng hiệu.

Ta dùng một dictionary $d$ để ghi lại mảng hiệu. Với mỗi phần tử $x$, ta thực hiện các thao tác sau trên mảng hiệu:

- Cộng $1$ tại vị trí $x-k$, cho biết một đoạn mới bắt đầu từ vị trí này.
- Trừ $1$ tại vị trí $x+k+1$, cho biết một đoạn kết thúc trước vị trí này.
- Cộng $0$ tại vị trí $x$, đảm bảo vị trí $x$ tồn tại trong mảng hiệu để tính toán về sau.

Đồng thời, ta cần ghi lại số lần xuất hiện của mỗi phần tử trong mảng ban đầu, bằng cách dùng một dictionary $cnt$.

Tiếp theo, ta tính tổng prefix trên mảng hiệu để biết có bao nhiêu đoạn bao phủ mỗi vị trí. Với mỗi vị trí $x$, ta tính số đoạn bao phủ nó là $s$. Sau đó xét các trường hợp:

- Nếu $x$ xuất hiện trong mảng ban đầu, việc thực hiện phép toán trên chính $x$ là vô nghĩa. Vì vậy, có $s - cnt[x]$ phần tử khác có thể được đổi thành $x$ bằng các phép toán, nhưng số phép toán thực hiện được nhiều nhất là $\textit{numOperations}$. Do đó, tần suất lớn nhất tại vị trí này là $\textit{cnt}[x] + \min(s - \textit{cnt}[x], \textit{numOperations})$.
- Nếu $x$ không xuất hiện trong mảng ban đầu, ta chỉ có thể thực hiện nhiều nhất $\textit{numOperations}$ phép toán để đổi các phần tử khác thành $x$. Do đó, tần suất lớn nhất tại vị trí này là $\min(s, \textit{numOperations})$.

Kết hợp hai trường hợp trên, ta có thể biểu diễn thống nhất bằng $\min(s, \textit{cnt}[x] + \textit{numOperations})$.

Cuối cùng, ta duyệt qua tất cả các vị trí, tính tần suất lớn nhất tại mỗi vị trí, rồi lấy giá trị lớn nhất làm đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequency(self, nums: List[int], k: int, numOperations: int) -> int:
        cnt = defaultdict(int)
        d = defaultdict(int)
        for x in nums:
            cnt[x] += 1
            d[x] += 0
            d[x - k] += 1
            d[x + k + 1] -= 1
        ans = s = 0
        for x, t in sorted(d.items()):
            s += t
            ans = max(ans, min(s, cnt[x] + numOperations))
        return ans
```

#### Java

```java
class Solution {
    public int maxFrequency(int[] nums, int k, int numOperations) {
        Map<Integer, Integer> cnt = new HashMap<>();
        TreeMap<Integer, Integer> d = new TreeMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
            d.putIfAbsent(x, 0);
            d.merge(x - k, 1, Integer::sum);
            d.merge(x + k + 1, -1, Integer::sum);
        }
        int ans = 0, s = 0;
        for (var e : d.entrySet()) {
            int x = e.getKey(), t = e.getValue();
            s += t;
            ans = Math.max(ans, Math.min(s, cnt.getOrDefault(x, 0) + numOperations));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k, int numOperations) {
        unordered_map<int, int> cnt;
        map<int, int> d;

        for (int x : nums) {
            cnt[x]++;
            d[x];
            d[x - k]++;
            d[x + k + 1]--;
        }

        int ans = 0, s = 0;
        for (const auto& [x, t] : d) {
            s += t;
            ans = max(ans, min(s, cnt[x] + numOperations));
        }

        return ans;
    }
};
```

#### Go

```go
func maxFrequency(nums []int, k int, numOperations int) (ans int) {
    cnt := make(map[int]int)
    d := make(map[int]int)
    for _, x := range nums {
        cnt[x]++
        d[x] = d[x]
        d[x-k]++
        d[x+k+1]--
    }

    s := 0
    keys := make([]int, 0, len(d))
    for key := range d {
        keys = append(keys, key)
    }
    sort.Ints(keys)
    for _, x := range keys {
        s += d[x]
        ans = max(ans, min(s, cnt[x]+numOperations))
    }

    return
}
```

#### TypeScript

```ts
function maxFrequency(nums: number[], k: number, numOperations: number): number {
    const cnt: Record<number, number> = {};
    const d: Record<number, number> = {};
    for (const x of nums) {
        cnt[x] = (cnt[x] || 0) + 1;
        d[x] = d[x] || 0;
        d[x - k] = (d[x - k] || 0) + 1;
        d[x + k + 1] = (d[x + k + 1] || 0) - 1;
    }
    let [ans, s] = [0, 0];
    const keys = Object.keys(d)
        .map(Number)
        .sort((a, b) => a - b);
    for (const x of keys) {
        s += d[x];
        ans = Math.max(ans, Math.min(s, (cnt[x] || 0) + numOperations));
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::{HashMap, BTreeMap};

impl Solution {
    pub fn max_frequency(nums: Vec<i32>, k: i32, num_operations: i32) -> i32 {
        let mut cnt = HashMap::new();
        let mut d = BTreeMap::new();

        for &x in &nums {
            *cnt.entry(x).or_insert(0) += 1;
            d.entry(x).or_insert(0);
            *d.entry(x - k).or_insert(0) += 1;
            *d.entry(x + k + 1).or_insert(0) -= 1;
        }

        let mut ans = 0;
        let mut s = 0;
        for (&x, &t) in d.iter() {
            s += t;
            let cur = s.min(cnt.get(&x).copied().unwrap_or(0) + num_operations);
            ans = ans.max(cur);
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxFrequency(int[] nums, int k, int numOperations) {
        var cnt = new Dictionary<int, int>();
        var d = new SortedDictionary<int, int>();

        foreach (var x in nums) {
            if (!cnt.ContainsKey(x)) {
                cnt[x] = 0;
            }
            cnt[x]++;

            if (!d.ContainsKey(x)) {
                d[x] = 0;
            }
            if (!d.ContainsKey(x - k)) {
                d[x - k] = 0;
            }
            if (!d.ContainsKey(x + k + 1)) {
                d[x + k + 1] = 0;
            }

            d[x - k] += 1;
            d[x + k + 1] -= 1;
        }

        int ans = 0, s = 0;
        foreach (var kvp in d) {
            int x = kvp.Key, t = kvp.Value;
            s += t;
            int cur = Math.Min(s, (cnt.ContainsKey(x) ? cnt[x] : 0) + numOperations);
            ans = Math.Max(ans, cur);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
