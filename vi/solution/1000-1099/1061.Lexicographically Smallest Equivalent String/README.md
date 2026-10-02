---
comments: true
difficulty: Medium
tags:
    - Union Find
    - String
---

<!-- problem:start -->

# [1061. Lexicographically Smallest Equivalent String](https://leetcode.com/problems/lexicographically-smallest-equivalent-string)

[中文文档](/solution/1000-1099/1061.Lexicographically%20Smallest%20Equivalent%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s1</code> và <code>s2</code> có cùng độ dài, cùng với chuỗi <code>baseStr</code>.</p>

<p>Ta gọi <code>s1[i]</code> và <code>s2[i]</code> là hai ký tự tương đương.</p>

<ul>
	<li>Ví dụ, nếu <code>s1 = &quot;abc&quot;</code> và <code>s2 = &quot;cde&quot;</code>, thì ta có <code>&#39;a&#39; == &#39;c&#39;</code>, <code>&#39;b&#39; == &#39;d&#39;</code> và <code>&#39;c&#39; == &#39;e&#39;</code>.</li>
</ul>

<p>Các ký tự tương đương tuân theo những tính chất thông thường của quan hệ tương đương:</p>

<ul>
	<li><strong>Tính phản xạ:</strong> <code>&#39;a&#39; == &#39;a&#39;</code>.</li>
	<li><strong>Tính đối xứng:</strong> <code>&#39;a&#39; == &#39;b&#39;</code> suy ra <code>&#39;b&#39; == &#39;a&#39;</code>.</li>
	<li><strong>Tính bắc cầu:</strong> <code>&#39;a&#39; == &#39;b&#39;</code> và <code>&#39;b&#39; == &#39;c&#39;</code> suy ra <code>&#39;a&#39; == &#39;c&#39;</code>.</li>
</ul>

<p>Ví dụ, dựa trên thông tin tương đương từ <code>s1 = &quot;abc&quot;</code> và <code>s2 = &quot;cde&quot;</code>, <code>&quot;acd&quot;</code> và <code>&quot;aab&quot;</code> đều là chuỗi tương đương với <code>baseStr = &quot;eed&quot;</code>, trong đó <code>&quot;aab&quot;</code> là chuỗi tương đương nhỏ nhất theo thứ tự từ điển.</p>

<p>Dựa trên thông tin tương đương từ <code>s1</code> và <code>s2</code>, hãy trả về <em>chuỗi tương đương nhỏ nhất theo thứ tự từ điển của </em><code>baseStr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;parker&quot;, s2 = &quot;morris&quot;, baseStr = &quot;parser&quot;
<strong>Output:</strong> &quot;makkek&quot;
<strong>Giải thích:</strong> Dựa trên thông tin tương đương trong s1 và s2, ta có thể nhóm các ký tự thành [m,p], [a,o], [k,r,s], [e,i].
Các ký tự trong mỗi nhóm tương đương nhau và được sắp xếp theo thứ tự từ điển.
Vì vậy, đáp án là &quot;makkek&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;hello&quot;, s2 = &quot;world&quot;, baseStr = &quot;hold&quot;
<strong>Output:</strong> &quot;hdld&quot;
<strong>Giải thích: </strong>Dựa trên thông tin tương đương trong s1 và s2, ta có thể nhóm các ký tự thành [h,w], [d,e,o], [l,r].
Do đó, chỉ ký tự thứ hai &#39;o&#39; trong baseStr được đổi thành &#39;d&#39;, và đáp án là &quot;hdld&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;leetcode&quot;, s2 = &quot;programs&quot;, baseStr = &quot;sourcecode&quot;
<strong>Output:</strong> &quot;aauaaaaada&quot;
<strong>Giải thích:</strong> Ta nhóm các ký tự tương đương trong s1 và s2 thành [a,o,e,r,s,c], [l,p], [g,t] và [d,m]. Do đó, tất cả ký tự trong baseStr ngoại trừ &#39;u&#39; và &#39;d&#39; đều được đổi thành &#39;a&#39;, và đáp án là &quot;aauaaaaada&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length, baseStr &lt;= 1000</code></li>
	<li><code>s1.length == s2.length</code></li>
	<li><code>s1</code>, <code>s2</code> và <code>baseStr</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union Find

<!-- thinking:start -->

> **Tư duy**
>
> Quan hệ tương đương có tính bắc cầu. Mỗi ký tự trong $baseStr$ cần được đổi thành ký tự nhỏ nhất trong nhóm tương đương của nó. Chỉ có 26 ký tự nên có thể dùng cấu trúc disjoint-set union.
>
> Khi hợp nhất $s1[i]$ và $s2[i]$, ta nối root lớn hơn vào root nhỏ hơn để đại diện của nhóm luôn là ký tự nhỏ nhất theo thứ tự từ điển.
>
> Thay mỗi ký tự trong $baseStr$ bằng root của nó.

<!-- thinking:end -->

Ta có thể dùng Union Find (Disjoint Set Union, DSU) để xử lý quan hệ tương đương giữa các ký tự. Xem mỗi ký tự là một node và mỗi quan hệ tương đương là một cạnh nối hai node. Union Find giúp nhóm các ký tự tương đương và nhanh chóng tìm phần tử đại diện của mỗi ký tự khi truy vấn. Khi thực hiện thao tác union, ta luôn chọn ký tự nhỏ nhất theo thứ tự từ điển làm đại diện. Nhờ đó, chuỗi cuối cùng là chuỗi tương đương nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O((n + m) \times \log |\Sigma|)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài của $s1$ và $s2$, $m$ là độ dài của $baseStr$, còn $|\Sigma|$ là kích thước bảng ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestEquivalentString(self, s1: str, s2: str, baseStr: str) -> str:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(26))
        for a, b in zip(s1, s2):
            x, y = ord(a) - ord("a"), ord(b) - ord("a")
            px, py = find(x), find(y)
            if px < py:
                p[py] = px
            else:
                p[px] = py
        return "".join(chr(find(ord(c) - ord("a")) + ord("a")) for c in baseStr)
```

#### Java

```java
class Solution {
    private final int[] p = new int[26];

    public String smallestEquivalentString(String s1, String s2, String baseStr) {
        for (int i = 0; i < p.length; ++i) {
            p[i] = i;
        }
        for (int i = 0; i < s1.length(); ++i) {
            int x = s1.charAt(i) - 'a';
            int y = s2.charAt(i) - 'a';
            int px = find(x), py = find(y);
            if (px < py) {
                p[py] = px;
            } else {
                p[px] = py;
            }
        }
        char[] s = baseStr.toCharArray();
        for (int i = 0; i < s.length; ++i) {
            s[i] = (char) ('a' + find(s[i] - 'a'));
        }
        return String.valueOf(s);
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestEquivalentString(string s1, string s2, string baseStr) {
        vector<int> p(26);
        iota(p.begin(), p.end(), 0);
        auto find = [&](this auto&& find, int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (int i = 0; i < s1.length(); ++i) {
            int x = s1[i] - 'a';
            int y = s2[i] - 'a';
            int px = find(x), py = find(y);
            if (px < py) {
                p[py] = px;
            } else {
                p[px] = py;
            }
        }
        string s;
        for (char c : baseStr) {
            s.push_back('a' + find(c - 'a'));
        }
        return s;
    }
};
```

#### Go

```go
func smallestEquivalentString(s1 string, s2 string, baseStr string) string {
	p := make([]int, 26)
	for i := 0; i < 26; i++ {
		p[i] = i
	}

	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}

	for i := 0; i < len(s1); i++ {
		x := int(s1[i] - 'a')
		y := int(s2[i] - 'a')
		px := find(x)
		py := find(y)
		if px < py {
			p[py] = px
		} else {
			p[px] = py
		}
	}

	var s []byte
	for i := 0; i < len(baseStr); i++ {
		s = append(s, byte('a'+find(int(baseStr[i]-'a'))))
	}

	return string(s)
}
```

#### TypeScript

```ts
function smallestEquivalentString(s1: string, s2: string, baseStr: string): string {
    const p: number[] = Array.from({ length: 26 }, (_, i) => i);

    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };

    for (let i = 0; i < s1.length; i++) {
        const x = s1.charCodeAt(i) - 'a'.charCodeAt(0);
        const y = s2.charCodeAt(i) - 'a'.charCodeAt(0);
        const px = find(x);
        const py = find(y);
        if (px < py) {
            p[py] = px;
        } else {
            p[px] = py;
        }
    }

    const s: string[] = [];
    for (let i = 0; i < baseStr.length; i++) {
        const c = baseStr.charCodeAt(i) - 'a'.charCodeAt(0);
        s.push(String.fromCharCode('a'.charCodeAt(0) + find(c)));
    }
    return s.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_equivalent_string(s1: String, s2: String, base_str: String) -> String {
        fn find(x: usize, p: &mut Vec<usize>) -> usize {
            if p[x] != x {
                p[x] = find(p[x], p);
            }
            p[x]
        }

        let mut p = (0..26).collect::<Vec<_>>();
        for (a, b) in s1.bytes().zip(s2.bytes()) {
            let x = (a - b'a') as usize;
            let y = (b - b'a') as usize;
            let px = find(x, &mut p);
            let py = find(y, &mut p);
            if px < py {
                p[py] = px;
            } else {
                p[px] = py;
            }
        }

        base_str
            .bytes()
            .map(|c| (b'a' + find((c - b'a') as usize, &mut p) as u8) as char)
            .collect()
    }
}
```

#### C#

```cs
public class Solution {
    public string SmallestEquivalentString(string s1, string s2, string baseStr) {
        int[] p = new int[26];
        for (int i = 0; i < 26; i++) {
            p[i] = i;
        }

        int Find(int x) {
            if (p[x] != x) {
                p[x] = Find(p[x]);
            }
            return p[x];
        }

        for (int i = 0; i < s1.Length; i++) {
            int x = s1[i] - 'a';
            int y = s2[i] - 'a';
            int px = Find(x);
            int py = Find(y);
            if (px < py) {
                p[py] = px;
            } else {
                p[px] = py;
            }
        }

        var res = new System.Text.StringBuilder();
        foreach (char c in baseStr) {
            int idx = Find(c - 'a');
            res.Append((char)(idx + 'a'));
        }

        return res.ToString();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
