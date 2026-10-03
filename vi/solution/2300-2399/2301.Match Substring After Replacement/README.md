---
comments: true
difficulty: Hard
rating: 1860
source: Biweekly Contest 80 Q3
tags:
    - Array
    - Hash Table
    - String
    - String Matching
---

<!-- problem:start -->

# [2301. Match Substring After Replacement](https://leetcode.com/problems/match-substring-after-replacement)

[中文文档](/solution/2300-2399/2301.Match%20Substring%20After%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>sub</code>. Bạn cũng được cho một mảng ký tự 2D <code>mappings</code>, trong đó <code>mappings[i] = [old<sub>i</sub>, new<sub>i</sub>]</code> cho biết bạn có thể thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li><strong>Thay thế</strong> một ký tự <code>old<sub>i</sub></code> trong <code>sub</code> bằng <code>new<sub>i</sub></code>.</li>
</ul>

<p>Mỗi ký tự trong <code>sub</code> <strong>không thể</strong> được thay thế quá một lần.</p>

<p>Trả về <code>true</code><em> nếu có thể biến </em><code>sub</code><em> thành một chuỗi con của </em><code>s</code><em> bằng cách thay thế không hoặc nhiều ký tự theo </em><code>mappings</code>. Nếu không, trả về <code>false</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp, không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;fool3e7bar&quot;, sub = &quot;leet&quot;, mappings = [[&quot;e&quot;,&quot;3&quot;],[&quot;t&quot;,&quot;7&quot;],[&quot;t&quot;,&quot;8&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Thay thế chữ &#39;e&#39; đầu tiên trong sub bằng &#39;3&#39; và chữ &#39;t&#39; trong sub bằng &#39;7&#39;.
Bây giờ sub = &quot;l3e7&quot; là một chuỗi con của s, nên ta trả về true.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;fooleetbar&quot;, sub = &quot;f00l&quot;, mappings = [[&quot;o&quot;,&quot;0&quot;]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Chuỗi &quot;f00l&quot; không phải là chuỗi con của s và không thể thực hiện phép thay thế nào.
Lưu ý rằng ta không thể thay thế &#39;0&#39; bằng &#39;o&#39;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Fool33tbaR&quot;, sub = &quot;leetd&quot;, mappings = [[&quot;e&quot;,&quot;3&quot;],[&quot;t&quot;,&quot;7&quot;],[&quot;t&quot;,&quot;8&quot;],[&quot;d&quot;,&quot;b&quot;],[&quot;p&quot;,&quot;b&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Thay thế chữ &#39;e&#39; thứ nhất và thứ hai trong sub bằng &#39;3&#39;, và chữ &#39;d&#39; trong sub bằng &#39;b&#39;.
Bây giờ sub = &quot;l33tb&quot; là một chuỗi con của s, nên ta trả về true.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sub.length &lt;= s.length &lt;= 5000</code></li>
	<li><code>0 &lt;= mappings.length &lt;= 1000</code></li>
	<li><code>mappings[i].length == 2</code></li>
	<li><code>old<sub>i</sub> != new<sub>i</sub></code></li>
	<li><code>s</code> và <code>sub</code> chỉ gồm chữ cái tiếng Anh viết hoa, viết thường và chữ số.</li>
	<li><code>old<sub>i</sub></code> và <code>new<sub>i</sub></code> là chữ cái tiếng Anh viết hoa, viết thường hoặc chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi mapping chỉ là một phép thay thế đơn lẻ, vì vậy ta chỉ cần thử mọi cách căn chỉnh của $sub$ bên trong $s$ và kiểm tra từng ký tự. Vì $|s| \le 5000$, tích của số cách căn chỉnh và độ dài chuỗi con vào khoảng $2.5 \times 10^7$, hoàn toàn có thể chấp nhận được.
>
> Một ký tự có thể ánh xạ tới nhiều phép thay thế; quét danh sách mapping tại mỗi vị trí sẽ lặp lại công việc. Ta lưu tập các ký tự có thể đạt được của mỗi ký tự gốc trong một hash table, rồi kiểm tra “bằng nhau hoặc có thể thay thế” trong $O(1)$ cho mỗi cặp.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table $d$ để ghi lại tập các ký tự mà mỗi ký tự có thể được thay thế thành.

Sau đó, ta liệt kê tất cả chuỗi con có độ dài $sub$ trong $s$, rồi kiểm tra xem có thể thu được chuỗi $sub$ bằng phép thay thế hay không. Nếu có, trả về `true`; nếu không, tiếp tục liệt kê chuỗi con tiếp theo.

Sau khi liệt kê hết, điều đó có nghĩa là không thể thu được $sub$ bằng cách thay thế bất kỳ chuỗi con nào trong $s$, nên ta trả về `false`.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(C^2)$. Trong đó, $m$ và $n$ lần lượt là độ dài của các chuỗi $s$ và $sub$, còn $C$ là kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matchReplacement(self, s: str, sub: str, mappings: List[List[str]]) -> bool:
        d = defaultdict(set)
        for a, b in mappings:
            d[a].add(b)
        for i in range(len(s) - len(sub) + 1):
            if all(a == b or a in d[b] for a, b in zip(s[i : i + len(sub)], sub)):
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean matchReplacement(String s, String sub, char[][] mappings) {
        Map<Character, Set<Character>> d = new HashMap<>();
        for (var e : mappings) {
            d.computeIfAbsent(e[0], k -> new HashSet<>()).add(e[1]);
        }
        int m = s.length(), n = sub.length();
        for (int i = 0; i < m - n + 1; ++i) {
            boolean ok = true;
            for (int j = 0; j < n && ok; ++j) {
                char a = s.charAt(i + j), b = sub.charAt(j);
                if (a != b && !d.getOrDefault(b, Collections.emptySet()).contains(a)) {
                    ok = false;
                }
            }
            if (ok) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool matchReplacement(string s, string sub, vector<vector<char>>& mappings) {
        unordered_map<char, unordered_set<char>> d;
        for (auto& e : mappings) {
            d[e[0]].insert(e[1]);
        }
        int m = s.size(), n = sub.size();
        for (int i = 0; i < m - n + 1; ++i) {
            bool ok = true;
            for (int j = 0; j < n && ok; ++j) {
                char a = s[i + j], b = sub[j];
                if (a != b && !d[b].count(a)) {
                    ok = false;
                }
            }
            if (ok) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func matchReplacement(s string, sub string, mappings [][]byte) bool {
	d := map[byte]map[byte]bool{}
	for _, e := range mappings {
		if d[e[0]] == nil {
			d[e[0]] = map[byte]bool{}
		}
		d[e[0]][e[1]] = true
	}
	for i := 0; i < len(s)-len(sub)+1; i++ {
		ok := true
		for j := 0; j < len(sub) && ok; j++ {
			a, b := s[i+j], sub[j]
			if a != b && !d[b][a] {
				ok = false
			}
		}
		if ok {
			return true
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mảng + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu mapping trong các hash set, nên hằng số thời gian phụ thuộc vào hashing. Tập ký tự chỉ gồm chữ cái và chữ số, vì vậy một bảng boolean $128 \times 128$ có thể ghi lại khả năng chuyển đổi và biến mỗi truy vấn thành một lần tra cứu theo chỉ số. Thứ tự căn chỉnh không thay đổi.

<!-- thinking:end -->

Vì tập ký tự chỉ chứa chữ cái tiếng Anh viết hoa, viết thường và chữ số, ta có thể dùng trực tiếp một mảng $128 \times 128$ là $d$ để ghi lại tập các ký tự mà mỗi ký tự có thể được thay thế thành.

Độ phức tạp thời gian là $O(m \times n)$, và độ phức tạp không gian là $O(C^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matchReplacement(self, s: str, sub: str, mappings: List[List[str]]) -> bool:
        d = [[False] * 128 for _ in range(128)]
        for a, b in mappings:
            d[ord(a)][ord(b)] = True
        for i in range(len(s) - len(sub) + 1):
            if all(
                a == b or d[ord(b)][ord(a)] for a, b in zip(s[i : i + len(sub)], sub)
            ):
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean matchReplacement(String s, String sub, char[][] mappings) {
        boolean[][] d = new boolean[128][128];
        for (var e : mappings) {
            d[e[0]][e[1]] = true;
        }
        int m = s.length(), n = sub.length();
        for (int i = 0; i < m - n + 1; ++i) {
            boolean ok = true;
            for (int j = 0; j < n && ok; ++j) {
                char a = s.charAt(i + j), b = sub.charAt(j);
                if (a != b && !d[b][a]) {
                    ok = false;
                }
            }
            if (ok) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool matchReplacement(string s, string sub, vector<vector<char>>& mappings) {
        bool d[128][128]{};
        for (auto& e : mappings) {
            d[e[0]][e[1]] = true;
        }
        int m = s.size(), n = sub.size();
        for (int i = 0; i < m - n + 1; ++i) {
            bool ok = true;
            for (int j = 0; j < n && ok; ++j) {
                char a = s[i + j], b = sub[j];
                if (a != b && !d[b][a]) {
                    ok = false;
                }
            }
            if (ok) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func matchReplacement(s string, sub string, mappings [][]byte) bool {
	d := [128][128]bool{}
	for _, e := range mappings {
		d[e[0]][e[1]] = true
	}
	for i := 0; i < len(s)-len(sub)+1; i++ {
		ok := true
		for j := 0; j < len(sub) && ok; j++ {
			a, b := s[i+j], sub[j]
			if a != b && !d[b][a] {
				ok = false
			}
		}
		if ok {
			return true
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
