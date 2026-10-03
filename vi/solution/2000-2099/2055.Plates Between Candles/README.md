---
comments: true
difficulty: Medium
rating: 1819
source: Biweekly Contest 64 Q3
tags:
    - Array
    - String
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [2055. Plates Between Candles](https://leetcode.com/problems/plates-between-candles)

[中文文档](/solution/2000-2099/2055.Plates%20Between%20Candles/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc bàn dài với một hàng đĩa và nến được sắp xếp trên đó. Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code> chỉ gồm các ký tự <code>&#39;*&#39;</code> và <code>&#39;|&#39;</code>, trong đó <code>&#39;*&#39;</code> biểu diễn một <strong>đĩa</strong> và <code>&#39;|&#39;</code> biểu diễn một <strong>cây nến</strong>.</p>

<p>Đồng thời, cho một mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>queries</code>, trong đó <code>queries[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> biểu diễn <strong>chuỗi con</strong> <code>s[left<sub>i</sub>...right<sub>i</sub>]</code> (<strong>bao gồm cả hai đầu</strong>). Với mỗi truy vấn, hãy tìm <strong>số lượng</strong> đĩa <strong>nằm giữa các cây nến</strong> và <strong>nằm trong chuỗi con</strong>. Một đĩa được xem là <strong>nằm giữa các cây nến</strong> nếu có ít nhất một cây nến ở bên trái <strong>và</strong> ít nhất một cây nến ở bên phải <strong>trong chuỗi con</strong>.</p>

<ul>
	<li>Ví dụ, <code>s = &quot;||**||**|*&quot;</code>, và truy vấn <code>[3, 8]</code> biểu diễn chuỗi con <code>&quot;*||<strong><u>**</u></strong>|&quot;</code>. Số đĩa nằm giữa các cây nến trong chuỗi con này là <code>2</code>, vì mỗi đĩa trong hai đĩa đều có ít nhất một cây nến <strong>ở bên trái</strong> và <strong>ở bên phải trong chuỗi con</strong>.</li>
</ul>

<p>Trả về <em>một mảng số nguyên</em> <code>answer</code>, <em>trong đó</em> <code>answer[i]</code> <em>là đáp án của</em> <code>i<sup>th</sup></code> <em>truy vấn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="ex-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2055.Plates%20Between%20Candles/images/ex-1.png" style="width: 400px; height: 134px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;**|**|***|&quot;, queries = [[2,5],[5,9]]
<strong>Đầu ra:</strong> [2,3]
<strong>Giải thích:</strong>
- queries[0] có hai đĩa nằm giữa các cây nến.
- queries[1] có ba đĩa nằm giữa các cây nến.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="ex-2" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2055.Plates%20Between%20Candles/images/ex-2.png" style="width: 600px; height: 193px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;***|**|*****|**||**|*&quot;, queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
<strong>Đầu ra:</strong> [9,0,0,0,0]
<strong>Giải thích:</strong>
- queries[0] có chín đĩa nằm giữa các cây nến.
- Các truy vấn còn lại có không đĩa nào nằm giữa các cây nến.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;*&#39;</code> và <code>&#39;|&#39;</code>.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= left<sub>i</sub> &lt;= right<sub>i</sub> &lt; s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài chuỗi và số lượng truy vấn đều có thể đạt $10^5$, vì vậy không thể duyệt qua chuỗi cho từng truy vấn. Một đĩa chỉ được đếm khi có cây nến ở cả hai phía của nó.
>
> Tổng tiền tố của `*` cho phép đếm số đĩa trong mọi khoảng mở; ta tiền xử lý cây nến gần nhất bên trái và bên phải `|`. Với $[l,r]$, lấy cây nến đầu tiên $i$ ở bên phải $l$ và cây nến cuối cùng $j$ ở bên trái $r$; nếu $i<j$ thì đáp án là $presum[j]-presum[i+1]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:
        n = len(s)
        presum = [0] * (n + 1)
        for i, c in enumerate(s):
            presum[i + 1] = presum[i] + (c == '*')

        left, right = [0] * n, [0] * n
        l = r = -1
        for i, c in enumerate(s):
            if c == '|':
                l = i
            left[i] = l
        for i in range(n - 1, -1, -1):
            if s[i] == '|':
                r = i
            right[i] = r

        ans = [0] * len(queries)
        for k, (l, r) in enumerate(queries):
            i, j = right[l], left[r]
            if i >= 0 and j >= 0 and i < j:
                ans[k] = presum[j] - presum[i + 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] platesBetweenCandles(String s, int[][] queries) {
        int n = s.length();
        int[] presum = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            presum[i + 1] = presum[i] + (s.charAt(i) == '*' ? 1 : 0);
        }
        int[] left = new int[n];
        int[] right = new int[n];
        for (int i = 0, l = -1; i < n; ++i) {
            if (s.charAt(i) == '|') {
                l = i;
            }
            left[i] = l;
        }
        for (int i = n - 1, r = -1; i >= 0; --i) {
            if (s.charAt(i) == '|') {
                r = i;
            }
            right[i] = r;
        }
        int[] ans = new int[queries.length];
        for (int k = 0; k < queries.length; ++k) {
            int i = right[queries[k][0]];
            int j = left[queries[k][1]];
            if (i >= 0 && j >= 0 && i < j) {
                ans[k] = presum[j] - presum[i + 1];
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> platesBetweenCandles(string s, vector<vector<int>>& queries) {
        int n = s.size();
        vector<int> presum(n + 1);
        for (int i = 0; i < n; ++i) presum[i + 1] = presum[i] + (s[i] == '*');
        vector<int> left(n);
        vector<int> right(n);
        for (int i = 0, l = -1; i < n; ++i) {
            if (s[i] == '|') l = i;
            left[i] = l;
        }
        for (int i = n - 1, r = -1; i >= 0; --i) {
            if (s[i] == '|') r = i;
            right[i] = r;
        }
        vector<int> ans(queries.size());
        for (int k = 0; k < queries.size(); ++k) {
            int i = right[queries[k][0]];
            int j = left[queries[k][1]];
            if (i >= 0 && j >= 0 && i < j) ans[k] = presum[j] - presum[i + 1];
        }
        return ans;
    }
};
```

#### Go

```go
func platesBetweenCandles(s string, queries [][]int) []int {
	n := len(s)
	presum := make([]int, n+1)
	for i := range s {
		if s[i] == '*' {
			presum[i+1] = 1
		}
		presum[i+1] += presum[i]
	}
	left, right := make([]int, n), make([]int, n)
	for i, l := 0, -1; i < n; i++ {
		if s[i] == '|' {
			l = i
		}
		left[i] = l
	}
	for i, r := n-1, -1; i >= 0; i-- {
		if s[i] == '|' {
			r = i
		}
		right[i] = r
	}
	ans := make([]int, len(queries))
	for k, q := range queries {
		i, j := right[q[0]], left[q[1]]
		if i >= 0 && j >= 0 && i < j {
			ans[k] = presum[j] - presum[i+1]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
