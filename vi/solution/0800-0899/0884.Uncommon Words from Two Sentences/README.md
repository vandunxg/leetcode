---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [884. Uncommon Words from Two Sentences](https://leetcode.com/problems/uncommon-words-from-two-sentences)

[中文文档](/solution/0800-0899/0884.Uncommon%20Words%20from%20Two%20Sentences/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Câu</strong> là chuỗi gồm các từ được ngăn cách bằng một dấu cách, trong đó mỗi từ chỉ gồm các chữ cái viết thường.</p>

<p>Một từ được gọi là <strong>không phổ biến</strong> nếu nó xuất hiện đúng một lần trong một câu và <strong>không xuất hiện</strong> trong câu còn lại.</p>

<p>Cho hai <strong>câu</strong> <code>s1</code> và <code>s2</code>, hãy trả về <em>danh sách tất cả <strong>các từ không phổ biến</strong></em>. Có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;this apple is sweet&quot;, s2 = &quot;this apple is sour&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;sweet&quot;,&quot;sour&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Từ <code>&quot;sweet&quot;</code> chỉ xuất hiện trong <code>s1</code>, còn từ <code>&quot;sour&quot;</code> chỉ xuất hiện trong <code>s2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s1 = &quot;apple apple&quot;, s2 = &quot;banana&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;banana&quot;]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 200</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu cách.</li>
	<li><code>s1</code> và <code>s2</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Tất cả từ trong <code>s1</code> và <code>s2</code> được ngăn cách bằng đúng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một từ không phổ biến nếu tổng số lần xuất hiện của nó trong cả hai câu là một. Hai câu ngắn nên chỉ cần dùng một bộ đếm chung.
>
> Tách cả hai câu và đếm chung; các từ có số lần xuất hiện bằng $1$ chính là đáp án.

<!-- thinking:end -->

Theo mô tả bài toán, từ có tổng cộng đúng một lần xuất hiện trong cả hai câu sẽ thỏa mãn yêu cầu. Vì vậy, ta dùng hash table `cnt` để ghi nhận tất cả từ và số lần xuất hiện của chúng.

Sau đó, ta duyệt hash table và lấy ra tất cả các từ chỉ xuất hiện một lần.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của hai chuỗi `s1` và `s2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uncommonFromSentences(self, s1: str, s2: str) -> List[str]:
        cnt = Counter(s1.split()) + Counter(s2.split())
        return [s for s, v in cnt.items() if v == 1]
```

#### Java

```java
class Solution {
    public String[] uncommonFromSentences(String s1, String s2) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String s : s1.split(" ")) {
            cnt.merge(s, 1, Integer::sum);
        }
        for (String s : s2.split(" ")) {
            cnt.merge(s, 1, Integer::sum);
        }
        List<String> ans = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            if (e.getValue() == 1) {
                ans.add(e.getKey());
            }
        }
        return ans.toArray(new String[0]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int> cnt;
        auto add = [&](string& s) {
            stringstream ss(s);
            string w;
            while (ss >> w) ++cnt[move(w)];
        };
        add(s1);
        add(s2);
        vector<string> ans;
        for (auto& [s, v] : cnt)
            if (v == 1) ans.emplace_back(s);
        return ans;
    }
};
```

#### Go

```go
func uncommonFromSentences(s1 string, s2 string) (ans []string) {
	cnt := map[string]int{}
	for _, s := range strings.Split(s1, " ") {
		cnt[s]++
	}
	for _, s := range strings.Split(s2, " ") {
		cnt[s]++
	}
	for s, v := range cnt {
		if v == 1 {
			ans = append(ans, s)
		}
	}
	return
}
```

#### TypeScript

```ts
function uncommonFromSentences(s1: string, s2: string): string[] {
    const cnt: Map<string, number> = new Map();
    for (const s of [...s1.split(' '), ...s2.split(' ')]) {
        cnt.set(s, (cnt.get(s) || 0) + 1);
    }
    const ans: Array<string> = [];
    for (const [s, v] of cnt.entries()) {
        if (v == 1) {
            ans.push(s);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn uncommon_from_sentences(s1: String, s2: String) -> Vec<String> {
        let mut map = HashMap::new();
        for s in s1.split(' ') {
            map.insert(s, !map.contains_key(s));
        }
        for s in s2.split(' ') {
            map.insert(s, !map.contains_key(s));
        }
        let mut res = Vec::new();
        for (k, v) in map {
            if v {
                res.push(String::from(k));
            }
        }
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {string[]}
 */
var uncommonFromSentences = function (s1, s2) {
    const cnt = new Map();
    for (const s of [...s1.split(' '), ...s2.split(' ')]) {
        cnt.set(s, (cnt.get(s) || 0) + 1);
    }
    const ans = [];
    for (const [s, v] of cnt.entries()) {
        if (v == 1) {
            ans.push(s);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
