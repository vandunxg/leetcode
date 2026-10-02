---
comments: true
difficulty: Medium
tags:
    - String
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [816. Ambiguous Coordinates](https://leetcode.com/problems/ambiguous-coordinates)

[中文文档](/solution/0800-0899/0816.Ambiguous%20Coordinates/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có một cặp tọa độ hai chiều, chẳng hạn <code>&quot;(1, 3)&quot;</code> hoặc <code>&quot;(2, 0.5)&quot;</code>. Sau đó, ta xóa tất cả dấu phẩy, dấu thập phân và khoảng trắng để thu được chuỗi s.</p>

<ul>
	<li>Ví dụ, <code>&quot;(1, 3)&quot;</code> trở thành <code>s = &quot;(13)&quot;</code>, còn <code>&quot;(2, 0.5)&quot;</code> trở thành <code>s = &quot;(205)&quot;</code>.</li>
</ul>

<p>Hãy trả về <em>danh sách các chuỗi biểu diễn mọi khả năng của cặp tọa độ ban đầu</em>.</p>

<p>Biểu diễn ban đầu không có số 0 thừa, nên không thể bắt đầu bằng các số như <code>&quot;00&quot;</code>, <code>&quot;0.0&quot;</code>, <code>&quot;0.00&quot;</code>, <code>&quot;1.0&quot;</code>, <code>&quot;001&quot;</code>, <code>&quot;00.01&quot;</code> hoặc bất kỳ số nào khác có thể biểu diễn bằng ít chữ số hơn. Ngoài ra, dấu thập phân trong một số luôn phải có ít nhất một chữ số đứng trước, nên số ban đầu cũng không thể có dạng <code>&quot;.1&quot;</code>.</p>

<p>Có thể trả danh sách kết quả cuối cùng theo bất kỳ thứ tự nào. Mỗi cặp tọa độ trong kết quả có đúng một dấu cách sau dấu phẩy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(123)&quot;
<strong>Đầu ra:</strong> [&quot;(1, 2.3)&quot;,&quot;(1, 23)&quot;,&quot;(1.2, 3)&quot;,&quot;(12, 3)&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(0123)&quot;
<strong>Đầu ra:</strong> [&quot;(0, 1.23)&quot;,&quot;(0, 12.3)&quot;,&quot;(0, 123)&quot;,&quot;(0.1, 2.3)&quot;,&quot;(0.1, 23)&quot;,&quot;(0.12, 3)&quot;]
<strong>Giải thích:</strong> Không được phép có các giá trị 0.0, 00, 0001 hoặc 00.01.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(00011)&quot;
<strong>Đầu ra:</strong> [&quot;(0, 0.011)&quot;,&quot;(0.001, 1)&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= s.length &lt;= 12</code></li>
	<li><code>s[0] == &#39;(&#39;</code> và <code>s[s.length - 1] == &#39;)&#39;</code>.</li>
	<li>Các ký tự còn lại của <code>s</code> đều là chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chèn dấu phẩy và có thể chèn dấu thập phân bên trong cặp ngoặc. Chuỗi dài tối đa $12$, nên chỉ cần thử mọi vị trí đặt dấu phẩy và dấu thập phân.
>
> Một phần hợp lệ khi và chỉ khi phần nguyên không có số 0 ở đầu (ngoại trừ trường hợp chỉ có một chữ số $0$) và phần thập phân không có số 0 ở cuối. Tạo các khả năng hợp lệ cho hai vế riêng rồi ghép từng cặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def ambiguousCoordinates(self, s: str) -> List[str]:
        def f(i, j):
            res = []
            for k in range(1, j - i + 1):
                l, r = s[i : i + k], s[i + k : j]
                ok = (l == '0' or not l.startswith('0')) and not r.endswith('0')
                if ok:
                    res.append(l + ('.' if k < j - i else '') + r)
            return res

        n = len(s)
        return [
            f'({x}, {y})' for i in range(2, n - 1) for x in f(1, i) for y in f(i, n - 1)
        ]
```

#### Java

```java
class Solution {
    public List<String> ambiguousCoordinates(String s) {
        int n = s.length();
        List<String> ans = new ArrayList<>();
        for (int i = 2; i < n - 1; ++i) {
            for (String x : f(s, 1, i)) {
                for (String y : f(s, i, n - 1)) {
                    ans.add(String.format("(%s, %s)", x, y));
                }
            }
        }
        return ans;
    }

    private List<String> f(String s, int i, int j) {
        List<String> res = new ArrayList<>();
        for (int k = 1; k <= j - i; ++k) {
            String l = s.substring(i, i + k);
            String r = s.substring(i + k, j);
            boolean ok = ("0".equals(l) || !l.startsWith("0")) && !r.endsWith("0");
            if (ok) {
                res.add(l + (k < j - i ? "." : "") + r);
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> ambiguousCoordinates(string s) {
        int n = s.size();
        vector<string> ans;
        auto f = [&](int i, int j) {
            vector<string> res;
            for (int k = 1; k <= j - i; ++k) {
                string l = s.substr(i, k);
                string r = s.substr(i + k, j - i - k);
                bool ok = (l == "0" || l[0] != '0') && r.back() != '0';
                if (ok) {
                    res.push_back(l + (k < j - i ? "." : "") + r);
                }
            }
            return res;
        };
        for (int i = 2; i < n - 1; ++i) {
            for (auto& x : f(1, i)) {
                for (auto& y : f(i, n - 1)) {
                    ans.emplace_back("(" + x + ", " + y + ")");
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func ambiguousCoordinates(s string) []string {
	f := func(i, j int) []string {
		res := []string{}
		for k := 1; k <= j-i; k++ {
			l, r := s[i:i+k], s[i+k:j]
			ok := (l == "0" || l[0] != '0') && (r == "" || r[len(r)-1] != '0')
			if ok {
				t := ""
				if k < j-i {
					t = "."
				}
				res = append(res, l+t+r)
			}
		}
		return res
	}

	n := len(s)
	ans := []string{}
	for i := 2; i < n-1; i++ {
		for _, x := range f(1, i) {
			for _, y := range f(i, n-1) {
				ans = append(ans, "("+x+", "+y+")")
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function ambiguousCoordinates(s: string): string[] {
    s = s.slice(1, s.length - 1);
    const n = s.length;
    const dfs = (s: string) => {
        const res: string[] = [];
        for (let i = 1; i < s.length; i++) {
            const t = `${s.slice(0, i)}.${s.slice(i)}`;
            if (`${Number(t)}` === t) {
                res.push(t);
            }
        }
        if (`${Number(s)}` === s) {
            res.push(s);
        }
        return res;
    };
    const ans: string[] = [];
    for (let i = 1; i < n; i++) {
        for (const left of dfs(s.slice(0, i))) {
            for (const right of dfs(s.slice(i))) {
                ans.push(`(${left}, ${right})`);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
