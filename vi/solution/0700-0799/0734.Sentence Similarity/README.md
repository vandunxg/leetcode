---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [734. Sentence Similarity 🔒](https://leetcode.com/problems/sentence-similarity)

[中文文档](/solution/0700-0799/0734.Sentence%20Similarity/README.md)

## Mô tả

<!-- description:start -->

<p>Có thể biểu diễn một câu bằng mảng các từ. Ví dụ, câu <code>&quot;I am happy with leetcode&quot;</code> có thể được biểu diễn thành <code>arr = [&quot;I&quot;,&quot;am&quot;,happy&quot;,&quot;with&quot;,&quot;leetcode&quot;]</code>.</p>

<p>Cho hai câu <code>sentence1</code> và <code>sentence2</code>, mỗi câu được biểu diễn bằng một mảng chuỗi, cùng mảng các cặp chuỗi <code>similarPairs</code>, trong đó <code>similarPairs[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> cho biết hai từ <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> tương tự nhau.</p>

<p>Trả về <em><code>true</code> nếu <code>sentence1</code> và <code>sentence2</code> tương tự nhau, hoặc <code>false</code> nếu chúng không tương tự</em>.</p>

<p>Hai câu được xem là tương tự nếu:</p>

<ul>
	<li>Chúng có <strong>cùng độ dài</strong> (tức cùng số từ).</li>
	<li><code>sentence1[i]</code> và <code>sentence2[i]</code> chỉ gồm các chữ cái tiếng Anh.</li>
</ul>

<p>Lưu ý, một từ luôn tương tự với chính nó; ngoài ra, quan hệ tương tự không có tính bắc cầu. Ví dụ, nếu các từ <code>a</code> và <code>b</code> tương tự nhau, đồng thời <code>b</code> và <code>c</code> tương tự nhau, thì <code>a</code> và <code>c</code> <strong>không nhất thiết tương tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;great&quot;,&quot;acting&quot;,&quot;skills&quot;], sentence2 = [&quot;fine&quot;,&quot;drama&quot;,&quot;talent&quot;], similarPairs = [[&quot;great&quot;,&quot;fine&quot;],[&quot;drama&quot;,&quot;acting&quot;],[&quot;skills&quot;,&quot;talent&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai câu có cùng độ dài và mỗi từ ở vị trí i trong sentence1 cũng tương tự với từ tương ứng trong sentence2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;great&quot;], sentence2 = [&quot;great&quot;], similarPairs = []
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một từ luôn tương tự với chính nó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;great&quot;], sentence2 = [&quot;doubleplus&quot;,&quot;good&quot;], similarPairs = [[&quot;great&quot;,&quot;doubleplus&quot;]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Vì hai câu không có cùng độ dài nên trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence1.length, sentence2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= sentence1[i].length, sentence2[i].length &lt;= 20</code></li>
	<li><code>sentence1[i]</code> và <code>sentence2[i]</code> chỉ gồm các chữ cái tiếng Anh.</li>
	<li><code>0 &lt;= similarPairs.length &lt;= 1000</code></li>
	<li><code>similarPairs[i].length == 2</code></li>
	<li><code>1 &lt;= x<sub>i</sub>.length, y<sub>i</sub>.length &lt;= 20</code></li>
	<li><code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> chỉ gồm chữ cái tiếng Anh viết thường và viết hoa.</li>
	<li>Mọi cặp <code>(x<sub>i</sub>,<sub> </sub>y<sub>i</sub>)</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hai câu tương tự khi chúng có cùng độ dài và mỗi cặp từ ở vị trí tương ứng giống nhau hoặc có trong danh sách. Quan hệ tương tự không có tính bắc cầu, nên cần kiểm tra từng cặp nhanh.
>
> Lưu $\textit{similarPairs}$ trong một set. Vì các cặp không có thứ tự, hãy lưu cả hai chiều hoặc kiểm tra cả hai chiều.
>
> Trả về false nếu độ dài khác nhau; nếu không, duyệt đồng thời hai câu và trả về false nếu có cặp từ vừa không giống nhau vừa không được lưu.

<!-- thinking:end -->

Trước tiên, kiểm tra độ dài của $\textit{sentence1}$ và $\textit{sentence2}$. Nếu chúng không bằng nhau, trả về $\text{false}$.

Sau đó, dùng hash table $\textit{s}$ để lưu các cặp từ tương tự. Với mỗi cặp $[x, y]$ trong $\textit{similarPairs}$, thêm cả $x$ và $y$ vào hash table $\textit{s}$.

Tiếp theo, duyệt $\textit{sentence1}$ và $\textit{sentence2}$. Với mỗi vị trí $i$, nếu $\textit{sentence1}[i]$ khác $\textit{sentence2}[i]$, đồng thời $(\textit{sentence1}[i], \textit{sentence2}[i])$ và $(\textit{sentence2}[i], \textit{sentence1}[i])$ đều không có trong hash table $\textit{s}$, thì trả về $\text{false}$.

Nếu duyệt xong mà chưa trả về $\text{false}$, nghĩa là $\textit{sentence1}$ và $\textit{sentence2}$ tương tự nhau, nên trả về $\text{true}$.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả chuỗi trong bài toán.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areSentencesSimilar(
        self, sentence1: List[str], sentence2: List[str], similarPairs: List[List[str]]
    ) -> bool:
        if len(sentence1) != len(sentence2):
            return False
        s = {(x, y) for x, y in similarPairs}
        for x, y in zip(sentence1, sentence2):
            if x != y and (x, y) not in s and (y, x) not in s:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean areSentencesSimilar(
        String[] sentence1, String[] sentence2, List<List<String>> similarPairs) {
        if (sentence1.length != sentence2.length) {
            return false;
        }
        Set<List<String>> s = new HashSet<>();
        for (var p : similarPairs) {
            s.add(p);
        }
        for (int i = 0; i < sentence1.length; i++) {
            if (!sentence1[i].equals(sentence2[i])
                && !s.contains(List.of(sentence1[i], sentence2[i]))
                && !s.contains(List.of(sentence2[i], sentence1[i]))) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool areSentencesSimilar(vector<string>& sentence1, vector<string>& sentence2, vector<vector<string>>& similarPairs) {
        if (sentence1.size() != sentence2.size()) {
            return false;
        }
        unordered_set<string> s;
        for (const auto& p : similarPairs) {
            s.insert(p[0] + "#" + p[1]);
            s.insert(p[1] + "#" + p[0]);
        }
        for (int i = 0; i < sentence1.size(); ++i) {
            if (sentence1[i] != sentence2[i] && !s.contains(sentence1[i] + "#" + sentence2[i])) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func areSentencesSimilar(sentence1 []string, sentence2 []string, similarPairs [][]string) bool {
	if len(sentence1) != len(sentence2) {
		return false
	}
	s := map[string]bool{}
	for _, p := range similarPairs {
		s[p[0]+"#"+p[1]] = true
	}
	for i, x := range sentence1 {
		y := sentence2[i]
		if x != y && !s[x+"#"+y] && !s[y+"#"+x] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function areSentencesSimilar(
    sentence1: string[],
    sentence2: string[],
    similarPairs: string[][],
): boolean {
    if (sentence1.length !== sentence2.length) {
        return false;
    }
    const s = new Set<string>();
    for (const [x, y] of similarPairs) {
        s.add(x + '#' + y);
        s.add(y + '#' + x);
    }
    for (let i = 0; i < sentence1.length; i++) {
        if (sentence1[i] !== sentence2[i] && !s.has(sentence1[i] + '#' + sentence2[i])) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn are_sentences_similar(
        sentence1: Vec<String>,
        sentence2: Vec<String>,
        similar_pairs: Vec<Vec<String>>,
    ) -> bool {
        if sentence1.len() != sentence2.len() {
            return false;
        }

        let s: HashSet<(String, String)> = similar_pairs
            .into_iter()
            .map(|pair| (pair[0].clone(), pair[1].clone()))
            .collect();

        for (x, y) in sentence1.iter().zip(sentence2.iter()) {
            if x != y
                && !s.contains(&(x.clone(), y.clone()))
                && !s.contains(&(y.clone(), x.clone()))
            {
                return false;
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} sentence1
 * @param {string[]} sentence2
 * @param {string[][]} similarPairs
 * @return {boolean}
 */
var areSentencesSimilar = function (sentence1, sentence2, similarPairs) {
    if (sentence1.length !== sentence2.length) {
        return false;
    }
    const s = new Set();
    for (const [x, y] of similarPairs) {
        s.add(x + '#' + y);
        s.add(y + '#' + x);
    }
    for (let i = 0; i < sentence1.length; i++) {
        if (sentence1[i] !== sentence2[i] && !s.has(sentence1[i] + '#' + sentence2[i])) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
