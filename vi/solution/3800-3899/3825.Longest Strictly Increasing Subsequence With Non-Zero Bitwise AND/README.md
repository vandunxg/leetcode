---
comments: true
difficulty: Medium
rating: 1845
source: Biweekly Contest 175 Q3
---

<!-- problem:start -->

# [3825. Longest Strictly Increasing Subsequence With Non-Zero Bitwise AND](https://leetcode.com/problems/longest-strictly-increasing-subsequence-with-non-zero-bitwise-and)

[中文文档](/solution/3800-3899/3825.Longest%20Strictly%20Increasing%20Subsequence%20With%20Non-Zero%20Bitwise%20AND/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về độ dài của <strong><span data-keyword="subsequence-array-nonempty">dãy con</span> tăng nghiêm ngặt dài nhất</strong> trong <code>nums</code> sao cho phép <strong>AND</strong> bitwise của dãy con khác <strong>0</strong>. Nếu không tồn tại <strong>dãy con</strong> như vậy, hãy trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một dãy con tăng nghiêm ngặt dài nhất là <code>[5, 7]</code>. Phép AND bitwise là <code>5 AND 7 = 5</code>, khác 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con tăng nghiêm ngặt dài nhất là <code>[2, 3, 6]</code>. Phép AND bitwise là <code>2 AND 3 AND 6 = 2</code>, khác 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một dãy con tăng nghiêm ngặt dài nhất là <code>[1]</code>. Phép AND bitwise là 1, khác 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Dãy con tăng dài nhất

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm dãy con tăng nghiêm ngặt dài nhất có phép AND khác 0. Vì $n \le 10^5$, LIS thông thường không xét điều kiện AND.
>
> AND khác 0 nghĩa là tồn tại một bit bằng $1$ trong mọi giá trị được chọn.
>
> Ta liệt kê bit đó, chỉ giữ lại các số có bit tương ứng bằng 1, rồi chạy LIS trên dãy đã lọc.
>
> Có khoảng $30$ bit, mỗi bit cần một lần chạy LIS với độ phức tạp $O(n \log n)$, sau đó lấy giá trị lớn nhất.

<!-- thinking:end -->

Kết quả AND bitwise khác 0 có nghĩa là tất cả các số trong dãy con đều có bit bằng $1$ tại một vị trí nào đó. Ta có thể liệt kê vị trí bit đó, tìm dãy con tăng nghiêm ngặt dài nhất trong các số có bit tương ứng bằng $1$, rồi lấy giá trị lớn nhất qua tất cả các lần liệt kê làm đáp án.

Độ phức tạp thời gian là $O(\log M \times n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $M$ lần lượt là độ dài của mảng và giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubsequence(self, nums: List[int]) -> int:
        def lis(arr: List[int]) -> int:
            g = []
            for x in arr:
                j = bisect_left(g, x)
                if j == len(g):
                    g.append(x)
                else:
                    g[j] = x
            return len(g)

        ans = 0
        m = max(nums).bit_length()
        for i in range(m):
            arr = [x for x in nums if x >> i & 1]
            ans = max(ans, lis(arr))
        return ans
```

#### Java

```java
class Solution {
    public int longestSubsequence(int[] nums) {
        int ans = 0;
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int m = 32 - Integer.numberOfLeadingZeros(mx);
        for (int i = 0; i < m; i++) {
            List<Integer> arr = new ArrayList<>();
            for (int x : nums) {
                if (((x >> i) & 1) == 1) {
                    arr.add(x);
                }
            }
            ans = Math.max(ans, lis(arr));
        }
        return ans;
    }

    private int lis(List<Integer> arr) {
        List<Integer> g = new ArrayList<>();
        for (int x : arr) {
            int l = 0, r = g.size();
            while (l < r) {
                int mid = (l + r) >>> 1;
                if (g.get(mid) >= x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (l == g.size()) {
                g.add(x);
            } else {
                g.set(l, x);
            }
        }
        return g.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubsequence(vector<int>& nums) {
        auto lis = [&](const vector<int>& arr) {
            vector<int> g;
            for (int x : arr) {
                auto it = lower_bound(g.begin(), g.end(), x);
                if (it == g.end()) {
                    g.push_back(x);
                } else {
                    *it = x;
                }
            }
            return (int) g.size();
        };

        int ans = 0;
        int mx = ranges::max(nums);
        int m = mx == 0 ? 0 : 32 - __builtin_clz(mx);

        for (int i = 0; i < m; ++i) {
            vector<int> arr;
            ranges::copy_if(nums, back_inserter(arr), [&](int x) {
                return (x >> i) & 1;
            });
            ans = max(ans, lis(arr));
        }

        return ans;
    }
};
```

#### Go

```go
func longestSubsequence(nums []int) int {
	ans := 0
	m := bits.Len(uint(slices.Max(nums)))
	for i := 0; i < m; i++ {
		arr := make([]int, 0)
		for _, x := range nums {
			if (x>>i)&1 == 1 {
				arr = append(arr, x)
			}
		}
		ans = max(ans, lis(arr))
	}
	return ans
}

func lis(arr []int) int {
	g := make([]int, 0)
	for _, x := range arr {
		j := sort.SearchInts(g, x)
		if j == len(g) {
			g = append(g, x)
		} else {
			g[j] = x
		}
	}
	return len(g)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
