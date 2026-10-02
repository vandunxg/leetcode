---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - String
---

<!-- problem:start -->

# [484. Find Permutation 🔒](https://leetcode.com/problems/find-permutation)

[中文文档](/solution/0400-0499/0484.Find%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Một hoán vị <code>perm</code> gồm <code>n</code> số nguyên trong khoảng <code>[1, n]</code> có thể được biểu diễn bằng chuỗi <code>s</code> độ dài <code>n - 1</code>, trong đó:</p>

<ul>
	<li><code>s[i] == &#39;I&#39;</code> nếu <code>perm[i] &lt; perm[i + 1]</code>, và</li>
	<li><code>s[i] == &#39;D&#39;</code> nếu <code>perm[i] &gt; perm[i + 1]</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code>. Hãy khôi phục và trả về hoán vị <code>perm</code> nhỏ nhất theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;I&quot;
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> [1,2] là hoán vị hợp lệ duy nhất được biểu diễn bởi s, trong đó 1 và 2 tạo thành quan hệ tăng dần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;DI&quot;
<strong>Đầu ra:</strong> [2,1,3]
<strong>Giải thích:</strong> Cả [2,1,3] và [3,1,2] đều có thể được biểu diễn thành &quot;DI&quot;, nhưng vì cần tìm hoán vị nhỏ nhất theo thứ tự từ điển nên phải trả về [2,1,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> chỉ có thể là <code>&#39;I&#39;</code> hoặc <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tạo hoán vị nhỏ nhất theo thứ tự từ điển của $1..n+1$ khớp với chuỗi $I/D$. Để hoán vị nhỏ nhất, dãy nên tăng dần, trừ những đoạn bắt buộc phải giảm.
>
> Bắt đầu với dãy $1,2,\ldots,n+1$ rồi đảo ngược từng đoạn ứng với một dãy liên tiếp ký tự $D$. Các vị trí có $I$ giữ nguyên thứ tự tương đối.
>
> Đảo ngược đoạn $D$ sẽ khiến khoảng đó giảm dần mà không làm thay đổi prefix chưa xử lý, nhờ vậy thu được hoán vị nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPermutation(self, s: str) -> List[int]:
        n = len(s)
        ans = list(range(1, n + 2))
        i = 0
        while i < n:
            j = i
            while j < n and s[j] == 'D':
                j += 1
            ans[i : j + 1] = ans[i : j + 1][::-1]
            i = max(i + 1, j)
        return ans
```

#### Java

```java
class Solution {
    public int[] findPermutation(String s) {
        int n = s.length();
        int[] ans = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            ans[i] = i + 1;
        }
        int i = 0;
        while (i < n) {
            int j = i;
            while (j < n && s.charAt(j) == 'D') {
                ++j;
            }
            reverse(ans, i, j);
            i = Math.max(i + 1, j);
        }
        return ans;
    }

    private void reverse(int[] arr, int i, int j) {
        for (; i < j; ++i, --j) {
            int t = arr[i];
            arr[i] = arr[j];
            arr[j] = t;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findPermutation(string s) {
        int n = s.size();
        vector<int> ans(n + 1);
        iota(ans.begin(), ans.end(), 1);
        int i = 0;
        while (i < n) {
            int j = i;
            while (j < n && s[j] == 'D') {
                ++j;
            }
            reverse(ans.begin() + i, ans.begin() + j + 1);
            i = max(i + 1, j);
        }
        return ans;
    }
};
```

#### Go

```go
func findPermutation(s string) []int {
	n := len(s)
	ans := make([]int, n+1)
	for i := range ans {
		ans[i] = i + 1
	}
	i := 0
	for i < n {
		j := i
		for ; j < n && s[j] == 'D'; j++ {
		}
		reverse(ans, i, j)
		i = max(i+1, j)
	}
	return ans
}

func reverse(arr []int, i, j int) {
	for ; i < j; i, j = i+1, j-1 {
		arr[i], arr[j] = arr[j], arr[i]
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
