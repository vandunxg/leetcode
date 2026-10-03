---
comments: true
difficulty: Hard
rating: 2217
source: Biweekly Contest 62 Q4
tags:
    - Array
    - Hash Table
    - Counting
    - Enumeration
    - Prefix Sum
---

<!-- problem:start -->

# [2025. Maximum Number of Ways to Partition an Array](https://leetcode.com/problems/maximum-number-of-ways-to-partition-an-array)

[中文文档](/solution/2000-2099/2025.Maximum%20Number%20of%20Ways%20to%20Partition%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>. Số cách <strong>chia</strong> <code>nums</code> là số lượng chỉ số <code>pivot</code> thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li><code>1 &lt;= pivot &lt; n</code></li>
	<li><code>nums[0] + nums[1] + ... + nums[pivot - 1] == nums[pivot] + nums[pivot + 1] + ... + nums[n - 1]</code></li>
</ul>

<p>Cho thêm một số nguyên <code>k</code>. Bạn có thể chọn đổi giá trị của <strong>một</strong> phần tử trong <code>nums</code> thành <code>k</code>, hoặc <strong>giữ nguyên</strong> mảng.</p>

<p>Trả về <em>số cách <strong>lớn nhất</strong> để <strong>chia</strong> </em><code>nums</code><em> sao cho thỏa mãn cả hai điều kiện sau khi đổi <strong>nhiều nhất</strong> một phần tử</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-1,2], k = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Một cách tối ưu là đổi nums[0] thành k. Mảng trở thành [<strong><u>3</u></strong>,-1,2].
Có một cách chia mảng:
- Với pivot = 2, ta có phép chia [3,-1 | 2]: 3 + -1 == 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cách tối ưu là giữ nguyên mảng.
Có hai cách chia mảng:
- Với pivot = 1, ta có phép chia [0 | 0,0]: 0 == 0 + 0.
- Với pivot = 2, ta có phép chia [0,0 | 0]: 0 + 0 == 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [22,4,-25,-20,-15,15,-16,7,19,-10,0,-13,-14], k = -33
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một cách tối ưu là đổi nums[2] thành k. Mảng trở thành [22,4,<u><strong>-33</strong></u>,-20,-15,15,-16,7,19,-10,0,-13,-14].
Có bốn cách chia mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= k, nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một điểm chia hợp lệ có hai nửa bằng nhau, tức là tổng tiền tố bằng một nửa tổng toàn mảng. Với $n \le 10^5$ và nhiều nhất một lần thay thế bằng $k$, việc duyệt lại các tổng tiền tố cho từng lần thay đổi là quá chậm.
>
> Khi không thay đổi, nếu tổng là số chẵn, ta đếm các tổng tiền tố bằng nửa tổng (không tính phần tử cuối). Khi thay $nums[i]$ bằng $k$, các tổng tiền tố bên trái không đổi còn các tổng tiền tố bên phải dịch đi một lượng $d=k-nums[i]$, nên ta có hai khóa đích.
>
> Hai map $left$ và $right$ theo dõi tần suất khi con trỏ chia di chuyển, nhờ đó tìm được giá trị lớn nhất trong thời gian tuyến tính.

<!-- thinking:end -->

Ta có thể tiền xử lý để thu được mảng tổng tiền tố $s$ tương ứng với mảng $nums$, trong đó $s[i]$ biểu diễn tổng của mảng $nums[0,...i-1]$. Vì vậy, tổng của toàn bộ phần tử trong mảng là $s[n - 1]$.

Nếu không thay đổi mảng $nums$, điều kiện để tổng của hai mảng con bằng nhau là $s[n - 1]$ phải là số chẵn. Nếu $s[n - 1]$ là số chẵn, ta tính $ans = \frac{right[s[n - 1] / 2]}{2}$.

Nếu thay đổi mảng $nums$, ta có thể lần lượt xét từng vị trí thay đổi $i$, đổi $nums[i]$ thành $k$, khi đó tổng của toàn mảng thay đổi một lượng là $d = k - nums[i]$. Lúc này, tổng của phần bên trái $i$ không đổi, nên phép chia hợp lệ phải thỏa mãn $s[i] = s[n - 1] + d - s[i]$, tức là $s[i] = \frac{s[n - 1] + d}{2}$. Mỗi tổng tiền tố của phần bên phải tăng thêm $d$, nên phép chia hợp lệ phải thỏa mãn $s[i] + d = s[n - 1] + d - (s[i] + d)$, tức là $s[i] = \frac{s[n - 1] - d}{2}$. Ta dùng các hash map $left$ và $right$ để ghi nhận số lần xuất hiện của mỗi tổng tiền tố tương ứng trong phần bên trái và phần bên phải. Khi đó, ta có thể tính $ans = max(ans, left[\frac{s[n - 1] + d}{2}]) + right[\frac{s[n - 1] - d}{2}]$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToPartition(self, nums: List[int], k: int) -> int:
        n = len(nums)
        s = [nums[0]] * n
        right = defaultdict(int)
        for i in range(1, n):
            s[i] = s[i - 1] + nums[i]
            right[s[i - 1]] += 1

        ans = 0
        if s[-1] % 2 == 0:
            ans = right[s[-1] // 2]

        left = defaultdict(int)
        for v, x in zip(s, nums):
            d = k - x
            if (s[-1] + d) % 2 == 0:
                t = left[(s[-1] + d) // 2] + right[(s[-1] - d) // 2]
                if ans < t:
                    ans = t
            left[v] += 1
            right[v] -= 1
        return ans
```

#### Java

```java
class Solution {
    public int waysToPartition(int[] nums, int k) {
        int n = nums.length;
        int[] s = new int[n];
        s[0] = nums[0];
        Map<Integer, Integer> right = new HashMap<>();
        for (int i = 0; i < n - 1; ++i) {
            right.merge(s[i], 1, Integer::sum);
            s[i + 1] = s[i] + nums[i + 1];
        }
        int ans = 0;
        if (s[n - 1] % 2 == 0) {
            ans = right.getOrDefault(s[n - 1] / 2, 0);
        }
        Map<Integer, Integer> left = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            int d = k - nums[i];
            if ((s[n - 1] + d) % 2 == 0) {
                int t = left.getOrDefault((s[n - 1] + d) / 2, 0)
                    + right.getOrDefault((s[n - 1] - d) / 2, 0);
                ans = Math.max(ans, t);
            }
            left.merge(s[i], 1, Integer::sum);
            right.merge(s[i], -1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToPartition(vector<int>& nums, int k) {
        int n = nums.size();
        long long s[n];
        s[0] = nums[0];
        unordered_map<long long, int> right;
        for (int i = 0; i < n - 1; ++i) {
            right[s[i]]++;
            s[i + 1] = s[i] + nums[i + 1];
        }
        int ans = 0;
        if (s[n - 1] % 2 == 0) {
            ans = right[s[n - 1] / 2];
        }
        unordered_map<long long, int> left;
        for (int i = 0; i < n; ++i) {
            int d = k - nums[i];
            if ((s[n - 1] + d) % 2 == 0) {
                int t = left[(s[n - 1] + d) / 2] + right[(s[n - 1] - d) / 2];
                ans = max(ans, t);
            }
            left[s[i]]++;
            right[s[i]]--;
        }
        return ans;
    }
};
```

#### Go

```go
func waysToPartition(nums []int, k int) (ans int) {
	n := len(nums)
	s := make([]int, n)
	s[0] = nums[0]
	right := map[int]int{}
	for i := range nums[:n-1] {
		right[s[i]]++
		s[i+1] = s[i] + nums[i+1]
	}
	if s[n-1]%2 == 0 {
		ans = right[s[n-1]/2]
	}
	left := map[int]int{}
	for i, x := range nums {
		d := k - x
		if (s[n-1]+d)%2 == 0 {
			t := left[(s[n-1]+d)/2] + right[(s[n-1]-d)/2]
			if ans < t {
				ans = t
			}
		}
		left[s[i]]++
		right[s[i]]--
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
