---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [288. Unique Word Abbreviation 🔒](https://leetcode.com/problems/unique-word-abbreviation)

[中文文档](/solution/0200-0299/0288.Unique%20Word%20Abbreviation/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Dạng viết tắt</strong> của một từ được tạo bằng cách ghép chữ cái đầu, số ký tự nằm giữa chữ cái đầu và cuối, rồi đến chữ cái cuối. Nếu từ chỉ có hai ký tự thì dạng viết tắt của nó chính là từ đó.</p>

<p>Ví dụ:</p>

<ul>
	<li><code>dog --&gt; d1g</code> vì có một chữ cái nằm giữa chữ cái đầu <code>&#39;d&#39;</code> và chữ cái cuối <code>&#39;g&#39;</code>.</li>
	<li><code>internationalization --&gt; i18n</code> vì có 18 chữ cái nằm giữa chữ cái đầu <code>&#39;i&#39;</code> và chữ cái cuối <code>&#39;n&#39;</code>.</li>
	<li><code>it --&gt; it</code> vì mọi từ chỉ có hai ký tự đều là dạng viết tắt của chính nó.</li>
</ul>

<p>Hãy triển khai class <code>ValidWordAbbr</code>:</p>

<ul>
	<li><code>ValidWordAbbr(String[] dictionary)</code> Khởi tạo object với <code>dictionary</code> gồm các từ.</li>
	<li><code>boolean isUnique(string word)</code> Trả về <code>true</code> nếu thỏa mãn <strong>một trong hai</strong> điều kiện sau (nếu không thì trả về <code>false</code>):
	<ul>
		<li>Trong <code>dictionary</code> không có từ nào có <strong>dạng viết tắt</strong> trùng với <strong>dạng viết tắt</strong> của <code>word</code>.</li>
		<li>Với mọi từ trong <code>dictionary</code> có <strong>dạng viết tắt</strong> trùng với <strong>dạng viết tắt</strong> của <code>word</code>, từ đó phải <strong>chính là</strong> <code>word</code>.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;ValidWordAbbr&quot;, &quot;isUnique&quot;, &quot;isUnique&quot;, &quot;isUnique&quot;, &quot;isUnique&quot;, &quot;isUnique&quot;]
[[[&quot;deer&quot;, &quot;door&quot;, &quot;cake&quot;, &quot;card&quot;]], [&quot;dear&quot;], [&quot;cart&quot;], [&quot;cane&quot;], [&quot;make&quot;], [&quot;cake&quot;]]
<strong>Đầu ra</strong>
[null, false, true, false, true, true]

<strong>Giải thích</strong>
ValidWordAbbr validWordAbbr = new ValidWordAbbr([&quot;deer&quot;, &quot;door&quot;, &quot;cake&quot;, &quot;card&quot;]);
validWordAbbr.isUnique(&quot;dear&quot;); // return false, dictionary word &quot;deer&quot; and word &quot;dear&quot; have the same abbreviation &quot;d2r&quot; but are not the same.
validWordAbbr.isUnique(&quot;cart&quot;); // return true, no words in the dictionary have the abbreviation &quot;c2t&quot;.
validWordAbbr.isUnique(&quot;cane&quot;); // return false, dictionary word &quot;cake&quot; and word &quot;cane&quot; have the same abbreviation  &quot;c2e&quot; but are not the same.
validWordAbbr.isUnique(&quot;make&quot;); // return true, no words in the dictionary have the abbreviation &quot;m2e&quot;.
validWordAbbr.isUnique(&quot;cake&quot;); // return true, because &quot;cake&quot; is already in the dictionary and no other word in the dictionary has &quot;c2e&quot; abbreviation.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= dictionary.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 20</code></li>
	<li><code>dictionary[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= word.length &lt;= 20</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Sẽ có tối đa <code>5000</code> lần gọi <code>isUnique</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Dạng viết tắt gồm chữ cái đầu, chữ cái cuối và số ký tự ở giữa. Khi truy vấn, ta dùng lại cấu trúc dữ liệu của dictionary: ánh xạ mỗi dạng viết tắt đến một set các từ tương ứng.
>
> Một từ được xem là duy nhất nếu chưa có dạng viết tắt của nó, hoặc set tương ứng chỉ chứa chính từ đó.

<!-- thinking:end -->

Theo mô tả bài toán, ta định nghĩa hàm $abbr(s)$ để tính dạng viết tắt của từ $s$. Nếu độ dài từ $s$ nhỏ hơn $3$ thì dạng viết tắt chính là từ đó; nếu không, dạng viết tắt gồm chữ cái đầu + (độ dài từ - 2) + chữ cái cuối.

Tiếp theo, ta định nghĩa hash table $d$, trong đó key là dạng viết tắt của từ, còn value là set chứa tất cả từ có dạng viết tắt tương ứng với key đó. Ta duyệt dictionary đã cho; với mỗi từ $s$, tính dạng viết tắt $abbr(s)$ rồi thêm $s$ vào $d[abbr(s)]$.

Để kiểm tra từ $word$ có thỏa mãn yêu cầu hay không, ta tính dạng viết tắt $abbr(word)$. Nếu $abbr(word)$ không có trong hash table $d$ thì $word$ thỏa mãn; nếu có, ta kiểm tra $d[abbr(word)]$ có đúng một phần tử hay không. Nếu chỉ có một phần tử và phần tử đó là $word$ thì $word$ thỏa mãn yêu cầu.

Độ phức tạp thời gian khởi tạo hash table là $O(n)$, với $n$ là số từ trong dictionary; thời gian kiểm tra một từ có thỏa điều kiện hay không là $O(1)$. Độ phức tạp không gian của hash table là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class ValidWordAbbr:
    def __init__(self, dictionary: List[str]):
        self.d = defaultdict(set)
        for s in dictionary:
            self.d[self.abbr(s)].add(s)

    def isUnique(self, word: str) -> bool:
        s = self.abbr(word)
        return s not in self.d or all(word == t for t in self.d[s])

    def abbr(self, s: str) -> str:
        return s if len(s) < 3 else s[0] + str(len(s) - 2) + s[-1]


# Your ValidWordAbbr object will be instantiated and called as such:
# obj = ValidWordAbbr(dictionary)
# param_1 = obj.isUnique(word)
```

#### Java

```java
class ValidWordAbbr {
    private Map<String, Set<String>> d = new HashMap<>();

    public ValidWordAbbr(String[] dictionary) {
        for (var s : dictionary) {
            d.computeIfAbsent(abbr(s), k -> new HashSet<>()).add(s);
        }
    }

    public boolean isUnique(String word) {
        var ws = d.get(abbr(word));
        return ws == null || (ws.size() == 1 && ws.contains(word));
    }

    private String abbr(String s) {
        int n = s.length();
        return n < 3 ? s : s.substring(0, 1) + (n - 2) + s.substring(n - 1);
    }
}

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * ValidWordAbbr obj = new ValidWordAbbr(dictionary);
 * boolean param_1 = obj.isUnique(word);
 */
```

#### C++

```cpp
class ValidWordAbbr {
public:
    ValidWordAbbr(vector<string>& dictionary) {
        for (auto& s : dictionary) {
            d[abbr(s)].insert(s);
        }
    }

    bool isUnique(string word) {
        string s = abbr(word);
        return !d.count(s) || (d[s].size() == 1 && d[s].count(word));
    }

private:
    unordered_map<string, unordered_set<string>> d;

    string abbr(string& s) {
        int n = s.size();
        return n < 3 ? s : s.substr(0, 1) + to_string(n - 2) + s.substr(n - 1, 1);
    }
};

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * ValidWordAbbr* obj = new ValidWordAbbr(dictionary);
 * bool param_1 = obj->isUnique(word);
 */
```

#### Go

```go
type ValidWordAbbr struct {
	d map[string]map[string]bool
}

func Constructor(dictionary []string) ValidWordAbbr {
	d := make(map[string]map[string]bool)
	for _, s := range dictionary {
		abbr := abbr(s)
		if _, ok := d[abbr]; !ok {
			d[abbr] = make(map[string]bool)
		}
		d[abbr][s] = true
	}
	return ValidWordAbbr{d}
}

func (this *ValidWordAbbr) IsUnique(word string) bool {
	ws := this.d[abbr(word)]
	return ws == nil || (len(ws) == 1 && ws[word])
}

func abbr(s string) string {
	n := len(s)
	if n < 3 {
		return s
	}
	return fmt.Sprintf("%c%d%c", s[0], n-2, s[n-1])
}

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * obj := Constructor(dictionary);
 * param_1 := obj.IsUnique(word);
 */
```

#### TypeScript

```ts
class ValidWordAbbr {
    private d: Map<string, Set<string>> = new Map();

    constructor(dictionary: string[]) {
        for (const s of dictionary) {
            const abbr = this.abbr(s);
            if (!this.d.has(abbr)) {
                this.d.set(abbr, new Set());
            }
            this.d.get(abbr)!.add(s);
        }
    }

    isUnique(word: string): boolean {
        const ws = this.d.get(this.abbr(word));
        return ws === undefined || (ws.size === 1 && ws.has(word));
    }

    abbr(s: string): string {
        const n = s.length;
        return n < 3 ? s : s[0] + (n - 2) + s[n - 1];
    }
}

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * var obj = new ValidWordAbbr(dictionary)
 * var param_1 = obj.isUnique(word)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
