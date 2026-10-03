---
comments: true
difficulty: Medium
rating: 1588
source: Biweekly Contest 46 Q2
tags:
    - Greedy
    - Array
    - Two Pointers
    - String Matching
    - KMP
---

<!-- problem:start -->

# [1764. Form Array by Concatenating Subarrays of Another Array](https://leetcode.com/problems/form-array-by-concatenating-subarrays-of-another-array)

[中文文档](/solution/1700-1799/1764.Form%20Array%20by%20Concatenating%20Subarrays%20of%20Another%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên hai chiều <code>groups</code> có độ dài <code>n</code>. Bạn cũng được cho mảng số nguyên <code>nums</code>.</p>

<p>Hãy xác định liệu có thể chọn <code>n</code> mảng con <strong>không giao nhau</strong> từ mảng <code>nums</code> sao cho mảng con thứ <code>i<sup>th</sup></code> bằng <code>groups[i]</code> (<b>đánh số từ 0</b>) hay không. Nếu <code>i &gt; 0</code>, mảng con thứ <code>(i-1)<sup>th</sup></code> phải xuất hiện <strong>trước</strong> mảng con thứ <code>i<sup>th</sup></code> trong <code>nums</code> (tức là các mảng con phải cùng thứ tự với <code>groups</code>).</p>

<p>Trả về <code>true</code> <em>nếu có thể thực hiện, và</em> <code>false</code> <em>nếu không</em>.</p>

<p>Lưu ý rằng các mảng con <strong>không giao nhau</strong> khi và chỉ khi không có chỉ số <code>k</code> nào mà <code>nums[k]</code> thuộc nhiều hơn một mảng con. Mảng con là một dãy phần tử liên tiếp trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> groups = [[1,-1,-1],[3,-2,0]], nums = [1,-1,0,1,-1,-1,3,-2,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chọn mảng con thứ 0<sup>th</sup> là [1,-1,0,<u><strong>1,-1,-1</strong></u>,3,-2,0] và mảng con thứ 1<sup>st</sup> là [1,-1,0,1,-1,-1,<u><strong>3,-2,0</strong></u>].
Các mảng con này không giao nhau vì không dùng chung phần tử nums[k].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> groups = [[10,-2],[1,2,3,4]], nums = [1,2,3,4,10,-2]
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Chọn các mảng con [<u><strong>1,2,3,4</strong></u>,10,-2] và [1,2,3,4,<u><strong>10,-2</strong></u>] là sai vì chúng không cùng thứ tự như trong groups.
[10,-2] phải xuất hiện trước [1,2,3,4].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> groups = [[1,2,3],[3,4]], nums = [7,7,1,2,3,4,7,7]
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Chọn các mảng con [7,7,<u><strong>1,2,3</strong></u>,4,7,7] và [7,7,1,2,<u><strong>3,4</strong></u>,7,7] là không hợp lệ vì chúng giao nhau.
Chúng dùng chung phần tử nums[4] (đánh số từ 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>groups.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= groups[i].length, sum(groups[i].length) &lt;= 10<sup><span style="font-size: 10.8333px;">3</span></sup></code></li>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>3</sup></code></li>
	<li><code>-10<sup>7</sup> &lt;= groups[i][j], nums[k] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải khớp các groups theo thứ tự thành những đoạn liên tiếp không giao nhau của $nums$. Kích thước nhỏ, nên có thể tham lam duyệt từ trái sang phải: dùng một group khi khớp, nếu không thì dịch điểm bắt đầu sang một vị trí.
>
> Con trỏ $i$ là group hiện tại còn $j$ duyệt qua $nums$. Khi một đoạn khớp, tăng $j$ theo độ dài group và tăng $i$; nếu không, tăng $j$. Thành công khi $i$ đạt số lượng group.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canChoose(self, groups: List[List[int]], nums: List[int]) -> bool:
        n, m = len(groups), len(nums)
        i = j = 0
        while i < n and j < m:
            g = groups[i]
            if g == nums[j : j + len(g)]:
                j += len(g)
                i += 1
            else:
                j += 1
        return i == n
```

#### Java

```java
class Solution {
    public boolean canChoose(int[][] groups, int[] nums) {
        int n = groups.length, m = nums.length;
        int i = 0;
        for (int j = 0; i < n && j < m;) {
            if (check(groups[i], nums, j)) {
                j += groups[i].length;
                ++i;
            } else {
                ++j;
            }
        }
        return i == n;
    }

    private boolean check(int[] a, int[] b, int j) {
        int m = a.length, n = b.length;
        int i = 0;
        for (; i < m && j < n; ++i, ++j) {
            if (a[i] != b[j]) {
                return false;
            }
        }
        return i == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canChoose(vector<vector<int>>& groups, vector<int>& nums) {
        auto check = [&](vector<int>& a, vector<int>& b, int j) {
            int m = a.size(), n = b.size();
            int i = 0;
            for (; i < m && j < n; ++i, ++j) {
                if (a[i] != b[j]) {
                    return false;
                }
            }
            return i == m;
        };
        int n = groups.size(), m = nums.size();
        int i = 0;
        for (int j = 0; i < n && j < m;) {
            if (check(groups[i], nums, j)) {
                j += groups[i].size();
                ++i;
            } else {
                ++j;
            }
        }
        return i == n;
    }
};
```

#### Go

```go
func canChoose(groups [][]int, nums []int) bool {
	check := func(a, b []int, j int) bool {
		m, n := len(a), len(b)
		i := 0
		for ; i < m && j < n; i, j = i+1, j+1 {
			if a[i] != b[j] {
				return false
			}
		}
		return i == m
	}
	n, m := len(groups), len(nums)
	i := 0
	for j := 0; i < n && j < m; {
		if check(groups[i], nums, j) {
			j += len(groups[i])
			i++
		} else {
			j++
		}
	}
	return i == n
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
