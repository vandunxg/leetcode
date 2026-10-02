---
comments: true
difficulty: Medium
rating: 2078
source: Weekly Contest 222 Q3
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [1712. Ways to Split Array Into Three Subarrays](https://leetcode.com/problems/ways-to-split-array-into-three-subarrays)

[中文文档](/solution/1700-1799/1712.Ways%20to%20Split%20Array%20Into%20Three%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Một cách chia mảng số nguyên là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Mảng được chia thành ba mảng con liên tiếp <strong>không rỗng</strong>, lần lượt từ trái sang phải có tên <code>left</code>, <code>mid</code>, <code>right</code>.</li>
	<li>Tổng các phần tử trong <code>left</code> không lớn hơn tổng các phần tử trong <code>mid</code>, và tổng trong <code>mid</code> không lớn hơn tổng trong <code>right</code>.</li>
</ul>

<p>Cho <code>nums</code> là mảng các số nguyên <strong>không âm</strong>, hãy trả về <em>số cách chia <strong>hợp lệ</strong></em> <code>nums</code>. Vì kết quả có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Cách chia hợp lệ duy nhất của nums là [1] [1] [1].</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,2,2,5,0]
<strong>Output:</strong> 3
<strong>Explanation:</strong> Có ba cách chia hợp lệ nums:
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,2,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> Không có cách chia hợp lệ nums.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần ba phần không rỗng sao cho $s_{\textit{left}}\le s_{\textit{mid}}\le s_{\textit{right}}$. Thử hai điểm cắt lồng nhau theo $i$ tốn $O(n^2)$ và không đáp ứng được khi $n\le 10^5$.
>
> Các giá trị không âm nên tổng tiền tố có tính đơn điệu. Sau khi cố định điểm cắt trái $i$, điểm cắt giữa nằm trong một đoạn liên tiếp và có thể tìm bằng binary search.
>
> Đoạn này thỏa $s[j]\ge 2s[i]$ và $s[k]\le (s[-1]+s[i])/2$. Với mỗi $i$, hai lần binary search sẽ đếm số cách, rồi lấy modulo $10^9+7$.

<!-- thinking:end -->

Trước hết, ta tiền xử lý mảng tổng tiền tố $s$ của mảng $nums$, trong đó $s[i]$ là tổng của $i+1$ phần tử đầu tiên trong $nums$.

Vì mọi phần tử của mảng $nums$ đều là số nguyên không âm, mảng tổng tiền tố $s$ là mảng tăng đơn điệu.

Ta duyệt chỉ số mà mảng con `left` có thể kết thúc trong phạm vi $[0,..n-2)$, rồi dựa vào tính tăng đơn điệu của mảng tổng tiền tố để dùng binary search tìm phạm vi hợp lệ của điểm chia `mid`, ký hiệu là $[j, k)$, và cộng số cách $k-j$.

Chi tiết trong binary search, điểm chia phải thỏa $s[j] \geq s[i]$ và $s[n - 1] - s[k] \geq s[k] - s[i]$. Tức là $s[j] \geq s[i]$ và $s[k] \leq \frac{s[n - 1] + s[i]}{2}$.

Cuối cùng, trả về số cách modulo $10^9+7$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToSplit(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        s = list(accumulate(nums))
        ans, n = 0, len(nums)
        for i in range(n - 2):
            j = bisect_left(s, s[i] << 1, i + 1, n - 1)
            k = bisect_right(s, (s[-1] + s[i]) >> 1, j, n - 1)
            ans += k - j
        return ans % mod
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int waysToSplit(int[] nums) {
        int n = nums.length;
        int[] s = new int[n];
        s[0] = nums[0];
        for (int i = 1; i < n; ++i) {
            s[i] = s[i - 1] + nums[i];
        }
        int ans = 0;
        for (int i = 0; i < n - 2; ++i) {
            int j = search(s, s[i] << 1, i + 1, n - 1);
            int k = search(s, ((s[n - 1] + s[i]) >> 1) + 1, j, n - 1);
            ans = (ans + k - j) % MOD;
        }
        return ans;
    }

    private int search(int[] s, int x, int left, int right) {
        while (left < right) {
            int mid = (left + right) >> 1;
            if (s[mid] >= x) {
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
    const int mod = 1e9 + 7;

    int waysToSplit(vector<int>& nums) {
        int n = nums.size();
        vector<int> s(n, nums[0]);
        for (int i = 1; i < n; ++i) s[i] = s[i - 1] + nums[i];
        int ans = 0;
        for (int i = 0; i < n - 2; ++i) {
            int j = lower_bound(s.begin() + i + 1, s.begin() + n - 1, s[i] << 1) - s.begin();
            int k = upper_bound(s.begin() + j, s.begin() + n - 1, (s[n - 1] + s[i]) >> 1) - s.begin();
            ans = (ans + k - j) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func waysToSplit(nums []int) (ans int) {
	const mod int = 1e9 + 7
	n := len(nums)
	s := make([]int, n)
	s[0] = nums[0]
	for i := 1; i < n; i++ {
		s[i] = s[i-1] + nums[i]
	}
	for i := 0; i < n-2; i++ {
		j := sort.Search(n-1, func(h int) bool { return h > i && s[h] >= (s[i]<<1) })
		k := sort.Search(n-1, func(h int) bool { return h >= j && s[h] > (s[n-1]+s[i])>>1 })
		ans = (ans + k - j) % mod
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var waysToSplit = function (nums) {
    const mod = 1e9 + 7;
    const n = nums.length;
    const s = new Array(n).fill(nums[0]);
    for (let i = 1; i < n; ++i) {
        s[i] = s[i - 1] + nums[i];
    }
    function search(s, x, left, right) {
        while (left < right) {
            const mid = (left + right) >> 1;
            if (s[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
    let ans = 0;
    for (let i = 0; i < n - 2; ++i) {
        const j = search(s, s[i] << 1, i + 1, n - 1);
        const k = search(s, ((s[n - 1] + s[i]) >> 1) + 1, j, n - 1);
        ans = (ans + k - j) % mod;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
