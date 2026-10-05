---
comments: true
difficulty: Medium
rating: 1504
source: Biweekly Contest 173 Q2
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [3795. Minimum Subarray Length With Distinct Sum At Least K](https://leetcode.com/problems/minimum-subarray-length-with-distinct-sum-at-least-k)

[中文文档](/solution/3700-3799/3795.Minimum%20Subarray%20Length%20With%20Distinct%20Sum%20At%20Least%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy trả về độ dài <strong>nhỏ nhất</strong> của một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> sao cho tổng các giá trị <strong>phân biệt</strong> xuất hiện trong mảng con đó (mỗi giá trị chỉ được tính một lần) <strong>ít nhất</strong> bằng <code>k</code>. Nếu không tồn tại mảng con thỏa mãn, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,3,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[2, 3]</code> có các phần tử phân biệt <code>{2, 3}</code>, tổng của chúng là <code>2 + 3 = 5</code>, lớn hơn hoặc bằng <code>k = 4</code>. Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,3,4], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[3, 2]</code> có các phần tử phân biệt <code>{3, 2}</code>, tổng của chúng là <code>3 + 2 = 5</code>, lớn hơn hoặc bằng <code>k = 5</code>. Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,4], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[5]</code> có các phần tử phân biệt <code>{5}</code>, tổng của chúng là <code>5</code>, lớn hơn hoặc bằng <code>k = 5</code>. Do đó, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Tổng các giá trị phân biệt là đơn điệu theo cửa sổ, vì vậy ta có thể dùng hai con trỏ. Bảng tần suất chỉ cộng một giá trị vào tổng khi số lần xuất hiện của nó tăng từ $0$ lên $1$; khi tổng đạt $k$, ta thu hẹp đầu trái và ghi nhận độ dài nhỏ nhất.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi phần tử trong cửa sổ hiện tại, và một biến $\textit{s}$ để ghi lại tổng các phần tử phân biệt trong cửa sổ hiện tại. Ta dùng hai con trỏ $l$ và $r$ biểu thị biên trái và biên phải của cửa sổ hiện tại, ban đầu cả hai đều trỏ đến đầu mảng. Ta khởi tạo biến $\textit{ans}$ để ghi lại độ dài nhỏ nhất của một cửa sổ thỏa mãn điều kiện, với giá trị ban đầu là $n + 1$, trong đó $n$ là độ dài mảng.

Ta liên tục di chuyển con trỏ phải $r$, thêm các phần tử mới vào cửa sổ và cập nhật $\textit{cnt}$ và $\textit{s}$. Khi $\textit{s}$ lớn hơn hoặc bằng $k$, ta cố gắng di chuyển con trỏ trái $l$ để thu hẹp cửa sổ, đồng thời cập nhật $\textit{cnt}$ và $\textit{s}$, cho đến khi $\textit{s}$ nhỏ hơn $k$. Trong quá trình này, ta ghi nhận độ dài nhỏ nhất của các cửa sổ thỏa mãn điều kiện.

Cuối cùng, nếu $\textit{ans} \gt n$, điều đó có nghĩa là không tồn tại cửa sổ hợp lệ, ta trả về $-1$; ngược lại, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLength(self, nums: List[int], k: int) -> int:
        cnt = defaultdict(int)
        n = len(nums)
        ans = n + 1
        s = l = 0
        for r, x in enumerate(nums):
            cnt[x] += 1
            if cnt[x] == 1:
                s += x
            while s >= k:
                ans = min(ans, r - l + 1)
                cnt[nums[l]] -= 1
                if cnt[nums[l]] == 0:
                    s -= nums[l]
                l += 1
        return -1 if ans > n else ans
```

#### Java

```java
class Solution {
    public int minLength(int[] nums, int k) {
        int n = nums.length;
        int ans = n + 1;
        Map<Integer, Integer> cnt = new HashMap<>();
        int l = 0;
        long s = 0;
        for (int r = 0; r < n; ++r) {
            if (cnt.merge(nums[r], 1, Integer::sum) == 1) {
                s += nums[r];
            }
            while (s >= k) {
                ans = Math.min(ans, r - l + 1);
                if (cnt.merge(nums[l], -1, Integer::sum) == 0) {
                    s -= nums[l];
                }
                ++l;
            }
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLength(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = n + 1;
        unordered_map<int, int> cnt;
        int l = 0;
        long long s = 0;
        for (int r = 0; r < n; ++r) {
            int x = nums[r];
            if (++cnt[x] == 1) {
                s += x;
            }
            while (s >= k) {
                ans = min(ans, r - l + 1);
                int y = nums[l];
                if (--cnt[y] == 0) {
                    s -= y;
                }
                ++l;
            }
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minLength(nums []int, k int) int {
	n := len(nums)
	ans := n + 1
	cnt := map[int]int{}
	l := 0
	var s int64 = 0
	for r := 0; r < n; r++ {
		cnt[nums[r]]++
		if cnt[nums[r]] == 1 {
			s += int64(nums[r])
		}
		for s >= int64(k) {
			if r-l+1 < ans {
				ans = r - l + 1
			}
			if cnt[nums[l]]--; cnt[nums[l]] == 0 {
				s -= int64(nums[l])
			}
			l++
		}
	}
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minLength(nums: number[], k: number): number {
    const n = nums.length;
    let ans = n + 1;
    const cnt = new Map<number, number>();
    let l = 0;
    let s = 0;
    for (let r = 0; r < n; ++r) {
        cnt.set(nums[r], (cnt.get(nums[r]) ?? 0) + 1);
        if (cnt.get(nums[r]) === 1) {
            s += nums[r];
        }
        while (s >= k) {
            ans = Math.min(ans, r - l + 1);
            cnt.set(nums[l], (cnt.get(nums[l]) ?? 0) - 1);
            if (cnt.get(nums[l]) === 0) {
                s -= nums[l];
            }
            ++l;
        }
    }
    return ans > n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
