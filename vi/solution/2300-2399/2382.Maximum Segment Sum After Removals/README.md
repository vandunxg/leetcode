---
comments: true
difficulty: Hard
rating: 2136
source: Biweekly Contest 85 Q4
tags:
    - Union Find
    - Array
    - Ordered Set
    - Prefix Sum
---

<!-- problem:start -->

# [2382. Maximum Segment Sum After Removals](https://leetcode.com/problems/maximum-segment-sum-after-removals)

[中文文档](/solution/2300-2399/2382.Maximum%20Segment%20Sum%20After%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>0-indexed</strong> <code>nums</code> và <code>removeQueries</code>, cả hai đều có độ dài <code>n</code>. Với truy vấn thứ <code>i<sup>th</sup></code>, phần tử trong <code>nums</code> tại chỉ số <code>removeQueries[i]</code> sẽ bị xóa, chia <code>nums</code> thành các đoạn khác nhau.</p>

<p>Một <strong>đoạn</strong> là một dãy liên tiếp gồm các số nguyên <strong>dương</strong> trong <code>nums</code>. <strong>Tổng đoạn</strong> là tổng của mọi phần tử trong một đoạn.</p>

<p>Trả về<em> một mảng số nguyên </em><code>answer</code><em>, có độ dài </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là tổng đoạn <strong>lớn nhất</strong> sau khi thực hiện lần xóa thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p><strong>Lưu ý:</strong> Cùng một chỉ số sẽ <strong>không</strong> bị xóa quá một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5,6,1], removeQueries = [0,3,2,4,1]
<strong>Đầu ra:</strong> [14,7,2,2,0]
<strong>Giải thích:</strong> Dùng 0 để biểu thị phần tử đã bị xóa, kết quả như sau:
Truy vấn 1: Xóa phần tử ở chỉ số 0, nums trở thành [0,2,5,6,1] và tổng đoạn lớn nhất là 14 với đoạn [2,5,6,1].
Truy vấn 2: Xóa phần tử ở chỉ số 3, nums trở thành [0,2,5,0,1] và tổng đoạn lớn nhất là 7 với đoạn [2,5].
Truy vấn 3: Xóa phần tử ở chỉ số 2, nums trở thành [0,2,0,0,1] và tổng đoạn lớn nhất là 2 với đoạn [2].
Truy vấn 4: Xóa phần tử ở chỉ số 4, nums trở thành [0,2,0,0,0] và tổng đoạn lớn nhất là 2 với đoạn [2].
Truy vấn 5: Xóa phần tử ở chỉ số 1, nums trở thành [0,0,0,0,0] và tổng đoạn lớn nhất là 0 vì không còn đoạn nào.
Cuối cùng, ta trả về [14,7,2,2,0].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,11,1], removeQueries = [3,2,1,0]
<strong>Đầu ra:</strong> [16,5,3,0]
<strong>Giải thích:</strong> Dùng 0 để biểu thị phần tử đã bị xóa, kết quả như sau:
Truy vấn 1: Xóa phần tử ở chỉ số 3, nums trở thành [3,2,11,0] và tổng đoạn lớn nhất là 16 với đoạn [3,2,11].
Truy vấn 2: Xóa phần tử ở chỉ số 2, nums trở thành [3,2,0,0] và tổng đoạn lớn nhất là 5 với đoạn [3,2].
Truy vấn 3: Xóa phần tử ở chỉ số 1, nums trở thành [3,0,0,0] và tổng đoạn lớn nhất là 3 với đoạn [3].
Truy vấn 4: Xóa phần tử ở chỉ số 0, nums trở thành [0,0,0,0] và tổng đoạn lớn nhất là 0 vì không còn đoạn nào.
Cuối cùng, ta trả về [16,5,3,0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length == removeQueries.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= removeQueries[i] &lt; n</code></li>
	<li>Tất cả các giá trị của <code>removeQueries</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xóa các chỉ số theo thứ tự cho trước và trả về tổng đoạn lớn nhất còn lại. Vì $n \le 10^5$ và việc xóa xuôi sẽ liên tục chia nhỏ các đoạn, ta có thể chèn lại theo chiều ngược để thực hiện thao tác gộp.
>
> Chèn lại các chỉ số đã xóa từ cuối về đầu, hợp nhất với các hàng xóm đã tồn tại và duy trì tổng của các đoạn. Giá trị lớn nhất sau mỗi lần chèn chính là đáp án ngay trước lần xóa tương ứng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSegmentSum(self, nums: List[int], removeQueries: List[int]) -> List[int]:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        def merge(a, b):
            pa, pb = find(a), find(b)
            p[pa] = pb
            s[pb] += s[pa]

        n = len(nums)
        p = list(range(n))
        s = [0] * n
        ans = [0] * n
        mx = 0
        for j in range(n - 1, 0, -1):
            i = removeQueries[j]
            s[i] = nums[i]
            if i and s[find(i - 1)]:
                merge(i, i - 1)
            if i < n - 1 and s[find(i + 1)]:
                merge(i, i + 1)
            mx = max(mx, s[find(i)])
            ans[j - 1] = mx
        return ans
```

#### Java

```java
class Solution {
    private int[] p;
    private long[] s;

    public long[] maximumSegmentSum(int[] nums, int[] removeQueries) {
        int n = nums.length;
        p = new int[n];
        s = new long[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        long[] ans = new long[n];
        long mx = 0;
        for (int j = n - 1; j > 0; --j) {
            int i = removeQueries[j];
            s[i] = nums[i];
            if (i > 0 && s[find(i - 1)] > 0) {
                merge(i, i - 1);
            }
            if (i < n - 1 && s[find(i + 1)] > 0) {
                merge(i, i + 1);
            }
            mx = Math.max(mx, s[find(i)]);
            ans[j - 1] = mx;
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    private void merge(int a, int b) {
        int pa = find(a), pb = find(b);
        p[pa] = pb;
        s[pb] += s[pa];
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    vector<int> p;
    vector<ll> s;

    vector<long long> maximumSegmentSum(vector<int>& nums, vector<int>& removeQueries) {
        int n = nums.size();
        p.resize(n);
        for (int i = 0; i < n; ++i) p[i] = i;
        s.assign(n, 0);
        vector<ll> ans(n);
        ll mx = 0;
        for (int j = n - 1; j; --j) {
            int i = removeQueries[j];
            s[i] = nums[i];
            if (i && s[find(i - 1)]) merge(i, i - 1);
            if (i < n - 1 && s[find(i + 1)]) merge(i, i + 1);
            mx = max(mx, s[find(i)]);
            ans[j - 1] = mx;
        }
        return ans;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    void merge(int a, int b) {
        int pa = find(a), pb = find(b);
        p[pa] = pb;
        s[pb] += s[pa];
    }
};
```

#### Go

```go
func maximumSegmentSum(nums []int, removeQueries []int) []int64 {
	n := len(nums)
	p := make([]int, n)
	s := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(x int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	merge := func(a, b int) {
		pa, pb := find(a), find(b)
		p[pa] = pb
		s[pb] += s[pa]
	}
	mx := 0
	ans := make([]int64, n)
	for j := n - 1; j > 0; j-- {
		i := removeQueries[j]
		s[i] = nums[i]
		if i > 0 && s[find(i-1)] > 0 {
			merge(i, i-1)
		}
		if i < n-1 && s[find(i+1)] > 0 {
			merge(i, i+1)
		}
		mx = max(mx, s[find(i)])
		ans[j-1] = int64(mx)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
