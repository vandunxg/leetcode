---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.08. Permutation II](https://leetcode.cn/problems/permutation-ii-lcci)

[中文文档](/lcci/08.08.Permutation%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một method để tính tất cả các hoán vị của một chuỗi mà các ký tự không nhất thiết phải khác nhau. Danh sách các hoán vị không được chứa phần tử trùng lặp.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào: </strong>S = &quot;qqe&quot;

<strong>Đầu ra: </strong>[&quot;eqq&quot;,&quot;qeq&quot;,&quot;qqe&quot;]

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào: </strong>S = &quot;ab&quot;

<strong>Đầu ra: </strong>[&quot;ab&quot;, &quot;ba&quot;]

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li>Tất cả các ký tự đều là chữ cái tiếng Anh.</li>
	<li><code>1 &lt;= S.length &lt;= 9</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Backtracking

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự có thể lặp lại, nên chỉ dựa vào “chỉ số chưa được sử dụng” sẽ tạo ra các chuỗi trùng lặp.
>
> Sắp xếp sẽ gom các chữ cái giống nhau lại. Một bản sao xuất hiện sau chỉ được sử dụng khi bản sao trước đó đã nằm trong prefix, nhờ đó mỗi giá trị chỉ được thử một lần ở mỗi độ sâu.
>
> Điều kiện `j==0 or s[j]!=s[j-1] or vis[j-1]` mã hóa thứ tự đó. Phần backtracking còn lại giống với trường hợp các ký tự khác nhau.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp chuỗi theo các ký tự, để các ký tự trùng lặp nằm cạnh nhau và dễ loại bỏ kết quả trùng lặp hơn.

Sau đó, ta thiết kế một hàm $\textit{dfs}(i)$, biểu diễn ký tự cần được điền vào vị trí thứ $i$. Cách triển khai cụ thể của hàm này như sau:

- Nếu $i = n$, nghĩa là ta đã điền đầy đủ tất cả các vị trí, thêm hoán vị hiện tại vào mảng kết quả rồi trả về.
- Nếu không, ta liệt kê ký tự $\textit{s}[j]$ cho vị trí thứ $i$, trong đó $j$ nằm trong đoạn $[0, n - 1]$. Ta cần đảm bảo $\textit{s}[j]$ chưa được sử dụng và khác với ký tự đã được liệt kê trước đó, để bảo đảm hoán vị hiện tại không bị trùng lặp. Nếu thỏa mãn các điều kiện, ta có thể điền $\textit{s}[j]$ rồi tiếp tục đệ quy điền vị trí tiếp theo bằng cách gọi $\textit{dfs}(i + 1)$. Sau khi lời gọi đệ quy kết thúc, ta cần đánh dấu $\textit{s}[j]$ là chưa được sử dụng để phục vụ cho các lần liệt kê tiếp theo.

Trong hàm chính, trước tiên ta sắp xếp chuỗi, sau đó gọi $\textit{dfs}(0)$ để bắt đầu điền từ vị trí thứ 0, và cuối cùng trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times n!)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $s$. Ta cần thực hiện $n!$ lần liệt kê, và mỗi lần liệt kê cần $O(n)$ thời gian để kiểm tra trùng lặp. Ngoài ra, ta cần một mảng đánh dấu để ghi nhận vị trí nào đã được sử dụng, nên độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def permutation(self, S: str) -> List[str]:
        def dfs(i: int):
            if i >= n:
                ans.append("".join(t))
                return
            for j, c in enumerate(s):
                if not vis[j] and (j == 0 or s[j] != s[j - 1] or vis[j - 1]):
                    vis[j] = True
                    t[i] = c
                    dfs(i + 1)
                    vis[j] = False

        s = sorted(S)
        ans = []
        t = s[:]
        n = len(s)
        vis = [False] * n
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
        int n = S.length();
        s = S.toCharArray();
        Arrays.sort(s);
        t = new char[n];
        vis = new boolean[n];
        dfs(0);
        return ans.toArray(new String[0]);
    }

    private void dfs(int i) {
        if (i >= s.length) {
            ans.add(new String(t));
            return;
        }
        for (int j = 0; j < s.length; ++j) {
            if (!vis[j] && (j == 0 || s[j] != s[j - 1] || vis[j - 1])) {
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
        ranges::sort(S);
        string t = S;
        int n = t.size();
        vector<bool> vis(n);
        vector<string> ans;
        auto dfs = [&](this auto&& dfs, int i) {
            if (i >= n) {
                ans.emplace_back(t);
                return;
            }
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (j == 0 || S[j] != S[j - 1] || vis[j - 1])) {
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
	s := []byte(S)
	sort.Slice(s, func(i, j int) bool { return s[i] < s[j] })
	t := slices.Clone(s)
	vis := make([]bool, len(s))
	var dfs func(int)
	dfs = func(i int) {
		if i >= len(s) {
			ans = append(ans, string(t))
			return
		}
		for j := range s {
			if !vis[j] && (j == 0 || s[j] != s[j-1] || vis[j-1]) {
				vis[j] = true
				t[i] = s[j]
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
    const s: string[] = S.split('').sort();
    const n = s.length;
    const t = Array(n).fill('');
    const vis: boolean[] = Array(n).fill(false);
    const ans: string[] = [];
    const dfs = (i: number) => {
        if (i >= n) {
            ans.push(t.join(''));
            return;
        }
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && (j === 0 || s[j] !== s[j - 1] || vis[j - 1])) {
                vis[j] = true;
                t[i] = s[j];
                dfs(i + 1);
                vis[j] = false;
            }
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
    const s = S.split('').sort();
    const n = s.length;
    const t = Array(n).fill('');
    const vis = Array(n).fill(false);
    const ans = [];
    const dfs = i => {
        if (i >= n) {
            ans.push(t.join(''));
            return;
        }
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && (j === 0 || s[j] !== s[j - 1] || vis[j - 1])) {
                vis[j] = true;
                t[i] = s[j];
                dfs(i + 1);
                vis[j] = false;
            }
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
        var s: [Character] = Array(S).sorted()
        var t: [Character] = Array(repeating: " ", count: s.count)
        var vis: [Bool] = Array(repeating: false, count: s.count)
        let n = s.count

        func dfs(_ i: Int) {
            if i >= n {
                ans.append(String(t))
                return
            }
            for j in 0..<n {
                if !vis[j] && (j == 0 || s[j] != s[j - 1] || vis[j - 1]) {
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
