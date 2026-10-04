---
comments: true
difficulty: Medium
rating: 1735
source: Biweekly Contest 105 Q2
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2707. Extra Characters in a String](https://leetcode.com/problems/extra-characters-in-a-string)

[Tài liệu tiếng Trung](/solution/2700-2799/2707.Extra%20Characters%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có chỉ số bắt đầu từ <strong>0</strong> và một từ điển gồm các từ <code>dictionary</code>. Bạn cần chia <code>s</code> thành một hoặc nhiều chuỗi con <strong>không chồng lấn</strong>, sao cho mỗi chuỗi con đều xuất hiện trong <code>dictionary</code>. Có thể có một số <strong>ký tự thừa</strong> trong <code>s</code> không xuất hiện trong bất kỳ chuỗi con nào.</p>

<p>Trả về <em>số lượng <strong>tối thiểu</strong> ký tự thừa còn lại nếu chia </em><code>s</code><em> một cách tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetscode&quot;, dictionary = [&quot;leet&quot;,&quot;code&quot;,&quot;leetcode&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chia s thành hai chuỗi con: &quot;leet&quot; từ chỉ số 0 đến 3 và &quot;code&quot; từ chỉ số 5 đến 8. Chỉ có 1 ký tự không được sử dụng (ở chỉ số 4), nên ta trả về 1.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;sayhelloworld&quot;, dictionary = [&quot;hello&quot;,&quot;world&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chia s thành hai chuỗi con: &quot;hello&quot; từ chỉ số 3 đến 7 và &quot;world&quot; từ chỉ số 8 đến 12. Các ký tự ở chỉ số 0, 1, 2 không được sử dụng trong bất kỳ chuỗi con nào, nên được xem là ký tự thừa. Vì vậy, ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>1 &lt;= dictionary.length &lt;= 50</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 50</code></li>
	<li><code>dictionary[i]</code>&nbsp;và <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường</li>
	<li><code>dictionary</code> chứa các từ khác nhau</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta muốn phân hoạch $s$ sao cho bao phủ được nhiều từ trong từ điển nhất có thể. Việc liệt kê các phân hoạch vẫn chấp nhận được với $n\le 50$, nhưng các tiền tố giống nhau sẽ bị tính lại.
>
> Số ký tự thừa ít nhất trong $i$ chữ cái đầu tiên chỉ phụ thuộc vào các tiền tố ngắn hơn: xem $s[i-1]$ là ký tự thừa, hoặc chuyển từ một $j$ nào đó mà lát cắt $s[j..i)$ có trong từ điển. Một hash set cho phép kiểm tra sự tồn tại trong $O(1)$, và ta tính $f[i]$ theo thứ tự tăng dần.

<!-- thinking:end -->

Ta có thể sử dụng một bảng băm $ss$ để lưu tất cả các từ trong từ điển, giúp nhanh chóng xác định một chuỗi có nằm trong từ điển hay không.

Tiếp theo, ta định nghĩa $f[i]$ là số lượng ký tự thừa tối thiểu trong $i$ ký tự đầu tiên của chuỗi $s$, ban đầu $f[0] = 0$.

Khi $i \ge 1$, ký tự thứ $i$ là $s[i - 1]$ có thể là một ký tự thừa, khi đó $f[i] = f[i - 1] + 1$. Nếu tồn tại một chỉ số $j \in [0, i - 1]$ sao cho $s[j..i)$ nằm trong bảng băm $ss$, ta có thể chọn $s[j..i)$ làm một từ, khi đó $f[i] = f[j]$.

Tóm lại, ta có công thức chuyển trạng thái:

$$
f[i] = \min \{ f[i - 1] + 1, \min_{j \in [0, i - 1]} f[j] \}
$$

trong đó $i \ge 1$, và $j \in [0, i - 1]$, đồng thời $s[j..i)$ nằm trong bảng băm $ss$.

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n^3 + L)$, và độ phức tạp không gian là $O(n + L)$. Ở đây, $n$ là độ dài chuỗi $s$, còn $L$ là tổng độ dài của tất cả các từ trong từ điển.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minExtraChar(self, s: str, dictionary: List[str]) -> int:
        ss = set(dictionary)
        n = len(s)
        f = [0] * (n + 1)
        for i in range(1, n + 1):
            f[i] = f[i - 1] + 1
            for j in range(i):
                if s[j:i] in ss and f[j] < f[i]:
                    f[i] = f[j]
        return f[n]
```

#### Java

```java
class Solution {
    public int minExtraChar(String s, String[] dictionary) {
        Set<String> ss = new HashSet<>();
        for (String w : dictionary) {
            ss.add(w);
        }
        int n = s.length();
        int[] f = new int[n + 1];
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            f[i] = f[i - 1] + 1;
            for (int j = 0; j < i; ++j) {
                if (ss.contains(s.substring(j, i))) {
                    f[i] = Math.min(f[i], f[j]);
                }
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minExtraChar(string s, vector<string>& dictionary) {
        unordered_set<string> ss(dictionary.begin(), dictionary.end());
        int n = s.size();
        int f[n + 1];
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            f[i] = f[i - 1] + 1;
            for (int j = 0; j < i; ++j) {
                if (ss.count(s.substr(j, i - j))) {
                    f[i] = min(f[i], f[j]);
                }
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func minExtraChar(s string, dictionary []string) int {
	ss := map[string]bool{}
	for _, w := range dictionary {
		ss[w] = true
	}
	n := len(s)
	f := make([]int, n+1)
	for i := 1; i <= n; i++ {
		f[i] = f[i-1] + 1
		for j := 0; j < i; j++ {
			if ss[s[j:i]] && f[j] < f[i] {
				f[i] = f[j]
			}
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function minExtraChar(s: string, dictionary: string[]): number {
    const ss = new Set(dictionary);
    const n = s.length;
    const f = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        f[i] = f[i - 1] + 1;
        for (let j = 0; j < i; ++j) {
            if (ss.has(s.substring(j, i))) {
                f[i] = Math.min(f[i], f[j]);
            }
        }
    }
    return f[n];
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn min_extra_char(s: String, dictionary: Vec<String>) -> i32 {
        let ss: HashSet<String> = dictionary.into_iter().collect();
        let n = s.len();
        let mut f = vec![0; n + 1];
        for i in 1..=n {
            f[i] = f[i - 1] + 1;
            for j in 0..i {
                if ss.contains(&s[j..i]) {
                    f[i] = f[i].min(f[j]);
                }
            }
        }
        f[n]
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string[]} dictionary
 * @return {number}
 */
var minExtraChar = function (s, dictionary) {
    const ss = new Set(dictionary);
    const n = s.length;
    const f = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        f[i] = f[i - 1] + 1;
        for (let j = 0; j < i; ++j) {
            if (ss.has(s.slice(j, i))) {
                f[i] = Math.min(f[i], f[j]);
            }
        }
    }
    return f[n];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Trie + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt mọi $j$ với mỗi $i$ và tạo các lát cắt, khiến thời gian tăng lên bậc ba. Việc chèn các từ đảo ngược vào một trie cho phép ta đi từ $i-1$ về bên trái và dừng ngay khi gặp ký tự không khớp, loại bỏ việc tạo lát cắt và kết hợp thao tác tra cứu vào các cạnh.

<!-- thinking:end -->

Ta có thể sử dụng trie để tối ưu độ phức tạp thời gian của Lời giải 1.

Cụ thể, trước tiên ta chèn mỗi từ trong từ điển vào trie $root$ theo thứ tự ngược, sau đó định nghĩa $f[i]$ là số lượng ký tự thừa tối thiểu trong $i$ ký tự đầu tiên của chuỗi $s$, ban đầu $f[0] = 0$.

Khi $i \ge 1$, ký tự thứ $i$ là $s[i - 1]$ có thể là một ký tự thừa, khi đó $f[i] = f[i - 1] + 1$. Ta cũng có thể duyệt chỉ số $j$ theo thứ tự ngược trong khoảng $[0..i-1]$, và xác định xem $s[j..i)$ có nằm trong trie $root$ hay không. Nếu có, ta có thể chọn $s[j..i)$ làm một từ, khi đó $f[i] = f[j]$.

Độ phức tạp thời gian là $O(n^2 + L)$, và độ phức tạp không gian là $O(n + L \times |\Sigma|)$. Ở đây, $n$ là độ dài chuỗi $s$, còn $L$ là tổng độ dài của tất cả các từ trong từ điển. Ngoài ra, $|\Sigma|$ là kích thước của tập ký tự. Trong bài này, tập ký tự là các chữ cái tiếng Anh viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ['children', 'is_end']

    def __init__(self):
        self.children: List[Node | None] = [None] * 26
        self.is_end = False


class Solution:
    def minExtraChar(self, s: str, dictionary: List[str]) -> int:
        root = Node()
        for w in dictionary:
            node = root
            for c in w[::-1]:
                i = ord(c) - ord('a')
                if node.children[i] is None:
                    node.children[i] = Node()
                node = node.children[i]
            node.is_end = True

        n = len(s)
        f = [0] * (n + 1)
        for i in range(1, n + 1):
            f[i] = f[i - 1] + 1
            node = root
            for j in range(i - 1, -1, -1):
                node = node.children[ord(s[j]) - ord('a')]
                if node is None:
                    break
                if node.is_end and f[j] < f[i]:
                    f[i] = f[j]
        return f[n]
```

#### Java

```java
class Node {
    Node[] children = new Node[26];
    boolean isEnd;
}

class Solution {
    public int minExtraChar(String s, String[] dictionary) {
        Node root = new Node();
        for (String w : dictionary) {
            Node node = root;
            for (int k = w.length() - 1; k >= 0; --k) {
                int i = w.charAt(k) - 'a';
                if (node.children[i] == null) {
                    node.children[i] = new Node();
                }
                node = node.children[i];
            }
            node.isEnd = true;
        }
        int n = s.length();
        int[] f = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            f[i] = f[i - 1] + 1;
            Node node = root;
            for (int j = i - 1; j >= 0; --j) {
                node = node.children[s.charAt(j) - 'a'];
                if (node == null) {
                    break;
                }
                if (node.isEnd && f[j] < f[i]) {
                    f[i] = f[j];
                }
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Node {
public:
    Node* children[26];
    bool isEnd = false;
    Node() {
        fill(children, children + 26, nullptr);
    }
};

class Solution {
public:
    int minExtraChar(string s, vector<string>& dictionary) {
        Node* root = new Node();
        for (const string& w : dictionary) {
            Node* node = root;
            for (int k = w.length() - 1; k >= 0; --k) {
                int i = w[k] - 'a';
                if (node->children[i] == nullptr) {
                    node->children[i] = new Node();
                }
                node = node->children[i];
            }
            node->isEnd = true;
        }

        int n = s.size();
        int f[n + 1];
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            f[i] = f[i - 1] + 1;
            Node* node = root;
            for (int j = i - 1; ~j; --j) {
                node = node->children[s[j] - 'a'];
                if (node == nullptr) {
                    break;
                }
                if (node->isEnd && f[j] < f[i]) {
                    f[i] = f[j];
                }
            }
        }
        return f[n];
    }
};
```

#### Go

```go
type Node struct {
	children [26]*Node
	isEnd    bool
}

func minExtraChar(s string, dictionary []string) int {
	root := &Node{}
	for _, w := range dictionary {
		node := root
		for k := len(w) - 1; k >= 0; k-- {
			i := int(w[k] - 'a')
			if node.children[i] == nil {
				node.children[i] = &Node{}
			}
			node = node.children[i]
		}
		node.isEnd = true
	}

	n := len(s)
	f := make([]int, n+1)
	for i := 1; i <= n; i++ {
		f[i] = f[i-1] + 1
		node := root
		for j := i - 1; j >= 0; j-- {
			node = node.children[int(s[j]-'a')]
			if node == nil {
				break
			}
			if node.isEnd && f[j] < f[i] {
				f[i] = f[j]
			}
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
class Node {
    children: (Node | null)[] = Array(26).fill(null);
    isEnd: boolean = false;
}

function minExtraChar(s: string, dictionary: string[]): number {
    const root = new Node();
    for (const w of dictionary) {
        let node = root;
        for (let k = w.length - 1; ~k; --k) {
            const i = w.charCodeAt(k) - 'a'.charCodeAt(0);
            if (node.children[i] === null) {
                node.children[i] = new Node();
            }
            node = node.children[i] as Node;
        }
        node.isEnd = true;
    }

    const n = s.length;
    const f: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        f[i] = f[i - 1] + 1;
        let node = root;
        for (let j = i - 1; ~j; --j) {
            node = (node.children[s.charCodeAt(j) - 'a'.charCodeAt(0)] as Node) || null;
            if (node === null) {
                break;
            }
            if (node.isEnd && f[j] < f[i]) {
                f[i] = f[j];
            }
        }
    }

    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
