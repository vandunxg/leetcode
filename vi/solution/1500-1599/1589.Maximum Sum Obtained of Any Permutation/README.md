---
comments: true
difficulty: Medium
rating: 1871
source: Biweekly Contest 35 Q2
tags:
    - Greedy
    - Array
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [1589. Maximum Sum Obtained of Any Permutation](https://leetcode.com/problems/maximum-sum-obtained-of-any-permutation)

[中文文档](/solution/1500-1599/1589.Maximum%20Sum%20Obtained%20of%20Any%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và mảng <code>requests</code>, trong đó <code>requests[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>. Yêu cầu thứ <code>i<sup>th</sup></code> cần tổng <code>nums[start<sub>i</sub>] + nums[start<sub>i</sub> + 1] + ... + nums[end<sub>i</sub> - 1] + nums[end<sub>i</sub>]</code>. Cả <code>start<sub>i</sub></code> và <code>end<sub>i</sub></code> đều được <em>đánh số từ 0</em>.</p>

<p>Trả về <em>tổng lớn nhất của tất cả yêu cầu trong mọi <strong>hoán vị</strong> của</em> <code>nums</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], requests = [[1,3],[0,1]]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Một hoán vị của nums là [2,1,3,4,5] với kết quả sau: 
requests[0] -&gt; nums[1] + nums[2] + nums[3] = 1 + 3 + 4 = 8
requests[1] -&gt; nums[0] + nums[1] = 2 + 1 = 3
Tổng: 8 + 3 = 11.
Một hoán vị có tổng lớn hơn là [3,5,4,2,1] với kết quả sau:
requests[0] -&gt; nums[1] + nums[2] + nums[3] = 5 + 4 + 2 = 11
requests[1] -&gt; nums[0] + nums[1] = 3 + 5  = 8
Tổng: 11 + 8 = 19, là kết quả tốt nhất có thể đạt được.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6], requests = [[0,1]]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Hoán vị cho tổng lớn nhất là [6,5,4,3,2,1], với tổng yêu cầu là [11].</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,10], requests = [[0,2],[1,3],[1,1]]
<strong>Đầu ra:</strong> 47
<strong>Giải thích:</strong> Hoán vị cho tổng lớn nhất là [4,10,5,3,2,1], với tổng các yêu cầu [19,18,10].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i]&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= requests.length &lt;=&nbsp;10<sup>5</sup></code></li>
	<li><code>requests[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub>&nbsp;&lt;= end<sub>i</sub>&nbsp;&lt;&nbsp;n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Hoán vị $nums$ để tối đa hóa tổng của tất cả các tổng đoạn được yêu cầu. Cả $n$ và số lượng yêu cầu đều có thể đạt $10^5$. Chỉ số được bao phủ thường xuyên hơn nên nhận giá trị lớn hơn.
>
> Mảng hiệu cập nhật mỗi đoạn $[l,r]$ trong $O(1)$; sau đó tổng tiền tố cho biết số lần mỗi chỉ số được bao phủ. Sắp xếp số lần bao phủ cùng $nums$ rồi nhân các cặp tương ứng sẽ gán giá trị lớn nhất cho những vị trí xuất hiện nhiều nhất.

<!-- thinking:end -->

Ta nhận thấy mỗi truy vấn trả về tổng của mọi phần tử trong đoạn truy vấn $[l, r]$. Bài toán yêu cầu tổng lớn nhất của kết quả tất cả truy vấn, nên ta cần cộng dồn số lần mỗi chỉ số được truy vấn. Vì vậy, chỉ số $i$ xuất hiện càng nhiều thì nên được gán giá trị càng lớn; phần tử $\textit{nums}[i]$ có tần suất cao hơn nên được ưu tiên. Hai lần xét chỉ số $i$ và $i$ đều tuân theo nguyên tắc này.

Do đó, ta dùng mảng hiệu để đếm số lần mỗi chỉ số xuất hiện trong các truy vấn, rồi sắp xếp các số đếm và mảng $\textit{nums}$ theo thứ tự tăng dần. Như vậy, chỉ số xuất hiện thường xuyên hơn sẽ được gán giá trị $\textit{nums}[i]$ lớn hơn. Sau đó, ta nhân mỗi giá trị với số lần xuất hiện tương ứng rồi cộng lại để thu được tổng lớn nhất.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumRangeQuery(self, nums: List[int], requests: List[List[int]]) -> int:
        n = len(nums)
        d = [0] * n
        for l, r in requests:
            d[l] += 1
            if r + 1 < n:
                d[r + 1] -= 1
        for i in range(1, n):
            d[i] += d[i - 1]
        nums.sort()
        d.sort()
        mod = 10**9 + 7
        return sum(a * b for a, b in zip(nums, d)) % mod
```

#### Java

```java
class Solution {
    public int maxSumRangeQuery(int[] nums, int[][] requests) {
        int n = nums.length;
        int[] d = new int[n];
        for (var req : requests) {
            int l = req[0], r = req[1];
            d[l]++;
            if (r + 1 < n) {
                d[r + 1]--;
            }
        }
        for (int i = 1; i < n; ++i) {
            d[i] += d[i - 1];
        }
        Arrays.sort(nums);
        Arrays.sort(d);
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = (ans + 1L * nums[i] * d[i]) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumRangeQuery(vector<int>& nums, vector<vector<int>>& requests) {
        int n = nums.size();
        int d[n];
        memset(d, 0, sizeof(d));
        for (auto& req : requests) {
            int l = req[0], r = req[1];
            d[l]++;
            if (r + 1 < n) {
                d[r + 1]--;
            }
        }
        for (int i = 1; i < n; ++i) {
            d[i] += d[i - 1];
        }
        sort(nums.begin(), nums.end());
        sort(d, d + n);
        long long ans = 0;
        const int mod = 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            ans = (ans + 1LL * nums[i] * d[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func maxSumRangeQuery(nums []int, requests [][]int) (ans int) {
	n := len(nums)
	d := make([]int, n)
	for _, req := range requests {
		l, r := req[0], req[1]
		d[l]++
		if r+1 < n {
			d[r+1]--
		}
	}
	for i := 1; i < n; i++ {
		d[i] += d[i-1]
	}
	sort.Ints(nums)
	sort.Ints(d)
	const mod = 1e9 + 7
	for i, a := range nums {
		b := d[i]
		ans = (ans + a*b) % mod
	}
	return
}
```

#### TypeScript

```ts
function maxSumRangeQuery(nums: number[], requests: number[][]): number {
    const n = nums.length;
    const d = new Array(n).fill(0);
    for (const [l, r] of requests) {
        d[l]++;
        if (r + 1 < n) {
            d[r + 1]--;
        }
    }
    for (let i = 1; i < n; ++i) {
        d[i] += d[i - 1];
    }
    nums.sort((a, b) => a - b);
    d.sort((a, b) => a - b);
    let ans = 0;
    const mod = 10 ** 9 + 7;
    for (let i = 0; i < n; ++i) {
        ans = (ans + nums[i] * d[i]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
