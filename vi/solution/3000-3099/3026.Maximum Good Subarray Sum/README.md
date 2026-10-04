---
comments: true
difficulty: Medium
rating: 1816
source: Biweekly Contest 123 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3026. Maximum Good Subarray Sum](https://leetcode.com/problems/maximum-good-subarray-sum)

[中文文档](/solution/3000-3099/3026.Maximum%20Good%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> có độ dài <code>n</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> được gọi là <strong>tốt</strong> nếu <strong>độ chênh lệch tuyệt đối</strong> giữa phần tử đầu tiên và phần tử cuối cùng của nó <strong>chính xác bằng</strong> <code>k</code>, nói cách khác, mảng con <code>nums[i..j]</code> là mảng con tốt nếu <code>|nums[i] - nums[j]| == k</code>.</p>

<p>Trả về <em>tổng <strong>lớn nhất</strong> của một mảng con <strong>tốt</strong> của </em><code>nums</code>. <em>Nếu không có mảng con tốt nào</em><em>, trả về </em><code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6], k = 1
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Độ chênh lệch tuyệt đối giữa phần tử đầu tiên và phần tử cuối cùng<!-- notionvc: 2a6d66c9-0149-4294-b267-8be9fe252de9 --> phải bằng 1 để một mảng con là mảng con tốt. Tất cả các mảng con tốt là: [1,2], [2,3], [3,4], [4,5] và [5,6]. Tổng mảng con lớn nhất là 11 đối với mảng con [5,6].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,3,2,4,5], k = 3
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Độ chênh lệch tuyệt đối giữa phần tử đầu tiên và phần tử cuối cùng<!-- notionvc: 2a6d66c9-0149-4294-b267-8be9fe252de9 --> phải bằng 3 để một mảng con là mảng con tốt. Tất cả các mảng con tốt là: [-1,3,2] và [2,4,5]. Tổng mảng con lớn nhất là 11 đối với mảng con [2,4,5].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,-3,-4], k = 2
<strong>Đầu ra:</strong> -6
<strong>Giải thích:</strong> Độ chênh lệch tuyệt đối giữa phần tử đầu tiên và phần tử cuối cùng<!-- notionvc: 2a6d66c9-0149-4294-b267-8be9fe252de9 --> phải bằng 2 để một mảng con là mảng con tốt. Tất cả các mảng con tốt là: [-1,-2,-3] và [-2,-3,-4]. Tổng mảng con lớn nhất là -6 đối với mảng con [-1,-2,-3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con tốt có hai đầu mút chênh lệch tuyệt đối bằng $k$, và ta cần tìm tổng lớn nhất. Vì $n \le 10^5$, ta không thể liệt kê tất cả các đầu mút.
>
> Tổng mảng con là hiệu của hai tổng tiền tố. Với một đầu mút phải $x$, đầu mút trái phải là $x-k$ hoặc $x+k$, và ta cần tổng tiền tố nhỏ nhất tại giá trị đó.
>
> Một hash map lưu tổng tiền tố nhỏ nhất (không bao gồm chính phần tử đó). Ta cập nhật đáp án từ tổng tiền tố hiện tại, sau đó đưa tổng tiền tố đó vào để xử lý giá trị tiếp theo.

<!-- thinking:end -->

Ta sử dụng một hash table $p$ để lưu tổng $s$ của mảng tổng tiền tố $nums[0..i-1]$ tương ứng với $nums[i]$. Nếu có nhiều $nums[i]$ giống nhau, ta chỉ giữ lại $s$ nhỏ nhất. Ban đầu, ta đặt $p[nums[0]]$ bằng $0$. Ngoài ra, ta sử dụng biến $s$ để lưu tổng tiền tố hiện tại, ban đầu $s = 0$. Khởi tạo đáp án $ans$ bằng $-\infty$.

Tiếp theo, ta duyệt qua $nums[i]$ và duy trì biến $s$ biểu diễn tổng của $nums[0..i]$. Nếu $nums[i] - k$ có trong $p$, ta đã tìm thấy một mảng con tốt và cập nhật đáp án thành $ans = \max(ans, s - p[nums[i] - k])$. Tương tự, nếu $nums[i] + k$ có trong $p$, ta cũng tìm thấy một mảng con tốt và cập nhật đáp án thành $ans = \max(ans, s - p[nums[i] + k])$. Sau đó, nếu $i + 1 \lt n$ và $nums[i + 1]$ không có trong $p$, hoặc $p[nums[i + 1]] \gt s$, ta đặt $p[nums[i + 1]]$ bằng $s$.

Cuối cùng, nếu $ans = -\infty$, ta trả về $0$, ngược lại trả về $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        ans = -inf
        p = {nums[0]: 0}
        s, n = 0, len(nums)
        for i, x in enumerate(nums):
            s += x
            if x - k in p:
                ans = max(ans, s - p[x - k])
            if x + k in p:
                ans = max(ans, s - p[x + k])
            if i + 1 < n and (nums[i + 1] not in p or p[nums[i + 1]] > s):
                p[nums[i + 1]] = s
        return 0 if ans == -inf else ans
```

#### Java

```java
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        Map<Integer, Long> p = new HashMap<>();
        p.put(nums[0], 0L);
        long s = 0;
        int n = nums.length;
        long ans = Long.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            if (p.containsKey(nums[i] - k)) {
                ans = Math.max(ans, s - p.get(nums[i] - k));
            }
            if (p.containsKey(nums[i] + k)) {
                ans = Math.max(ans, s - p.get(nums[i] + k));
            }
            if (i + 1 < n && (!p.containsKey(nums[i + 1]) || p.get(nums[i + 1]) > s)) {
                p.put(nums[i + 1], s);
            }
        }
        return ans == Long.MIN_VALUE ? 0 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        unordered_map<int, long long> p;
        p[nums[0]] = 0;
        long long s = 0;
        const int n = nums.size();
        long long ans = LONG_LONG_MIN;
        for (int i = 0;; ++i) {
            s += nums[i];
            auto it = p.find(nums[i] - k);
            if (it != p.end()) {
                ans = max(ans, s - it->second);
            }
            it = p.find(nums[i] + k);
            if (it != p.end()) {
                ans = max(ans, s - it->second);
            }
            if (i + 1 == n) {
                break;
            }
            it = p.find(nums[i + 1]);
            if (it == p.end() || it->second > s) {
                p[nums[i + 1]] = s;
            }
        }
        return ans == LONG_LONG_MIN ? 0 : ans;
    }
};
```

#### Go

```go
func maximumSubarraySum(nums []int, k int) int64 {
	p := map[int]int64{nums[0]: 0}
	var s int64 = 0
	n := len(nums)
	var ans int64 = math.MinInt64
	for i, x := range nums {
		s += int64(x)
		if t, ok := p[nums[i]-k]; ok {
			ans = max(ans, s-t)
		}
		if t, ok := p[nums[i]+k]; ok {
			ans = max(ans, s-t)
		}
		if i+1 == n {
			break
		}
		if t, ok := p[nums[i+1]]; !ok || s < t {
			p[nums[i+1]] = s
		}
	}
	if ans == math.MinInt64 {
		return 0
	}
	return ans
}
```

#### TypeScript

```ts
function maximumSubarraySum(nums: number[], k: number): number {
    const p: Map<number, number> = new Map();
    p.set(nums[0], 0);
    let ans: number = -Infinity;
    let s: number = 0;
    const n: number = nums.length;
    for (let i = 0; i < n; ++i) {
        s += nums[i];
        if (p.has(nums[i] - k)) {
            ans = Math.max(ans, s - p.get(nums[i] - k)!);
        }
        if (p.has(nums[i] + k)) {
            ans = Math.max(ans, s - p.get(nums[i] + k)!);
        }
        if (i + 1 < n && (!p.has(nums[i + 1]) || p.get(nums[i + 1])! > s)) {
            p.set(nums[i + 1], s);
        }
    }
    return ans === -Infinity ? 0 : ans;
}
```

#### C#

```cs
public class Solution {
    public long MaximumSubarraySum(int[] nums, int k) {
        Dictionary<int, long> p = new Dictionary<int, long>();
        p[nums[0]] = 0L;
        long s = 0;
        int n = nums.Length;
        long ans = long.MinValue;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            if (p.ContainsKey(nums[i] - k)) {
                ans = Math.Max(ans, s - p[nums[i] - k]);
            }
            if (p.ContainsKey(nums[i] + k)) {
                ans = Math.Max(ans, s - p[nums[i] + k]);
            }
            if (i + 1 < n && (!p.ContainsKey(nums[i + 1]) || p[nums[i + 1]] > s)) {
                p[nums[i + 1]] = s;
            }
        }
        return ans == long.MinValue ? 0 : ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
