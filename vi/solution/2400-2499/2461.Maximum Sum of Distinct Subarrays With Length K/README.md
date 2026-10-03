---
comments: true
difficulty: Medium
rating: 1552
source: Weekly Contest 318 Q2
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k)

[中文文档](/solution/2400-2499/2461.Maximum%20Sum%20of%20Distinct%20Subarrays%20With%20Length%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Hãy tìm tổng lớn nhất của các mảng con của <code>nums</code> thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Độ dài của mảng con bằng <code>k</code>, và</li>
	<li>Tất cả phần tử trong mảng con đều <strong>khác nhau</strong>.</li>
</ul>

<p>Hãy trả về <em>tổng lớn nhất của các mảng con thỏa mãn các điều kiện trên</em><em>.</em> Nếu không có mảng con nào thỏa mãn, trả về <code>0</code>.</p>

<p><em><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,4,2,9,9,9], k = 3
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Các mảng con của nums có độ dài 3 là:
- [1,5,4] thỏa mãn yêu cầu và có tổng bằng 10.
- [5,4,2] thỏa mãn yêu cầu và có tổng bằng 11.
- [4,2,9] thỏa mãn yêu cầu và có tổng bằng 15.
- [2,9,9] không thỏa mãn yêu cầu vì phần tử 9 bị lặp lại.
- [9,9,9] không thỏa mãn yêu cầu vì phần tử 9 bị lặp lại.
Ta trả về 15 vì đây là tổng lớn nhất của các mảng con thỏa mãn yêu cầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,4], k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Các mảng con của nums có độ dài 3 là:
- [4,4,4] không thỏa mãn yêu cầu vì phần tử 4 bị lặp lại.
Ta trả về 0 vì không có mảng con nào thỏa mãn yêu cầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm cửa sổ độ dài $k$ có tổng lớn nhất và các giá trị phân biệt, với $n\le 10^5$. Ta trượt một cửa sổ, đồng thời duy trì tổng và số lần xuất hiện của các phần tử; khi kích thước của map bằng $k$, ta cập nhật đáp án. Xóa một key khi số lần xuất hiện của nó giảm về 0.

<!-- thinking:end -->

Ta duy trì một cửa sổ trượt có độ dài $k$, dùng hash table $cnt$ để ghi lại số lần xuất hiện của mỗi số trong cửa sổ, đồng thời dùng biến $s$ để ghi lại tổng của tất cả các số trong cửa sổ. Mỗi lần trượt cửa sổ, nếu tất cả các số trong cửa sổ đều khác nhau, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        cnt = Counter(nums[:k])
        s = sum(nums[:k])
        ans = s if len(cnt) == k else 0
        for i in range(k, len(nums)):
            cnt[nums[i]] += 1
            cnt[nums[i - k]] -= 1
            if cnt[nums[i - k]] == 0:
                cnt.pop(nums[i - k])
            s += nums[i] - nums[i - k]
            if len(cnt) == k:
                ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        int n = nums.length;
        Map<Integer, Integer> cnt = new HashMap<>(k);
        long s = 0;
        for (int i = 0; i < k; ++i) {
            cnt.merge(nums[i], 1, Integer::sum);
            s += nums[i];
        }
        long ans = cnt.size() == k ? s : 0;
        for (int i = k; i < n; ++i) {
            cnt.merge(nums[i], 1, Integer::sum);
            if (cnt.merge(nums[i - k], -1, Integer::sum) == 0) {
                cnt.remove(nums[i - k]);
            }
            s += nums[i] - nums[i - k];
            if (cnt.size() == k) {
                ans = Math.max(ans, s);
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
    long long maximumSubarraySum(vector<int>& nums, int k) {
        using ll = long long;
        int n = nums.size();
        unordered_map<int, ll> cnt;
        ll s = 0;
        for (int i = 0; i < k; ++i) {
            ++cnt[nums[i]];
            s += nums[i];
        }
        ll ans = cnt.size() == k ? s : 0;
        for (int i = k; i < n; ++i) {
            ++cnt[nums[i]];
            if (--cnt[nums[i - k]] == 0) {
                cnt.erase(nums[i - k]);
            }
            s += nums[i] - nums[i - k];
            if (cnt.size() == k) {
                ans = max(ans, s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSubarraySum(nums []int, k int) (ans int64) {
	n := len(nums)
	cnt := map[int]int64{}
	var s int64
	for _, x := range nums[:k] {
		cnt[x]++
		s += int64(x)
	}
	if len(cnt) == k {
		ans = s
	}
	for i := k; i < n; i++ {
		cnt[nums[i]]++
		cnt[nums[i-k]]--
		if cnt[nums[i-k]] == 0 {
			delete(cnt, nums[i-k])
		}
		s += int64(nums[i] - nums[i-k])
		if len(cnt) == k && ans < s {
			ans = s
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumSubarraySum(nums: number[], k: number): number {
    const n = nums.length;
    const cnt: Map<number, number> = new Map();
    let s = 0;
    for (let i = 0; i < k; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) ?? 0) + 1);
        s += nums[i];
    }
    let ans = cnt.size === k ? s : 0;
    for (let i = k; i < n; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) ?? 0) + 1);
        cnt.set(nums[i - k], cnt.get(nums[i - k])! - 1);
        if (cnt.get(nums[i - k]) === 0) {
            cnt.delete(nums[i - k]);
        }
        s += nums[i] - nums[i - k];
        if (cnt.size === k) {
            ans = Math.max(ans, s);
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public long MaximumSubarraySum(int[] nums, int k) {
        int n = nums.Length;
        Dictionary<int, int> cnt = new Dictionary<int, int>(k);
        long s = 0;

        for (int i = 0; i < k; ++i) {
            if (!cnt.ContainsKey(nums[i])) {
                cnt[nums[i]] = 1;
            }
            else {
                cnt[nums[i]]++;
            }
            s += nums[i];
        }

        long ans = cnt.Count == k ? s : 0;

        for (int i = k; i < n; ++i) {
            if (!cnt.ContainsKey(nums[i])) {
                cnt[nums[i]] = 1;
            }
            else {
                cnt[nums[i]]++;
            }
            if (--cnt[nums[i - k]] == 0) {
                cnt.Remove(nums[i - k]);
            }

            s += nums[i] - nums[i - k];

            if (cnt.Count == k) {
                ans = Math.Max(ans, s);
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
