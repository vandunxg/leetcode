---
comments: true
difficulty: Medium
rating: 2080
source: Biweekly Contest 43 Q3
tags:
    - Array
    - Backtracking
---

<!-- problem:start -->

# [1718. Construct the Lexicographically Largest Valid Sequence](https://leetcode.com/problems/construct-the-lexicographically-largest-valid-sequence)

[中文文档](/solution/1700-1799/1718.Construct%20the%20Lexicographically%20Largest%20Valid%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy tìm một dãy có các phần tử trong khoảng <code>[1, n]</code> thỏa mãn:</p>

<ul>
	<li>Số nguyên <code>1</code> xuất hiện một lần trong dãy.</li>
	<li>Mỗi số nguyên từ <code>2</code> đến <code>n</code> xuất hiện hai lần trong dãy.</li>
	<li>Với mọi số nguyên <code>i</code> từ <code>2</code> đến <code>n</code>, <strong>khoảng cách</strong> giữa hai lần xuất hiện của <code>i</code> chính xác bằng <code>i</code>.</li>
</ul>

<p><strong>Khoảng cách</strong> giữa hai số <code>a[i]</code> và <code>a[j]</code> trong dãy là hiệu tuyệt đối giữa hai chỉ số, <code>|j - i|</code>.</p>

<p>Trả về <em>dãy <strong>lớn nhất theo thứ tự từ điển</strong></em><em>. Với các ràng buộc đã cho, luôn tồn tại lời giải. </em></p>

<p>Dãy <code>a</code> lớn hơn dãy <code>b</code> theo thứ tự từ điển (khi có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, số trong dãy <code>a</code> lớn hơn số tương ứng trong <code>b</code>. Ví dụ, <code>[0,1,9,0]</code> lớn hơn <code>[0,1,5,6]</code> vì vị trí khác nhau đầu tiên là vị trí thứ ba, và <code>9</code> lớn hơn <code>5</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> [3,1,2,3,2]
<strong>Explanation:</strong> [2,3,2,1,3] cũng là một dãy hợp lệ, nhưng [3,1,2,3,2] là dãy hợp lệ lớn nhất theo thứ tự từ điển.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 5
<strong>Output:</strong> [5,3,1,4,3,5,2,4,2]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dãy có độ dài $2n-1$: $1$ xuất hiện một lần, còn mỗi $i\in[2,n]$ xuất hiện hai lần cách nhau $i$. $n$ đủ nhỏ để dùng backtracking.
>
> Dãy lớn nhất theo thứ tự từ điển phải thử các giá trị lớn trước. Điền các vị trí trống từ trái sang phải, thử từ $n$ xuống $2$, rồi thử $1$.
>
> $\textit{path}$ lưu các giá trị đã đặt và $\textit{cnt}$ lưu số lần còn lại. Giá trị $i$ chiếm các vị trí $u$ và $u+i$. Điền đủ $2n-1$ vị trí sẽ cho lời giải luôn tồn tại theo đề bài.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constructDistancedSequence(self, n: int) -> List[int]:
        def dfs(u):
            if u == n * 2:
                return True
            if path[u]:
                return dfs(u + 1)
            for i in range(n, 1, -1):
                if cnt[i] and u + i < n * 2 and path[u + i] == 0:
                    cnt[i] = 0
                    path[u] = path[u + i] = i
                    if dfs(u + 1):
                        return True
                    path[u] = path[u + i] = 0
                    cnt[i] = 2
            if cnt[1]:
                cnt[1], path[u] = 0, 1
                if dfs(u + 1):
                    return True
                path[u], cnt[1] = 0, 1
            return False

        path = [0] * (n * 2)
        cnt = [2] * (n * 2)
        cnt[1] = 1
        dfs(1)
        return path[1:]
```

#### Java

```java
class Solution {
    private int[] path;
    private int[] cnt;
    private int n;

    public int[] constructDistancedSequence(int n) {
        this.n = n;
        path = new int[n * 2];
        cnt = new int[n * 2];
        Arrays.fill(cnt, 2);
        cnt[1] = 1;
        dfs(1);
        int[] ans = new int[n * 2 - 1];
        for (int i = 0; i < ans.length; ++i) {
            ans[i] = path[i + 1];
        }
        return ans;
    }

    private boolean dfs(int u) {
        if (u == n * 2) {
            return true;
        }
        if (path[u] > 0) {
            return dfs(u + 1);
        }
        for (int i = n; i > 1; --i) {
            if (cnt[i] > 0 && u + i < n * 2 && path[u + i] == 0) {
                cnt[i] = 0;
                path[u] = i;
                path[u + i] = i;
                if (dfs(u + 1)) {
                    return true;
                }
                cnt[i] = 2;
                path[u] = 0;
                path[u + i] = 0;
            }
        }
        if (cnt[1] > 0) {
            path[u] = 1;
            cnt[1] = 0;
            if (dfs(u + 1)) {
                return true;
            }
            cnt[1] = 1;
            path[u] = 0;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;
    vector<int> cnt, path;

    vector<int> constructDistancedSequence(int _n) {
        n = _n;
        cnt.resize(n * 2, 2);
        path.resize(n * 2);
        cnt[1] = 1;
        dfs(1);
        vector<int> ans;
        for (int i = 1; i < n * 2; ++i) ans.push_back(path[i]);
        return ans;
    }

    bool dfs(int u) {
        if (u == n * 2) return 1;
        if (path[u]) return dfs(u + 1);
        for (int i = n; i > 1; --i) {
            if (cnt[i] && u + i < n * 2 && !path[u + i]) {
                path[u] = path[u + i] = i;
                cnt[i] = 0;
                if (dfs(u + 1)) return 1;
                cnt[i] = 2;
                path[u] = path[u + i] = 0;
            }
        }
        if (cnt[1]) {
            path[u] = 1;
            cnt[1] = 0;
            if (dfs(u + 1)) return 1;
            cnt[1] = 1;
            path[u] = 0;
        }
        return 0;
    }
};
```

#### Go

```go
func constructDistancedSequence(n int) []int {
	path := make([]int, n*2)
	cnt := make([]int, n*2)
	for i := range cnt {
		cnt[i] = 2
	}
	cnt[1] = 1
	var dfs func(u int) bool
	dfs = func(u int) bool {
		if u == n*2 {
			return true
		}
		if path[u] > 0 {
			return dfs(u + 1)
		}
		for i := n; i > 1; i-- {
			if cnt[i] > 0 && u+i < n*2 && path[u+i] == 0 {
				cnt[i] = 0
				path[u], path[u+i] = i, i
				if dfs(u + 1) {
					return true
				}
				cnt[i] = 2
				path[u], path[u+i] = 0, 0
			}
		}
		if cnt[1] > 0 {
			cnt[1] = 0
			path[u] = 1
			if dfs(u + 1) {
				return true
			}
			cnt[1] = 1
			path[u] = 0
		}
		return false
	}
	dfs(1)
	return path[1:]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
