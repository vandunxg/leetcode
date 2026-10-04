---
comments: true
difficulty: Medium
rating: 1903
source: Weekly Contest 338 Q3
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2602. Minimum Operations to Make All Array Elements Equal](https://leetcode.com/problems/minimum-operations-to-make-all-array-elements-equal)

[中文文档](/solution/2600-2699/2602.Minimum%20Operations%20to%20Make%20All%20Array%20Elements%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>queries</code> có kích thước <code>m</code>. Với truy vấn thứ <code>i<sup>th</sup></code>, bạn muốn đưa tất cả phần tử của <code>nums</code> về bằng <code> queries[i]</code>. Bạn có thể thực hiện thao tác sau trên mảng <strong>bao nhiêu lần cũng được</strong>:</p>

<ul>
	<li><strong>Tăng</strong> hoặc <strong>giảm</strong> một phần tử của mảng đi <code>1</code>.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em> có kích thước </em><code>m</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>số thao tác nhỏ nhất</strong> cần thực hiện để đưa tất cả phần tử của </em><code>nums</code><em> về bằng </em><code>queries[i]</code>.</p>

<p><strong>Lưu ý</strong> rằng sau mỗi truy vấn, mảng được đặt lại về trạng thái ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,6,8], queries = [1,5]
<strong>Đầu ra:</strong> [14,10]
<strong>Giải thích:</strong> Với truy vấn đầu tiên, ta có thể thực hiện các thao tác sau:
- Giảm nums[0] 2 lần, để nums = [1,1,6,8].
- Giảm nums[2] 5 lần, để nums = [1,1,1,8].
- Giảm nums[3] 7 lần, để nums = [1,1,1,1].
Tổng số thao tác cho truy vấn đầu tiên là 2 + 5 + 7 = 14.
Với truy vấn thứ hai, ta có thể thực hiện các thao tác sau:
- Tăng nums[0] 2 lần, để nums = [5,1,6,8].
- Tăng nums[1] 4 lần, để nums = [5,5,6,8].
- Giảm nums[2] 1 lần, để nums = [5,5,5,8].
- Giảm nums[3] 3 lần, để nums = [5,5,5,5].
Tổng số thao tác cho truy vấn thứ hai là 2 + 4 + 1 + 3 = 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,9,6,3], queries = [10]
<strong>Đầu ra:</strong> [20]
<strong>Giải thích:</strong> Ta có thể tăng mỗi giá trị trong mảng lên 10. Tổng số thao tác là 8 + 1 + 4 + 7 = 20.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>m == queries.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], queries[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + tổng tiền tố + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn đưa mọi phần tử về cùng một giá trị đích $x$, mỗi thao tác tăng hoặc giảm một đơn vị. Duyệt $nums$ cho từng truy vấn có độ phức tạp $O(nq)$ và không phù hợp với $n,q \le 10^5$.
>
> Các phần tử không ảnh hưởng lẫn nhau: chi phí là $\sum |nums_i-x|$. Sau khi sắp xếp, $x$ chia mảng thành các giá trị nhỏ hơn và lớn hơn $x$, đồng thời cả hai loại độ lệch tuyệt đối đều có thể tính trong $O(1)$ nhờ tổng tiền tố.
>
> Vì vậy, ta sắp xếp, xây dựng các tổng tiền tố, rồi với mỗi $x$ dùng tìm kiếm nhị phân để tìm chỉ số phân tách và cộng chi phí tăng và giảm.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp mảng $nums$ và tính mảng tổng tiền tố $s$ có độ dài $n+1$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên trong mảng $nums$.

Tiếp theo, ta duyệt từng truy vấn $queries[i]$; ta cần giảm tất cả phần tử lớn hơn $queries[i]$ về $queries[i]$, đồng thời tăng tất cả phần tử nhỏ hơn $queries[i]$ về $queries[i]$.

Ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $i$ của phần tử đầu tiên trong mảng $nums$ lớn hơn $queries[i]$. Có $n-i$ phần tử cần được giảm về $queries[i]$, và tổng của các phần tử này là $s[n]-s[i]$. Các phần tử này cần được giảm đi $n-i$ $queries[i]$, nên tổng số thao tác để giảm các phần tử này về $queries[i]$ là $s[n]-s[i]-(n-i)\times queries[i]$.

Tương tự, ta có thể tìm chỉ số $i$ của phần tử đầu tiên trong mảng $nums$ lớn hơn hoặc bằng $queries[i]$. Có $i$ phần tử cần được tăng về $queries[i]$, và tổng của các phần tử này là $s[i]$. Do đó, tổng số thao tác để tăng các phần tử này về $queries[i]$ là $queries[i]\times i-s[i]$.

Cuối cùng, cộng hai tổng số thao tác này để nhận được số thao tác nhỏ nhất cần thực hiện để đưa tất cả phần tử trong mảng $nums$ về $queries[i]$, tức là $ans[i]=s[n]-s[i]-(n-i)\times queries[i]+queries[i]\times i-s[i]$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], queries: List[int]) -> List[int]:
        nums.sort()
        s = list(accumulate(nums, initial=0))
        ans = []
        for x in queries:
            i = bisect_left(nums, x + 1)
            t = s[-1] - s[i] - (len(nums) - i) * x
            i = bisect_left(nums, x)
            t += x * i - s[i]
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public List<Long> minOperations(int[] nums, int[] queries) {
        Arrays.sort(nums);
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        List<Long> ans = new ArrayList<>();
        for (int x : queries) {
            int i = search(nums, x + 1);
            long t = s[n] - s[i] - 1L * (n - i) * x;
            i = search(nums, x);
            t += 1L * x * i - s[i];
            ans.add(t);
        }
        return ans;
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> minOperations(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<long long> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        vector<long long> ans;
        for (auto& x : queries) {
            int i = lower_bound(nums.begin(), nums.end(), x + 1) - nums.begin();
            long long t = s[n] - s[i] - 1LL * (n - i) * x;
            i = lower_bound(nums.begin(), nums.end(), x) - nums.begin();
            t += 1LL * x * i - s[i];
            ans.push_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, queries []int) (ans []int64) {
	sort.Ints(nums)
	n := len(nums)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for _, x := range queries {
		i := sort.SearchInts(nums, x+1)
		t := s[n] - s[i] - (n-i)*x
		i = sort.SearchInts(nums, x)
		t += x*i - s[i]
		ans = append(ans, int64(t))
	}
	return
}
```

#### TypeScript

```ts
function minOperations(nums: number[], queries: number[]): number[] {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const s: number[] = new Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    const search = (x: number): number => {
        let l = 0;
        let r = n;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const ans: number[] = [];
    for (const x of queries) {
        const i = search(x + 1);
        let t = s[n] - s[i] - (n - i) * x;
        const j = search(x);
        t += x * j - s[j];
        ans.push(t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
