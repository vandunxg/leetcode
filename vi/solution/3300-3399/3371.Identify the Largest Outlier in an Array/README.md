---
comments: true
difficulty: Medium
rating: 1643
source: Weekly Contest 426 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [3371. Identify the Largest Outlier in an Array](https://leetcode.com/problems/identify-the-largest-outlier-in-an-array)

[中文文档](/solution/3300-3399/3371.Identify%20the%20Largest%20Outlier%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Mảng này chứa <code>n</code> phần tử, trong đó <strong>chính xác</strong> <code>n - 2</code> phần tử là <strong>các</strong><strong> số đặc biệt</strong>. Một trong <strong>hai</strong> phần tử còn lại là <em>tổng</em> của các <strong>số đặc biệt</strong> này, phần tử còn lại là một <strong>outlier</strong>.</p>

<p><strong>Outlier</strong> được định nghĩa là một số <em>không phải</em> là một trong các số đặc biệt ban đầu và cũng <em>không phải</em> là phần tử biểu diễn tổng của các số đó.</p>

<p><strong>Lưu ý</strong> rằng các số đặc biệt, phần tử tổng và outlier phải có <strong>chỉ số khác nhau</strong>, nhưng <em>có thể</em> có <strong>cùng giá trị</strong>.</p>

<p>Trả về <strong>outlier</strong> <strong>tiềm năng</strong> <strong>lớn nhất</strong> trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đặc biệt có thể là 2 và 3, khi đó tổng của chúng là 5 và outlier là 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-2,-1,-3,-6,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đặc biệt có thể là -2, -1 và -3, khi đó tổng của chúng là -6 và outlier là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,1,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số đặc biệt có thể là 1, 1, 1, 1 và 1, khi đó tổng của chúng là 5 và 5 còn lại là outlier.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <strong>ít nhất một</strong> outlier tiềm năng tồn tại trong <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mảng gồm các giá trị thông thường, tổng của chúng và một outlier. Với $n \le 10^5$, ta thử từng outlier trong $O(1)$.
>
> Gọi tổng của toàn bộ mảng là $s$. Nếu $x$ là outlier, $s-x$ phải là số chẵn và $(s-x)/2$ phải xuất hiện trong các phần tử còn lại dưới dạng phần tử tổng.
>
> Dùng một frequency map để kiểm tra trường hợp này; nếu tổng bằng $x$ thì cần có hai bản sao. Ta giữ lại $x$ hợp lệ lớn nhất.

<!-- thinking:end -->

Ta dùng một hash table $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi phần tử trong mảng $\textit{nums}$.

Tiếp theo, ta liệt kê từng phần tử $x$ trong mảng $\textit{nums}$ làm outlier tiềm năng. Với mỗi $x$, ta tính tổng $t$ của tất cả phần tử trong mảng $\textit{nums}$ ngoại trừ $x$. Nếu $t$ không chẵn hoặc một nửa của $t$ không có trong $\textit{cnt}$, thì $x$ không thỏa điều kiện và ta bỏ qua $x$. Ngược lại, nếu $x$ khác một nửa của $t$, hoặc $x$ xuất hiện nhiều hơn một lần trong $\textit{cnt}$, thì $x$ là một outlier tiềm năng và ta cập nhật đáp án.

Sau khi liệt kê tất cả phần tử, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLargestOutlier(self, nums: List[int]) -> int:
        s = sum(nums)
        cnt = Counter(nums)
        ans = -inf
        for x, v in cnt.items():
            t = s - x
            if t % 2 or cnt[t // 2] == 0:
                continue
            if x != t // 2 or v > 1:
                ans = max(ans, x)
        return ans
```

#### Java

```java
class Solution {
    public int getLargestOutlier(int[] nums) {
        int s = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            s += x;
            cnt.merge(x, 1, Integer::sum);
        }
        int ans = Integer.MIN_VALUE;
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            int t = s - x;
            if (t % 2 != 0 || !cnt.containsKey(t / 2)) {
                continue;
            }
            if (x != t / 2 || v > 1) {
                ans = Math.max(ans, x);
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
    int getLargestOutlier(vector<int>& nums) {
        int s = 0;
        unordered_map<int, int> cnt;
        for (int x : nums) {
            s += x;
            cnt[x]++;
        }
        int ans = INT_MIN;
        for (auto [x, v] : cnt) {
            int t = s - x;
            if (t % 2 || !cnt.contains(t / 2)) {
                continue;
            }
            if (x != t / 2 || v > 1) {
                ans = max(ans, x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getLargestOutlier(nums []int) int {
	s := 0
	cnt := map[int]int{}
	for _, x := range nums {
		s += x
		cnt[x]++
	}
	ans := math.MinInt32
	for x, v := range cnt {
		t := s - x
		if t%2 != 0 || cnt[t/2] == 0 {
			continue
		}
		if x != t/2 || v > 1 {
			ans = max(ans, x)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function getLargestOutlier(nums: number[]): number {
    let s = 0;
    const cnt: Record<number, number> = {};
    for (const x of nums) {
        s += x;
        cnt[x] = (cnt[x] || 0) + 1;
    }
    let ans = -Infinity;
    for (const [x, v] of Object.entries(cnt)) {
        const t = s - +x;
        if (t % 2 || !cnt.hasOwnProperty((t / 2) | 0)) {
            continue;
        }
        if (+x != ((t / 2) | 0) || v > 1) {
            ans = Math.max(ans, +x);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn get_largest_outlier(nums: Vec<i32>) -> i32 {
        let mut s = 0;
        let mut cnt = HashMap::new();
        for &x in &nums {
            s += x;
            *cnt.entry(x).or_insert(0) += 1;
        }

        let mut ans = i32::MIN;
        for (&x, &v) in &cnt {
            let t = s - x;
            if t % 2 != 0 {
                continue;
            }
            let y = t / 2;
            if let Some(&count_y) = cnt.get(&y) {
                if x != y || v > 1 {
                    ans = ans.max(x);
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int GetLargestOutlier(int[] nums) {
        int s = 0;
        var cnt = new Dictionary<int, int>();
        foreach (int x in nums) {
            s += x;
            if (!cnt.ContainsKey(x)) cnt[x] = 0;
            cnt[x]++;
        }

        int ans = int.MinValue;
        foreach (var kv in cnt) {
            int x = kv.Key, v = kv.Value;
            int t = s - x;
            if (t % 2 != 0) continue;
            int y = t / 2;
            if (cnt.ContainsKey(y)) {
                if (x != y || v > 1) {
                    ans = Math.Max(ans, x);
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
