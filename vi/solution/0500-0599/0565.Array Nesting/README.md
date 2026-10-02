---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Array
---

<!-- problem:start -->

# [565. Array Nesting](https://leetcode.com/problems/array-nesting)

[中文文档](/solution/0500-0599/0565.Array%20Nesting/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một hoán vị của các số trong khoảng <code>[0, n - 1]</code>.</p>

<p>Hãy tạo tập hợp <code>s[k] = {nums[k], nums[nums[k]], nums[nums[nums[k]]], ... }</code> theo các quy tắc sau:</p>

<ul>
	<li>Phần tử đầu tiên trong <code>s[k]</code> là <code>nums[k]</code>, tương ứng với <code>index = k</code>.</li>
	<li>Phần tử tiếp theo trong <code>s[k]</code> là <code>nums[nums[k]]</code>, sau đó là <code>nums[nums[nums[k]]]</code>, và cứ tiếp tục như vậy.</li>
	<li>Dừng ngay trước khi một phần tử bị lặp lại trong <code>s[k]</code>.</li>
</ul>

<p>Hãy trả về <em>độ dài lớn nhất của tập hợp</em> <code>s[k]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,0,3,1,6,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 
nums[0] = 5, nums[1] = 4, nums[2] = 0, nums[3] = 3, nums[4] = 1, nums[5] = 6, nums[6] = 2.
Một trong những tập hợp s[k] dài nhất:
s[0] = {nums[0], nums[5], nums[6], nums[2]} = {5, 6, 2, 0}
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
	<li>Tất cả giá trị trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $A[i]$ dẫn đến $A[A[i]]$, nhờ đó mảng được chia thành các chu trình rời nhau. Mỗi chu trình tương ứng với một dãy lồng nhau. Bắt đầu duyệt từ mọi chỉ số sẽ đi lại qua cùng một chu trình.
>
> Mảng visited đánh dấu các chỉ số đã gặp; chỉ bắt đầu duyệt từ chỉ số chưa được đánh dấu và đếm độ dài chu trình. Vì các chu trình rời nhau nên mỗi chỉ số chỉ được thăm một lần. Chu trình dài nhất là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayNesting(self, nums: List[int]) -> int:
        n = len(nums)
        vis = [False] * n
        res = 0
        for i in range(n):
            if vis[i]:
                continue
            cur, m = nums[i], 1
            vis[cur] = True
            while nums[cur] != nums[i]:
                cur = nums[cur]
                m += 1
                vis[cur] = True
            res = max(res, m)
        return res
```

#### Java

```java
class Solution {
    public int arrayNesting(int[] nums) {
        int n = nums.length;
        boolean[] vis = new boolean[n];
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (vis[i]) {
                continue;
            }
            int cur = nums[i], m = 1;
            vis[cur] = true;
            while (nums[cur] != nums[i]) {
                cur = nums[cur];
                m++;
                vis[cur] = true;
            }
            res = Math.max(res, m);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        int n = nums.size();
        vector<bool> vis(n);
        int res = 0;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) continue;
            int cur = nums[i], m = 1;
            vis[cur] = true;
            while (nums[cur] != nums[i]) {
                cur = nums[cur];
                ++m;
                vis[cur] = true;
            }
            res = max(res, m);
        }
        return res;
    }
};
```

#### Go

```go
func arrayNesting(nums []int) int {
	n := len(nums)
	vis := make([]bool, n)
	ans := 0
	for i := 0; i < n; i++ {
		if vis[i] {
			continue
		}
		cur, m := nums[i], 1
		vis[cur] = true
		for nums[cur] != nums[i] {
			cur = nums[cur]
			m++
			vis[cur] = true
		}
		if m > ans {
			ans = m
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng mảng visited có kích thước $O(n)$. Các giá trị vốn nằm trong $[0,n-1]$, nên có thể dùng sentinel $n$ để đánh dấu trực tiếp ô đã thăm.
>
> Duyệt chu trình, ghi $n$ vào từng ô và đếm số ô. Giá trị $n$ cho biết chỉ số đó đã được xử lý. Bộ nhớ phụ giảm còn hằng số; các chu trình vẫn không thay đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayNesting(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(n):
            cnt = 0
            while nums[i] != n:
                j = nums[i]
                nums[i] = n
                i = j
                cnt += 1
            ans = max(ans, cnt)
        return ans
```

#### Java

```java
class Solution {
    public int arrayNesting(int[] nums) {
        int ans = 0, n = nums.length;
        for (int i = 0; i < n; ++i) {
            int cnt = 0;
            int j = i;
            while (nums[j] < n) {
                int k = nums[j];
                nums[j] = n;
                j = k;
                ++cnt;
            }
            ans = Math.max(ans, cnt);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        int ans = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            int cnt = 0;
            int j = i;
            while (nums[j] < n) {
                int k = nums[j];
                nums[j] = n;
                j = k;
                ++cnt;
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func arrayNesting(nums []int) int {
	ans, n := 0, len(nums)
	for i := range nums {
		cnt, j := 0, i
		for nums[j] != n {
			k := nums[j]
			nums[j] = n
			j = k
			cnt++
		}
		if ans < cnt {
			ans = cnt
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
