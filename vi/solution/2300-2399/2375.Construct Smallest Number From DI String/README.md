---
comments: true
difficulty: Medium
rating: 1641
source: Weekly Contest 306 Q3
tags:
    - Stack
    - Greedy
    - String
    - Backtracking
---

<!-- problem:start -->

# [2375. Construct Smallest Number From DI String](https://leetcode.com/problems/construct-smallest-number-from-di-string)

[中文文档](/solution/2300-2399/2375.Construct%20Smallest%20Number%20From%20DI%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>pattern</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code> và chỉ gồm các ký tự <code>&#39;I&#39;</code> biểu thị <strong>tăng</strong> và <code>&#39;D&#39;</code> biểu thị <strong>giảm</strong>.</p>

<p>Một chuỗi <code>num</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n + 1</code>, được tạo theo các điều kiện sau:</p>

<ul>
	<li><code>num</code> chỉ gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>, trong đó mỗi chữ số được sử dụng <strong>nhiều nhất</strong> một lần.</li>
	<li>Nếu <code>pattern[i] == &#39;I&#39;</code> thì <code>num[i] &lt; num[i + 1]</code>.</li>
	<li>Nếu <code>pattern[i] == &#39;D&#39;</code> thì <code>num[i] &gt; num[i + 1]</code>.</li>
</ul>

<p>Trả về <em>chuỗi </em><code>num</code><em> <strong>nhỏ nhất theo thứ tự từ điển</strong> có thể thỏa mãn các điều kiện trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pattern = &quot;IIIDIDDD&quot;
<strong>Đầu ra:</strong> &quot;123549876&quot;
<strong>Giải thích:
</strong>Tại các chỉ số 0, 1, 2 và 4, ta phải có num[i] &lt; num[i+1].
Tại các chỉ số 3, 5, 6 và 7, ta phải có num[i] &gt; num[i+1].
Một số giá trị có thể có của num là &quot;245639871&quot;, &quot;135749862&quot; và &quot;123849765&quot;.
Có thể chứng minh rằng &quot;123549876&quot; là num nhỏ nhất có thể thỏa mãn các điều kiện.
Lưu ý rằng &quot;123414321&quot; là không thể vì chữ số &#39;1&#39; được sử dụng nhiều hơn một lần.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pattern = &quot;DDD&quot;
<strong>Đầu ra:</strong> &quot;4321&quot;
<strong>Giải thích:</strong>
Một số giá trị có thể có của num là &quot;9876&quot;, &quot;7321&quot; và &quot;8742&quot;.
Có thể chứng minh rằng &quot;4321&quot; là num nhỏ nhất có thể thỏa mãn các điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 8</code></li>
	<li><code>pattern</code> chỉ gồm các chữ cái <code>&#39;I&#39;</code> và <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải sử dụng các chữ số $1..9$ nhiều nhất một lần, tuân theo $I/D$ và tạo ra chuỗi nhỏ nhất theo thứ tự từ điển. Vì $|pattern| \le 8$, ta có thể duyệt các hoán vị theo thứ tự tăng dần, với nhiều nhất $9!$ hoán vị.
>
> DFS thử các chữ số chưa sử dụng; với tiền tố hiện tại, ta áp dụng điều kiện $I$ hoặc $D$ cuối cùng. Vì thử các chữ số nhỏ hơn trước, chuỗi hoàn chỉnh đầu tiên là đáp án tối ưu và phép tìm kiếm dừng lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestNumber(self, pattern: str) -> str:
        def dfs(u):
            nonlocal ans
            if ans:
                return
            if u == len(pattern) + 1:
                ans = ''.join(t)
                return
            for i in range(1, 10):
                if not vis[i]:
                    if u and pattern[u - 1] == 'I' and int(t[-1]) >= i:
                        continue
                    if u and pattern[u - 1] == 'D' and int(t[-1]) <= i:
                        continue
                    vis[i] = True
                    t.append(str(i))
                    dfs(u + 1)
                    vis[i] = False
                    t.pop()

        vis = [False] * 10
        t = []
        ans = None
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private boolean[] vis = new boolean[10];
    private StringBuilder t = new StringBuilder();
    private String p;
    private String ans;

    public String smallestNumber(String pattern) {
        p = pattern;
        dfs(0);
        return ans;
    }

    private void dfs(int u) {
        if (ans != null) {
            return;
        }
        if (u == p.length() + 1) {
            ans = t.toString();
            return;
        }
        for (int i = 1; i < 10; ++i) {
            if (!vis[i]) {
                if (u > 0 && p.charAt(u - 1) == 'I' && t.charAt(u - 1) - '0' >= i) {
                    continue;
                }
                if (u > 0 && p.charAt(u - 1) == 'D' && t.charAt(u - 1) - '0' <= i) {
                    continue;
                }
                vis[i] = true;
                t.append(i);
                dfs(u + 1);
                t.deleteCharAt(t.length() - 1);
                vis[i] = false;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string ans = "";
    string pattern;
    vector<bool> vis;
    string t = "";

    string smallestNumber(string pattern) {
        this->pattern = pattern;
        vis.assign(10, false);
        dfs(0);
        return ans;
    }

    void dfs(int u) {
        if (ans != "") return;
        if (u == pattern.size() + 1) {
            ans = t;
            return;
        }
        for (int i = 1; i < 10; ++i) {
            if (!vis[i]) {
                if (u && pattern[u - 1] == 'I' && t.back() - '0' >= i) continue;
                if (u && pattern[u - 1] == 'D' && t.back() - '0' <= i) continue;
                vis[i] = true;
                t += to_string(i);
                dfs(u + 1);
                t.pop_back();
                vis[i] = false;
            }
        }
    }
};
```

#### Go

```go
func smallestNumber(pattern string) string {
	vis := make([]bool, 10)
	t := []byte{}
	ans := ""
	var dfs func(u int)
	dfs = func(u int) {
		if ans != "" {
			return
		}
		if u == len(pattern)+1 {
			ans = string(t)
			return
		}
		for i := 1; i < 10; i++ {
			if !vis[i] {
				if u > 0 && pattern[u-1] == 'I' && int(t[len(t)-1]-'0') >= i {
					continue
				}
				if u > 0 && pattern[u-1] == 'D' && int(t[len(t)-1]-'0') <= i {
					continue
				}
				vis[i] = true
				t = append(t, byte('0'+i))
				dfs(u + 1)
				vis[i] = false
				t = t[:len(t)-1]
			}
		}
	}
	dfs(0)
	return ans
}
```

#### TypeScript

```ts
function smallestNumber(pattern: string): string {
    const n = pattern.length;
    const res = new Array(n + 1).fill('');
    const vis = new Array(n + 1).fill(false);
    const dfs = (i: number, num: number) => {
        if (i === n) {
            return;
        }

        if (vis[num]) {
            vis[num] = false;
            if (pattern[i] === 'I') {
                dfs(i - 1, num - 1);
            } else {
                dfs(i - 1, num + 1);
            }
            return;
        }

        vis[num] = true;
        res[i] = num;

        if (pattern[i] === 'I') {
            for (let j = res[i] + 1; j <= n + 1; j++) {
                if (!vis[j]) {
                    dfs(i + 1, j);
                    return;
                }
            }
            vis[num] = false;
            dfs(i, num - 1);
        } else {
            for (let j = res[i] - 1; j > 0; j--) {
                if (!vis[j]) {
                    dfs(i + 1, j);
                    return;
                }
            }
            vis[num] = false;
            dfs(i, num + 1);
        }
    };
    dfs(0, 1);
    for (let i = 1; i <= n + 1; i++) {
        if (!vis[i]) {
            res[n] = i;
            break;
        }
    }

    return res.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
