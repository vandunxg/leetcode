---
comments: true
difficulty: Medium
rating: 1623
source: Weekly Contest 161 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1248. Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays)

[中文文档](/solution/1200-1299/1248.Count%20Number%20of%20Nice%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. Một mảng con liên tiếp được gọi là <strong>nice</strong> nếu chứa đúng <code>k</code> số lẻ.</p>

<p>Trả về <em>số lượng mảng con <strong>nice</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,1,1], k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai mảng con duy nhất có 3 số lẻ là [1,1,2,1] và [1,2,1,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng không có số lẻ nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,1,2,2,1,2,2,2], k = 2
<strong>Đầu ra:</strong> 16
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^5</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các mảng con có đúng $k$ số lẻ, với $n \le 5\times 10^4$. Nếu quy ước số lẻ là $1$ và số chẵn là $0$, các mảng con cần tìm là những đoạn mà tổng prefix sum chênh nhau đúng $k$.
>
> Khi duyệt, ta duy trì số lượng số lẻ $t$, tra xem giá trị $t-k$ đã xuất hiện bao nhiêu lần rồi ghi nhận $t$. Prefix sum biến điều kiện “có đúng $k$ số lẻ” thành một lần tra cứu trong map.

<!-- thinking:end -->

Bài toán yêu cầu đếm số mảng con chứa đúng $k$ số lẻ. Với mỗi prefix của mảng, ta tính số số lẻ $t$ và lưu tần suất vào mảng hoặc hash table $cnt$. Với mỗi prefix hiện tại, ta chỉ cần tìm số prefix trước đó có $t-k$ số lẻ; đó chính là số mảng con kết thúc tại prefix hiện tại thỏa mãn yêu cầu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubarrays(self, nums: List[int], k: int) -> int:
        cnt = Counter({0: 1})
        ans = t = 0
        for v in nums:
            t += v & 1
            ans += cnt[t - k]
            cnt[t] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] cnt = new int[n + 1];
        cnt[0] = 1;
        int ans = 0, t = 0;
        for (int v : nums) {
            t += v & 1;
            if (t - k >= 0) {
                ans += cnt[t - k];
            }
            cnt[t]++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> cnt(n + 1);
        cnt[0] = 1;
        int ans = 0, t = 0;
        for (int& v : nums) {
            t += v & 1;
            if (t - k >= 0) {
                ans += cnt[t - k];
            }
            cnt[t]++;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubarrays(nums []int, k int) (ans int) {
	n := len(nums)
	cnt := make([]int, n+1)
	cnt[0] = 1
	t := 0
	for _, v := range nums {
		t += v & 1
		if t >= k {
			ans += cnt[t-k]
		}
		cnt[t]++
	}
	return
}
```

#### TypeScript

```ts
function numberOfSubarrays(nums: number[], k: number): number {
    const n = nums.length;
    const cnt = Array(n + 1).fill(0);
    cnt[0] = 1;
    let [t, ans] = [0, 0];
    for (const v of nums) {
        t += v & 1;
        ans += cnt[t - k] ?? 0;
        cnt[t] += 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
