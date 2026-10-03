---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1885. Count Pairs in Two Arrays 🔒](https://leetcode.com/problems/count-pairs-in-two-arrays)

[中文文档](/solution/1800-1899/1885.Count%20Pairs%20in%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>, hãy đếm số cặp chỉ số <code>(i, j)</code> sao cho <code>i &lt; j</code> và <code>nums1[i] + nums1[j] &gt; nums2[i] + nums2[j]</code>.</p>

<p>Trả về <em><strong>số cặp</strong> thỏa mãn điều kiện.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,1,2,1], nums2 = [1,2,1,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>: Các cặp thỏa mãn điều kiện là:
- (0, 2), vì 2 + 2 &gt; 1 + 1.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,10,6,2], nums2 = [1,4,1,5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích</strong>: Các cặp thỏa mãn điều kiện là:
- (0, 1), vì 1 + 10 &gt; 1 + 4.
- (0, 2), vì 1 + 6 &gt; 1 + 1.
- (1, 2), vì 10 + 6 &gt; 4 + 1.
- (1, 3), vì 10 + 2 &gt; 4 + 5.
- (2, 3), vì 6 + 2 &gt; 1 + 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Cần đếm các cặp $i<j$ sao cho $nums1[i]+nums1[j]>nums2[i]+nums2[j]$. Điều này tương đương $a[i]+a[j]>0$ với $a=nums1-nums2$. Duyệt hai vòng lặp có độ phức tạp $O(n^2)$, quá chậm khi $n\le 10^5$.
>
> Sắp xếp $a$ rồi di chuyển $r$ từ bên phải: tăng $l$ cho đến khi $a[l]+a[r]>0$, khi đó mọi chỉ số trong $[l,r)$ đều tạo thành cặp hợp lệ với $r$. Mỗi con trỏ chỉ di chuyển một lần.

<!-- thinking:end -->

Ta có thể biến đổi bất đẳng thức trong bài toán thành $\textit{nums1}[i] - \textit{nums2}[i] + \textit{nums1}[j] - \textit{nums2}[j] > 0$, rút gọn thành $\textit{nums}[i] + \textit{nums}[j] > 0$, trong đó $\textit{nums}[i] = \textit{nums1}[i] - \textit{nums2}[i]$.

Với mảng $\textit{nums}$, ta cần tìm tất cả cặp $(i, j)$ thỏa mãn $\textit{nums}[i] + \textit{nums}[j] > 0$.

Ta có thể sắp xếp mảng $\textit{nums}$ rồi dùng phương pháp hai con trỏ. Khởi tạo con trỏ trái $l = 0$ và con trỏ phải $r = n - 1$. Mỗi lần, ta kiểm tra xem $\textit{nums}[l] + \textit{nums}[r]$ có nhỏ hơn hoặc bằng $0$ hay không. Nếu có, ta lặp để dịch con trỏ trái sang phải cho đến khi $\textit{nums}[l] + \textit{nums}[r] > 0$. Khi đó, tất cả cặp có con trỏ trái ở $l$, $l + 1$, $l + 2$, $\cdots$, $r - 1$ và con trỏ phải ở $r$ đều thỏa mãn điều kiện, có tổng cộng $r - l$ cặp. Ta cộng các cặp này vào đáp án. Sau đó, dịch con trỏ phải sang trái và tiếp tục quy trình trên cho đến khi $l \ge r$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, nums1: List[int], nums2: List[int]) -> int:
        nums = [a - b for a, b in zip(nums1, nums2)]
        nums.sort()
        l, r = 0, len(nums) - 1
        ans = 0
        while l < r:
            while l < r and nums[l] + nums[r] <= 0:
                l += 1
            ans += r - l
            r -= 1
        return ans
```

#### Java

```java
class Solution {
    public long countPairs(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = nums1[i] - nums2[i];
        }
        Arrays.sort(nums);
        int l = 0, r = n - 1;
        long ans = 0;
        while (l < r) {
            while (l < r && nums[l] + nums[r] <= 0) {
                ++l;
            }
            ans += r - l;
            --r;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countPairs(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> nums(n);
        for (int i = 0; i < n; ++i) {
            nums[i] = nums1[i] - nums2[i];
        }
        ranges::sort(nums);
        int l = 0, r = n - 1;
        long long ans = 0;
        while (l < r) {
            while (l < r && nums[l] + nums[r] <= 0) {
                ++l;
            }
            ans += r - l;
            --r;
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(nums1 []int, nums2 []int) (ans int64) {
	n := len(nums1)
	nums := make([]int, n)
	for i, x := range nums1 {
		nums[i] = x - nums2[i]
	}
	sort.Ints(nums)
	l, r := 0, n-1
	for l < r {
		for l < r && nums[l]+nums[r] <= 0 {
			l++
		}
		ans += int64(r - l)
		r--
	}
	return
}
```

#### TypeScript

```ts
function countPairs(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const nums: number[] = [];
    for (let i = 0; i < n; ++i) {
        nums.push(nums1[i] - nums2[i]);
    }
    nums.sort((a, b) => a - b);
    let ans = 0;
    let [l, r] = [0, n - 1];
    while (l < r) {
        while (l < r && nums[l] + nums[r] <= 0) {
            ++l;
        }
        ans += r - l;
        --r;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_pairs(nums1: Vec<i32>, nums2: Vec<i32>) -> i64 {
        let mut nums: Vec<i32> = nums1.iter().zip(nums2.iter()).map(|(a, b)| a - b).collect();
        nums.sort();
        let mut l = 0;
        let mut r = nums.len() - 1;
        let mut ans = 0;
        while l < r {
            while l < r && nums[l] + nums[r] <= 0 {
                l += 1;
            }
            ans += (r - l) as i64;
            r -= 1;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var countPairs = function (nums1, nums2) {
    const n = nums1.length;
    const nums = [];
    for (let i = 0; i < n; ++i) {
        nums.push(nums1[i] - nums2[i]);
    }
    nums.sort((a, b) => a - b);
    let ans = 0;
    let [l, r] = [0, n - 1];
    while (l < r) {
        while (l < r && nums[l] + nums[r] <= 0) {
            ++l;
        }
        ans += r - l;
        --r;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
