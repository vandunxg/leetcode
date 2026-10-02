---
comments: true
difficulty: Medium
rating: 1541
source: Weekly Contest 156 Q3
tags:
    - Stack
    - String
---

<!-- problem:start -->

# [1209. Remove All Adjacent Duplicates in String II](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii)

[中文文档](/solution/1200-1299/1209.Remove%20All%20Adjacent%20Duplicates%20in%20String%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>. Một lần <strong>xóa phần tử trùng lặp</strong> với <code>k</code> là chọn <code>k</code> ký tự liền kề và giống nhau trong <code>s</code> rồi xóa chúng, khiến phần bên trái và bên phải của chuỗi con vừa xóa nối lại với nhau.</p>

<p>Ta liên tục thực hiện thao tác <strong>xóa phần tử trùng lặp</strong> với <code>k</code> trên <code>s</code> cho đến khi không thể thực hiện thêm.</p>

<p>Hãy trả về <em>chuỗi cuối cùng sau khi thực hiện hết các thao tác xóa phần tử trùng lặp như trên</em>. Đảm bảo đáp án là <strong>duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;, k = 2
<strong>Output:</strong> &quot;abcd&quot;
<strong>Giải thích: </strong>Không có gì cần xóa.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;deeedbbcccbdaa&quot;, k = 3
<strong>Output:</strong> &quot;aa&quot;
<strong>Giải thích:
</strong>Đầu tiên xóa &quot;eee&quot; và &quot;ccc&quot;, thu được &quot;ddbbbdaa&quot;
Sau đó xóa &quot;bbb&quot;, thu được &quot;dddaa&quot;
Cuối cùng xóa &quot;ddd&quot;, thu được &quot;aa&quot;</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;pbbcggttciiippooaais&quot;, k = 2
<strong>Output:</strong> &quot;ps&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= k &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Quét và xóa lặp lại các nhóm gồm $k$ ký tự giống nhau có thể khiến ta phải quét lại chuỗi nhiều lần khi $n \le 10^5$. Thao tác xóa chỉ ảnh hưởng cục bộ: các đoạn ký tự liền kề có thể nối lại, và hai phía có thể trở thành kề nhau sau khi xóa.
>
> Stack lưu các ký tự còn lại và độ dài đoạn tương ứng theo thứ tự từ trái sang phải. Nếu ký tự hiện tại trùng với phần tử trên cùng, tăng bộ đếm; khi đạt $k$, pop đoạn đó, tức là xóa một nhóm.
>
> Ta lưu $(char, count)$ và lấy bộ đếm modulo $k$ để pop ngay khi một đoạn đủ độ dài. Sau một lượt duyệt, stack chứa chuỗi cuối cùng.

<!-- thinking:end -->

Ta duyệt chuỗi $s$ và duy trì một stack lưu các ký tự cùng số lần xuất hiện liên tiếp của chúng. Khi gặp ký tự $c$, nếu ký tự trên cùng stack cũng là $c$ thì tăng bộ đếm của phần tử đó lên 1; nếu không, push ký tự $c$ cùng bộ đếm $1$ vào stack. Khi bộ đếm trên cùng bằng $k$, pop phần tử đó khỏi stack.

Sau khi duyệt xong chuỗi $s$, các phần tử còn lại trong stack tạo thành kết quả cuối cùng. Ta có thể lần lượt pop chúng, nối thành một chuỗi và đó là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDuplicates(self, s: str, k: int) -> str:
        stk = []
        for c in s:
            if stk and stk[-1][0] == c:
                stk[-1][1] = (stk[-1][1] + 1) % k
                if stk[-1][1] == 0:
                    stk.pop()
            else:
                stk.append([c, 1])
        ans = [c * v for c, v in stk]
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        Deque<int[]> stk = new ArrayDeque<>();
        for (int i = 0; i < s.length(); ++i) {
            int j = s.charAt(i) - 'a';
            if (!stk.isEmpty() && stk.peek()[0] == j) {
                stk.peek()[1] = (stk.peek()[1] + 1) % k;
                if (stk.peek()[1] == 0) {
                    stk.pop();
                }
            } else {
                stk.push(new int[] {j, 1});
            }
        }
        StringBuilder ans = new StringBuilder();
        for (var e : stk) {
            char c = (char) (e[0] + 'a');
            for (int i = 0; i < e[1]; ++i) {
                ans.append(c);
            }
        }
        ans.reverse();
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeDuplicates(string s, int k) {
        vector<pair<char, int>> stk;
        for (char& c : s) {
            if (stk.size() && stk.back().first == c) {
                stk.back().second = (stk.back().second + 1) % k;
                if (stk.back().second == 0) {
                    stk.pop_back();
                }
            } else {
                stk.push_back({c, 1});
            }
        }
        string ans;
        for (auto [c, v] : stk) {
            ans += string(v, c);
        }
        return ans;
    }
};
```

#### Go

```go
func removeDuplicates(s string, k int) string {
	stk := []pair{}
	for _, c := range s {
		if len(stk) > 0 && stk[len(stk)-1].c == c {
			stk[len(stk)-1].v = (stk[len(stk)-1].v + 1) % k
			if stk[len(stk)-1].v == 0 {
				stk = stk[:len(stk)-1]
			}
		} else {
			stk = append(stk, pair{c, 1})
		}
	}
	ans := []rune{}
	for _, e := range stk {
		for i := 0; i < e.v; i++ {
			ans = append(ans, e.c)
		}
	}
	return string(ans)
}

type pair struct {
	c rune
	v int
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
