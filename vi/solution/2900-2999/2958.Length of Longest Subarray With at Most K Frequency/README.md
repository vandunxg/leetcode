---
comments: true
difficulty: Medium
rating: 1535
source: Biweekly Contest 119 Q3
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2958. Length of Longest Subarray With at Most K Frequency](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency)

[中文文档](/solution/2900-2999/2958.Length%20of%20Longest%20Subarray%20With%20at%20Most%20K%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p><strong>Tần suất</strong> của một phần tử <code>x</code> là số lần phần tử đó xuất hiện trong một mảng.</p>

<p>Một mảng được gọi là <strong>hợp lệ</strong> nếu tần suất của mỗi phần tử trong mảng đó <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</p>

<p>Trả về <em>độ dài của <strong>mảng con hợp lệ</strong> <strong>dài nhất</strong> của</em> <code>nums</code><em>.</em></p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,1,2,3,1,2], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Mảng con hợp lệ dài nhất có thể là [1,2,3,1,2,3] vì các giá trị 1, 2 và 3 xuất hiện nhiều nhất hai lần trong mảng con này. Lưu ý rằng các mảng con [2,3,1,2,3,1] và [3,1,2,3,1,2] cũng hợp lệ.
Có thể chứng minh rằng không có mảng con hợp lệ nào có độ dài lớn hơn 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1,2,1,2], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mảng con hợp lệ dài nhất có thể là [1,2] vì các giá trị 1 và 2 xuất hiện nhiều nhất một lần trong mảng con này. Lưu ý rằng mảng con [2,1] cũng hợp lệ.
Có thể chứng minh rằng không có mảng con hợp lệ nào có độ dài lớn hơn 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5,5,5,5,5], k = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Mảng con hợp lệ dài nhất có thể là [5,5,5,5] vì giá trị 5 xuất hiện 4 lần trong mảng con này.
Có thể chứng minh rằng không có mảng con hợp lệ nào có độ dài lớn hơn 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con dài nhất trong đó không có giá trị nào xuất hiện quá $k$ lần. Với $n \le 10^5$, không thể duyệt mọi điểm kết thúc. Ràng buộc này có tính đơn điệu: tăng $r$ chỉ có thể làm ràng buộc bị phá vỡ, còn tăng $l$ chỉ có thể khôi phục nó.
>
> Dùng hash map để đếm tần suất; sau khi thêm $x$, thu hẹp $l$ khi $cnt[x]>k$. Cập nhật độ dài khi cửa sổ hợp lệ.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ $l$ và $r$ để biểu diễn hai đầu trái và phải của mảng con, ban đầu cả hai con trỏ đều trỏ đến phần tử đầu tiên của mảng.

Tiếp theo, ta duyệt qua từng phần tử $x$ trong mảng $nums$. Với mỗi phần tử $x$, ta tăng số lần xuất hiện của $x$, sau đó kiểm tra xem mảng con hiện tại có thỏa mãn yêu cầu hay không. Nếu mảng con hiện tại không thỏa mãn yêu cầu, ta di chuyển con trỏ $l$ sang phải một bước và giảm số lần xuất hiện của $nums[l]$, cho đến khi mảng con hiện tại thỏa mãn yêu cầu. Sau đó, ta cập nhật đáp án $ans = \max(ans, r - l + 1)$. Tiếp tục lặp cho đến khi $r$ đi đến cuối mảng.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarrayLength(self, nums: List[int], k: int) -> int:
        ans = l = 0
        cnt = defaultdict(int)
        for r, x in enumerate(nums):
            cnt[x] += 1
            while cnt[x] > k:
                cnt[nums[l]] -= 1
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxSubarrayLength(int[] nums, int k) {
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int l = 0, r = 0; r < nums.length; ++r) {
            cnt.merge(nums[r], 1, Integer::sum);
            while (cnt.get(nums[r]) > k) {
                cnt.merge(nums[l++], -1, Integer::sum);
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubarrayLength(vector<int>& nums, int k) {
        int ans = 0;
        unordered_map<int, int> cnt;
        for (int l = 0, r = 0; r < nums.size(); ++r) {
            ++cnt[nums[r]];
            while (cnt[nums[r]] > k) {
                --cnt[nums[l++]];
            }
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubarrayLength(nums []int, k int) (ans int) {
	cnt := make(map[int]int)
	for l, r := 0, 0; r < len(nums); r++ {
		cnt[nums[r]]++
		for cnt[nums[r]] > k {
			cnt[nums[l]]--
			l++
		}
		ans = max(ans, r-l+1)
	}
	return
}
```

#### TypeScript

```ts
function maxSubarrayLength(nums: number[], k: number): number {
    let ans = 0;
    const cnt = new Map<number, number>();
    for (let l = 0, r = 0; r < nums.length; ++r) {
        cnt.set(nums[r], (cnt.get(nums[r]) ?? 0) + 1);
        while (cnt.get(nums[r])! > k) {
            cnt.set(nums[l], cnt.get(nums[l])! - 1);
            ++l;
        }
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_subarray_length(nums: Vec<i32>, k: i32) -> i32 {
        let mut ans = 0;
        let mut cnt = std::collections::HashMap::new();

        let mut l = 0;
        for r in 0..nums.len() {
            *cnt.entry(nums[r]).or_insert(0) += 1;

            while cnt[&nums[r]] > k {
                *cnt.get_mut(&nums[l]).unwrap() -= 1;
                l += 1;
            }

            ans = ans.max((r - l + 1) as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
