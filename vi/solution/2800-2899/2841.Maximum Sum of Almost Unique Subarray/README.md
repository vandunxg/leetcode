---
comments: true
difficulty: Medium
rating: 1545
source: Biweekly Contest 112 Q3
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2841. Maximum Sum of Almost Unique Subarray](https://leetcode.com/problems/maximum-sum-of-almost-unique-subarray)

[中文文档](/solution/2800-2899/2841.Maximum%20Sum%20of%20Almost%20Unique%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên dương <code>m</code> và <code>k</code>.</p>

<p>Hãy trả về <em><strong>tổng lớn nhất</strong> trong tất cả các mảng con <strong>gần như duy nhất</strong> có độ dài </em><code>k</code><em> của</em> <code>nums</code>. Nếu không tồn tại mảng con nào như vậy, trả về <code>0</code>.</p>

<p>Một mảng con của <code>nums</code> được gọi là <strong>gần như duy nhất</strong> nếu nó chứa ít nhất <code>m</code> phần tử phân biệt.</p>

<p>Mảng con là một dãy phần tử <strong>liên tiếp không rỗng</strong> nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,6,7,3,1,7], m = 3, k = 4
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Có 3 mảng con gần như duy nhất có kích thước <code>k = 4</code>. Các mảng con này là [2, 6, 7, 3], [6, 7, 3, 1] và [7, 3, 1, 7]. Trong số các mảng con này, mảng có tổng lớn nhất là [2, 6, 7, 3], với tổng bằng 18.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,9,9,2,4,5,4], m = 1, k = 3
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong> Có 5 mảng con gần như duy nhất có kích thước k. Các mảng con này là [5, 9, 9], [9, 9, 2], [9, 2, 4], [2, 4, 5] và [4, 5, 4]. Trong số các mảng con này, mảng có tổng lớn nhất là [5, 9, 9], với tổng bằng 23.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1,2,1], m = 3, k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào có kích thước <code>k = 3</code> chứa ít nhất <code>m = 3</code> phần tử phân biệt trong mảng đã cho [1,2,1,2,1,2,1]. Do đó, không tồn tại mảng con gần như duy nhất nào và tổng lớn nhất là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m &lt;= k &lt;= nums.length</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt + Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng lớn nhất trong các cửa sổ độ dài $k$ chứa ít nhất $m$ giá trị phân biệt. Cửa sổ trượt với một bảng tần suất và tổng hiện tại sẽ cập nhật đáp án mỗi khi số lượng giá trị phân biệt đủ lớn.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $nums$, duy trì một cửa sổ có kích thước $k$, dùng một bảng băm $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong cửa sổ và dùng biến $s$ để tính tổng tất cả phần tử trong cửa sổ. Nếu số lượng phần tử khác nhau trong $cnt$ lớn hơn hoặc bằng $m$, ta cập nhật đáp án $ans = \max(ans, s)$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(k)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: List[int], m: int, k: int) -> int:
        cnt = Counter(nums[:k])
        s = sum(nums[:k])
        ans = s if len(cnt) >= m else 0
        for i in range(k, len(nums)):
            cnt[nums[i]] += 1
            cnt[nums[i - k]] -= 1
            s += nums[i] - nums[i - k]
            if cnt[nums[i - k]] == 0:
                cnt.pop(nums[i - k])
            if len(cnt) >= m:
                ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public long maxSum(List<Integer> nums, int m, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int n = nums.size();
        long s = 0;
        for (int i = 0; i < k; ++i) {
            cnt.merge(nums.get(i), 1, Integer::sum);
            s += nums.get(i);
        }
        long ans = cnt.size() >= m ? s : 0;
        for (int i = k; i < n; ++i) {
            cnt.merge(nums.get(i), 1, Integer::sum);
            if (cnt.merge(nums.get(i - k), -1, Integer::sum) == 0) {
                cnt.remove(nums.get(i - k));
            }
            s += nums.get(i) - nums.get(i - k);
            if (cnt.size() >= m) {
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
    long long maxSum(vector<int>& nums, int m, int k) {
        unordered_map<int, int> cnt;
        long long s = 0;
        int n = nums.size();
        for (int i = 0; i < k; ++i) {
            cnt[nums[i]]++;
            s += nums[i];
        }
        long long ans = cnt.size() >= m ? s : 0;
        for (int i = k; i < n; ++i) {
            cnt[nums[i]]++;
            if (--cnt[nums[i - k]] == 0) {
                cnt.erase(nums[i - k]);
            }
            s += nums[i] - nums[i - k];
            if (cnt.size() >= m) {
                ans = max(ans, s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int, m int, k int) int64 {
	cnt := map[int]int{}
	var s int64
	for _, x := range nums[:k] {
		cnt[x]++
		s += int64(x)
	}
	var ans int64
	if len(cnt) >= m {
		ans = s
	}
	for i := k; i < len(nums); i++ {
		cnt[nums[i]]++
		cnt[nums[i-k]]--
		if cnt[nums[i-k]] == 0 {
			delete(cnt, nums[i-k])
		}
		s += int64(nums[i]) - int64(nums[i-k])
		if len(cnt) >= m {
			ans = max(ans, s)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxSum(nums: number[], m: number, k: number): number {
    const n = nums.length;
    const cnt: Map<number, number> = new Map();
    let s = 0;
    for (let i = 0; i < k; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) || 0) + 1);
        s += nums[i];
    }
    let ans = cnt.size >= m ? s : 0;
    for (let i = k; i < n; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) || 0) + 1);
        cnt.set(nums[i - k], cnt.get(nums[i - k])! - 1);
        if (cnt.get(nums[i - k]) === 0) {
            cnt.delete(nums[i - k]);
        }
        s += nums[i] - nums[i - k];
        if (cnt.size >= m) {
            ans = Math.max(ans, s);
        }
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public long MaxSum(IList<int> nums, int m, int k) {
        Dictionary<int, int> cnt = new Dictionary<int, int>();
        int n = nums.Count;
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

        long ans = cnt.Count >= m ? s : 0;

        for (int i = k; i < n; ++i) {
            if (!cnt.ContainsKey(nums[i])) {
                cnt[nums[i]] = 1;
            }
            else {
                cnt[nums[i]]++;
            }
            if (cnt.ContainsKey(nums[i - k])) {
                cnt[nums[i - k]]--;
                if (cnt[nums[i - k]] == 0) {
                    cnt.Remove(nums[i - k]);
                }
            }

            s += nums[i] - nums[i - k];

            if (cnt.Count >= m) {
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
