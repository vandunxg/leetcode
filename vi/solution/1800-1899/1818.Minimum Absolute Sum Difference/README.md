---
comments: true
difficulty: Medium
rating: 1934
source: Weekly Contest 235 Q3
tags:
    - Array
    - Binary Search
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [1818. Minimum Absolute Sum Difference](https://leetcode.com/problems/minimum-absolute-sum-difference)

[中文文档](/solution/1800-1899/1818.Minimum%20Absolute%20Sum%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên dương <code>nums1</code> và <code>nums2</code>, cả hai có cùng độ dài <code>n</code>.</p>

<p><strong>Tổng hiệu tuyệt đối</strong> của hai mảng <code>nums1</code> và <code>nums2</code> được định nghĩa là <strong>tổng</strong> của <code>|nums1[i] - nums2[i]|</code> với mỗi <code>0 &lt;= i &lt; n</code> (đánh chỉ số từ <strong>0</strong>).</p>

<p>Bạn có thể thay thế <strong>tối đa một</strong> phần tử của <code>nums1</code> bằng <strong>bất kỳ</strong> phần tử nào khác trong <code>nums1</code> để <strong>tối thiểu hóa</strong> tổng hiệu tuyệt đối.</p>

<p>Trả về <em>tổng hiệu tuyệt đối nhỏ nhất <strong>sau khi</strong> thay thế tối đa một<strong> </strong>phần tử trong mảng <code>nums1</code>.</em> Vì đáp án có thể lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><code>|x|</code> được định nghĩa như sau:</p>

<ul>
	<li><code>x</code> nếu <code>x &gt;= 0</code>, hoặc</li>
	<li><code>-x</code> nếu <code>x &lt; 0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,7,5], nums2 = [2,3,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Có hai cách tối ưu:
- Thay phần tử thứ hai bằng phần tử thứ nhất: [1,<u><strong>7</strong></u>,5] =&gt; [1,<u><strong>1</strong></u>,5], hoặc
- Thay phần tử thứ hai bằng phần tử thứ ba: [1,<u><strong>7</strong></u>,5] =&gt; [1,<u><strong>5</strong></u>,5].
Cả hai đều cho tổng hiệu tuyệt đối <code>|1-2| + (|1-3| or |5-3|) + |5-5| = </code>3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>nums1 bằng nums2 nên không cần thay thế. Khi đó tổng hiệu tuyệt đối bằng
0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
<strong>Đầu ra:</strong> 20
<strong>Giải thích: </strong>Thay phần tử thứ nhất bằng phần tử thứ hai: [<u><strong>1</strong></u>,10,4,4,2,7] =&gt; [<u><strong>10</strong></u>,10,4,4,2,7].
Khi đó tổng hiệu tuyệt đối là <code>|10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20</code>
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length</code></li>
	<li><code>n == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta được phép thay tối đa một phần tử của $nums1$ để tối thiểu hóa $\sum|nums1[i]-nums2[i]|$. Thử mọi giá trị thay thế ở mọi chỉ số tốn $O(n^2)$. Với $n\le 10^5$, cách này không thể đáp ứng.
>
> Tính tổng chưa thay thế $s$. Lợi ích khi thay tại chỉ số $i$ là phần giảm từ $|nums1[i]-nums2[i]|$ xuống khoảng cách giữa $nums2[i]$ và giá trị gần nhất đã xuất hiện trong $nums1$. Sắp xếp một bản sao của $nums1$, tìm kiếm nhị phân hai phần tử lân cận của mỗi $nums2[i]$, giữ lợi ích lớn nhất $mx$ và trả về $s-mx$.

<!-- thinking:end -->

Theo đề bài, trước hết ta tính tổng hiệu tuyệt đối của `nums1` và `nums2` khi chưa thay thế, gọi là $s$.

Tiếp theo, ta xét từng phần tử $nums1[i]$ trong `nums1`, thay nó bằng phần tử gần $nums2[i]$ nhất và cũng tồn tại trong `nums1`. Vì vậy, trước khi xét, ta tạo một bản sao của `nums1`, được mảng `nums`, rồi sắp xếp `nums`. Sau đó, ta tìm kiếm nhị phân trong `nums` phần tử gần $nums2[i]$ nhất, gọi là $nums[j]$, rồi tính $|nums1[i] - nums2[i]| - |nums[j] - nums2[i]|$ và cập nhật giá trị lớn nhất của hiệu $mx$.

Cuối cùng, ta lấy $mx$ trừ khỏi $s$ để được đáp án. Lưu ý thực hiện phép modulo.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng `nums1`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAbsoluteSumDiff(self, nums1: List[int], nums2: List[int]) -> int:
        mod = 10**9 + 7
        nums = sorted(nums1)
        s = sum(abs(a - b) for a, b in zip(nums1, nums2)) % mod
        mx = 0
        for a, b in zip(nums1, nums2):
            d1, d2 = abs(a - b), inf
            i = bisect_left(nums, b)
            if i < len(nums):
                d2 = min(d2, abs(nums[i] - b))
            if i:
                d2 = min(d2, abs(nums[i - 1] - b))
            mx = max(mx, d1 - d2)
        return (s - mx + mod) % mod
```

#### Java

```java
class Solution {
    public int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
        final int mod = (int) 1e9 + 7;
        int[] nums = nums1.clone();
        Arrays.sort(nums);
        int s = 0, n = nums.length;
        for (int i = 0; i < n; ++i) {
            s = (s + Math.abs(nums1[i] - nums2[i])) % mod;
        }
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            int d1 = Math.abs(nums1[i] - nums2[i]);
            int d2 = 1 << 30;
            int j = search(nums, nums2[i]);
            if (j < n) {
                d2 = Math.min(d2, Math.abs(nums[j] - nums2[i]));
            }
            if (j > 0) {
                d2 = Math.min(d2, Math.abs(nums[j - 1] - nums2[i]));
            }
            mx = Math.max(mx, d1 - d2);
        }
        return (s - mx + mod) % mod;
    }

    private int search(int[] nums, int x) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) {
        const int mod = 1e9 + 7;
        vector<int> nums(nums1);
        sort(nums.begin(), nums.end());
        int s = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            s = (s + abs(nums1[i] - nums2[i])) % mod;
        }
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            int d1 = abs(nums1[i] - nums2[i]);
            int d2 = 1 << 30;
            int j = lower_bound(nums.begin(), nums.end(), nums2[i]) - nums.begin();
            if (j < n) {
                d2 = min(d2, abs(nums[j] - nums2[i]));
            }
            if (j) {
                d2 = min(d2, abs(nums[j - 1] - nums2[i]));
            }
            mx = max(mx, d1 - d2);
        }
        return (s - mx + mod) % mod;
    }
};
```

#### Go

```go
func minAbsoluteSumDiff(nums1 []int, nums2 []int) int {
	n := len(nums1)
	nums := make([]int, n)
	copy(nums, nums1)
	sort.Ints(nums)
	s, mx := 0, 0
	const mod int = 1e9 + 7
	for i, a := range nums1 {
		b := nums2[i]
		s = (s + abs(a-b)) % mod
	}
	for i, a := range nums1 {
		b := nums2[i]
		d1, d2 := abs(a-b), 1<<30
		j := sort.SearchInts(nums, b)
		if j < n {
			d2 = min(d2, abs(nums[j]-b))
		}
		if j > 0 {
			d2 = min(d2, abs(nums[j-1]-b))
		}
		mx = max(mx, d1-d2)
	}
	return (s - mx + mod) % mod
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minAbsoluteSumDiff(nums1: number[], nums2: number[]): number {
    const mod = 10 ** 9 + 7;
    const nums = [...nums1];
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let s = 0;
    for (let i = 0; i < n; ++i) {
        s = (s + Math.abs(nums1[i] - nums2[i])) % mod;
    }
    let mx = 0;
    for (let i = 0; i < n; ++i) {
        const d1 = Math.abs(nums1[i] - nums2[i]);
        let d2 = 1 << 30;
        let j = search(nums, nums2[i]);
        if (j < n) {
            d2 = Math.min(d2, Math.abs(nums[j] - nums2[i]));
        }
        if (j) {
            d2 = Math.min(d2, Math.abs(nums[j - 1] - nums2[i]));
        }
        mx = Math.max(mx, d1 - d2);
    }
    return (s - mx + mod) % mod;
}

function search(nums: number[], x: number): number {
    let left = 0;
    let right = nums.length;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (nums[mid] >= x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var minAbsoluteSumDiff = function (nums1, nums2) {
    const mod = 10 ** 9 + 7;
    const nums = [...nums1];
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let s = 0;
    for (let i = 0; i < n; ++i) {
        s = (s + Math.abs(nums1[i] - nums2[i])) % mod;
    }
    let mx = 0;
    for (let i = 0; i < n; ++i) {
        const d1 = Math.abs(nums1[i] - nums2[i]);
        let d2 = 1 << 30;
        let j = search(nums, nums2[i]);
        if (j < n) {
            d2 = Math.min(d2, Math.abs(nums[j] - nums2[i]));
        }
        if (j) {
            d2 = Math.min(d2, Math.abs(nums[j - 1] - nums2[i]));
        }
        mx = Math.max(mx, d1 - d2);
    }
    return (s - mx + mod) % mod;
};

function search(nums, x) {
    let left = 0;
    let right = nums.length;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (nums[mid] >= x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
