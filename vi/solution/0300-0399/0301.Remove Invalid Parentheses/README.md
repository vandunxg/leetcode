---
comments: true
difficulty: Hard
tags:
    - Breadth-First Search
    - String
    - Backtracking
---

<!-- problem:start -->

# [301. Remove Invalid Parentheses](https://leetcode.com/problems/remove-invalid-parentheses)

[中文文档](/solution/0300-0399/0301.Remove%20Invalid%20Parentheses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm dấu ngoặc đơn và chữ cái. Hãy xóa số lượng dấu ngoặc không hợp lệ ít nhất để chuỗi đầu vào trở thành hợp lệ.</p>

<p>Hãy trả về <em>danh sách các chuỗi <strong>khác nhau</strong> hợp lệ với số lần xóa ít nhất</em>. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;()())()&quot;
<strong>Đầu ra:</strong> [&quot;(())()&quot;,&quot;()()()&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(a)())()&quot;
<strong>Đầu ra:</strong> [&quot;(a())()&quot;,&quot;(a)()()&quot;]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;)(&quot;
<strong>Đầu ra:</strong> [&quot;&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 25</code></li>
	<li><code>s</code> chỉ gồm chữ cái tiếng Anh viết thường và dấu ngoặc <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code>.</li>
	<li><code>s</code> có tối đa <code>20</code> dấu ngoặc.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xóa ít dấu ngoặc nhất có thể và liệt kê mọi chuỗi tối ưu. Quyết định giữ hay xóa từng dấu ngoặc sẽ tạo ra quá nhiều nhánh, phần lớn trong số đó không đạt số lần xóa tối thiểu.
>
> Trước tiên, duyệt chuỗi để tìm số dấu ngoặc trái $l$ và phải $r$ tối thiểu cần xóa. Khi tìm kiếm, ta theo dõi số lượng cần xóa còn lại và số dấu ngoặc trái/phải hiện có; cắt tỉa nếu số ký tự còn lại không đủ để xóa theo quota hoặc tiền tố hiện tại có nhiều dấu ngoặc phải hơn dấu ngoặc trái. Thử xóa dấu ngoặc nếu quota còn, sau đó thử giữ lại; dùng set để loại kết quả trùng. Chỉ các nhánh xóa đúng số lượng tối thiểu mới được giữ lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        def dfs(i, l, r, lcnt, rcnt, t):
            if i == n:
                if l == 0 and r == 0:
                    ans.add(t)
                return
            if n - i < l + r or lcnt < rcnt:
                return
            if s[i] == '(' and l:
                dfs(i + 1, l - 1, r, lcnt, rcnt, t)
            elif s[i] == ')' and r:
                dfs(i + 1, l, r - 1, lcnt, rcnt, t)
            dfs(i + 1, l, r, lcnt + (s[i] == '('), rcnt + (s[i] == ')'), t + s[i])

        l = r = 0
        for c in s:
            if c == '(':
                l += 1
            elif c == ')':
                if l:
                    l -= 1
                else:
                    r += 1
        ans = set()
        n = len(s)
        dfs(0, l, r, 0, 0, '')
        return list(ans)
```

#### Java

```java
class Solution {
    private String s;
    private int n;
    private Set<String> ans = new HashSet<>();

    public List<String> removeInvalidParentheses(String s) {
        this.s = s;
        this.n = s.length();
        int l = 0, r = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                ++l;
            } else if (c == ')') {
                if (l > 0) {
                    --l;
                } else {
                    ++r;
                }
            }
        }
        dfs(0, l, r, 0, 0, "");
        return new ArrayList<>(ans);
    }

    private void dfs(int i, int l, int r, int lcnt, int rcnt, String t) {
        if (i == n) {
            if (l == 0 && r == 0) {
                ans.add(t);
            }
            return;
        }
        if (n - i < l + r || lcnt < rcnt) {
            return;
        }
        char c = s.charAt(i);
        if (c == '(' && l > 0) {
            dfs(i + 1, l - 1, r, lcnt, rcnt, t);
        }
        if (c == ')' && r > 0) {
            dfs(i + 1, l, r - 1, lcnt, rcnt, t);
        }
        int x = c == '(' ? 1 : 0;
        int y = c == ')' ? 1 : 0;
        dfs(i + 1, l, r, lcnt + x, rcnt + y, t + c);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        unordered_set<string> ans;
        int l = 0, r = 0, n = s.size();
        for (char& c : s) {
            if (c == '(') {
                ++l;
            } else if (c == ')') {
                if (l) {
                    --l;
                } else {
                    ++r;
                }
            }
        }
        function<void(int, int, int, int, int, string)> dfs;
        dfs = [&](int i, int l, int r, int lcnt, int rcnt, string t) {
            if (i == n) {
                if (l == 0 && r == 0) {
                    ans.insert(t);
                }
                return;
            }
            if (n - i < l + r || lcnt < rcnt) {
                return;
            }
            if (s[i] == '(' && l) {
                dfs(i + 1, l - 1, r, lcnt, rcnt, t);
            }
            if (s[i] == ')' && r) {
                dfs(i + 1, l, r - 1, lcnt, rcnt, t);
            }
            int x = s[i] == '(' ? 1 : 0;
            int y = s[i] == ')' ? 1 : 0;
            dfs(i + 1, l, r, lcnt + x, rcnt + y, t + s[i]);
        };

        dfs(0, l, r, 0, 0, "");
        return vector<string>(ans.begin(), ans.end());
    }
};
```

#### Go

```go
func removeInvalidParentheses(s string) []string {
	vis := map[string]bool{}
	l, r, n := 0, 0, len(s)
	for _, c := range s {
		if c == '(' {
			l++
		} else if c == ')' {
			if l > 0 {
				l--
			} else {
				r++
			}
		}
	}
	var dfs func(i, l, r, lcnt, rcnt int, t string)
	dfs = func(i, l, r, lcnt, rcnt int, t string) {
		if i == n {
			if l == 0 && r == 0 {
				vis[t] = true
			}
			return
		}
		if n-i < l+r || lcnt < rcnt {
			return
		}
		if s[i] == '(' && l > 0 {
			dfs(i+1, l-1, r, lcnt, rcnt, t)
		}
		if s[i] == ')' && r > 0 {
			dfs(i+1, l, r-1, lcnt, rcnt, t)
		}
		x, y := 0, 0
		if s[i] == '(' {
			x = 1
		} else if s[i] == ')' {
			y = 1
		}
		dfs(i+1, l, r, lcnt+x, rcnt+y, t+string(s[i]))
	}
	dfs(0, l, r, 0, 0, "")
	ans := make([]string, 0, len(vis))
	for v := range vis {
		ans = append(ans, v)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
