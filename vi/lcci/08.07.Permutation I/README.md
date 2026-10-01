---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.07. Permutation I](https://leetcode.cn/problems/permutation-i-lcci)

[中文文档](/lcci/08.07.Permutation%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một phương thức để tính tất cả các hoán vị của một chuỗi gồm các ký tự khác nhau.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: S = &quot;qwe&quot;

<strong> Đầu ra</strong>: [&quot;qwe&quot;, &quot;qew&quot;, &quot;wqe&quot;, &quot;weq&quot;, &quot;ewq&quot;, &quot;eqw&quot;]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: S = &quot;ab&quot;

<strong> Đầu ra</strong>: [&quot;ab&quot;, &quot;ba&quot;]

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li>Tất cả các ký tự là chữ cái tiếng Anh.</li>
	<li><code>1 &lt;= S.length &lt;= 9</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS (Quay lui)

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm tất cả các hoán vị của một chuỗi có các ký tự phân biệt. Kích thước $n!$ khớp với cận dưới của đầu ra.
>
> Điền các vị trí từ trái sang phải bằng các ký tự chưa sử dụng, được theo dõi bởi $vis$.
>
> $dfs(i)$ ghi một chỉ số chưa dùng của $S$ vào $t[i]$ và ghi nhận một chuỗi khi $i=n$. Bỏ đánh dấu khi quay lui giúp tạo ra mỗi hoán vị đúng một lần.

<!-- thinking:end -->

Ta xây dựng một hàm $\textit{dfs}(i)$ để biểu diễn rằng $i$ vị trí đầu tiên đã được điền và hiện cần điền vị trí thứ $(i+1)$. Liệt kê tất cả các ký tự có thể chọn; nếu ký tự đó chưa được sử dụng, điền ký tự này và tiếp tục điền vị trí tiếp theo cho đến khi tất cả vị trí đều được điền.

Độ phức tạp thời gian là $O(n \times n!)$, trong đó $n$ là độ dài chuỗi. Có tổng cộng $n!$ hoán vị và cần $O(n)$ thời gian để xây dựng mỗi hoán vị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def permutation(self, S: str) -> List[str]:
        def dfs(i: int):
            if i >= n:
                ans.append("".join(t))
                return
            for j, c in enumerate(S):
                if not vis[j]:
                    vis[j] = True
                    t[i] = c
                    dfs(i + 1)
                    vis[j] = False

        ans = []
        n = len(S)
        vis = [False] * n
        t = list(S)
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private char[] s;
    private char[] t;
    private boolean[] vis;
    private List<String> ans = new ArrayList<>();

    public String[] permutation(String S) {
        s = S.toCharArray();
        int n = s.length;
        vis = new boolean[n];
        t = new char[n];
        dfs(0);
        return ans.toArray(new String[0]);
    }

    private void dfs(int i) {
        if (i >= s.length) {
            ans.add(new String(t));
            return;
        }
        for (int j = 0; j < s.length; ++j) {
            if (!vis[j]) {
                vis[j] = true;
                t[i] = s[j];
                dfs(i + 1);
                vis[j] = false;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> permutation(string S) {
        int n = S.size();
        vector<bool> vis(n);
        string t = S;
        vector<string> ans;
        auto dfs = [&](this auto&& dfs, int i) {
            if (i >= n) {
                ans.emplace_back(t);
                return;
            }
            for (int j = 0; j < n; ++j) {
                if (!vis[j]) {
                    vis[j] = true;
                    t[i] = S[j];
                    dfs(i + 1);
                    vis[j] = false;
                }
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func permutation(S string) (ans []string) {
	t := []byte(S)
	n := len(t)
	vis := make([]bool, n)
	var dfs func(int)
	dfs = func(i int) {
		if i >= n {
			ans = append(ans, string(t))
			return
		}
		for j := range S {
			if !vis[j] {
				vis[j] = true
				t[i] = S[j]
				dfs(i + 1)
				vis[j] = false
			}
		}
	}
	dfs(0)
	return
}
```

#### TypeScript

```ts
function permutation(S: string): string[] {
    const n = S.length;
    const vis: boolean[] = Array(n).fill(false);
    const ans: string[] = [];
    const t: string[] = Array(n).fill('');
    const dfs = (i: number) => {
        if (i >= n) {
            ans.push(t.join(''));
            return;
        }
        for (let j = 0; j < n; ++j) {
            if (vis[j]) {
                continue;
            }
            vis[j] = true;
            t[i] = S[j];
            dfs(i + 1);
            vis[j] = false;
        }
    };
    dfs(0);
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} S
 * @return {string[]}
 */
var permutation = function (S) {
    const n = S.length;
    const vis = Array(n).fill(false);
    const ans = [];
    const t = Array(n).fill('');
    const dfs = i => {
        if (i >= n) {
            ans.push(t.join(''));
            return;
        }
        for (let j = 0; j < n; ++j) {
            if (vis[j]) {
                continue;
            }
            vis[j] = true;
            t[i] = S[j];
            dfs(i + 1);
            vis[j] = false;
        }
    };
    dfs(0);
    return ans;
};
```

#### Swift

```swift
class Solution {
    func permutation(_ S: String) -> [String] {
        var ans: [String] = []
        let s = Array(S)
        var t = s
        var vis = Array(repeating: false, count: s.count)
        let n = s.count

        func dfs(_ i: Int) {
            if i >= n {
                ans.append(String(t))
                return
            }
            for j in 0..<n {
                if !vis[j] {
                    vis[j] = true
                    t[i] = s[j]
                    dfs(i + 1)
                    vis[j] = false
                }
            }
        }

        dfs(0)
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
