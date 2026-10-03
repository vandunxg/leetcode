---
comments: true
difficulty: Easy
rating: 1335
source: Weekly Contest 324 Q1
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2506. Count Pairs Of Similar Strings](https://leetcode.com/problems/count-pairs-of-similar-strings)

[中文文档](/solution/2500-2599/2506.Count%20Pairs%20Of%20Similar%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Hai chuỗi được gọi là <strong>tương tự</strong> nếu chúng gồm cùng một tập ký tự.</p>

<ul>
	<li>Ví dụ, <code>&quot;abca&quot;</code> và <code>&quot;cba&quot;</code> tương tự vì cả hai đều gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
	<li>Tuy nhiên, <code>&quot;abacba&quot;</code> và <code>&quot;bcfd&quot;</code> không tương tự vì chúng không gồm cùng các ký tự.</li>
</ul>

<p>Trả về <em>số cặp </em><code>(i, j)</code><em> thỏa mãn </em><code>0 &lt;= i &lt; j &lt;= word.length - 1</code><em> và hai chuỗi </em><code>words[i]</code><em> và </em><code>words[j]</code><em> tương tự</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aba&quot;,&quot;aabb&quot;,&quot;abcd&quot;,&quot;bac&quot;,&quot;aabc&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 cặp thỏa mãn các điều kiện:
- i = 0 và j = 1 : words[0] và words[1] chỉ gồm các ký tự &#39;a&#39; và &#39;b&#39;.
- i = 3 và j = 4 : words[3] và words[4] chỉ gồm các ký tự &#39;a&#39;, &#39;b&#39; và &#39;c&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aabb&quot;,&quot;ab&quot;,&quot;ba&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cặp thỏa mãn các điều kiện:
- i = 0 và j = 1 : words[0] và words[1] chỉ gồm các ký tự &#39;a&#39; và &#39;b&#39;.
- i = 0 và j = 2 : words[0] và words[2] chỉ gồm các ký tự &#39;a&#39; và &#39;b&#39;.
- i = 1 và j = 2 : words[1] và words[2] chỉ gồm các ký tự &#39;a&#39; và &#39;b&#39;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;nba&quot;,&quot;cba&quot;,&quot;dba&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì không tồn tại cặp nào thỏa mãn các điều kiện, ta trả về 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Hai chuỗi tương tự khi và chỉ khi chúng sử dụng cùng một tập chữ cái. Việc so sánh các tập hợp theo từng cặp vẫn khả thi khi $n\le 100$, nhưng liên tục xây dựng các tập hợp sẽ bỏ qua thực tế rằng những tập hợp giống nhau có thể được ghép cặp như nhau.
>
> Hai mươi sáu chữ cái có thể được biểu diễn trong một bitmask số nguyên. Khi duyệt, một hash map lưu số lần mỗi mask đã xuất hiện; chuỗi hiện tại ghép cặp được với mọi chuỗi trước đó có cùng mask, sau đó tăng bộ đếm tương ứng.

<!-- thinking:end -->

Với mỗi chuỗi, ta có thể chuyển nó thành một số nhị phân có độ dài $26$, trong đó bit thứ $i$ bằng $1$ cho biết chuỗi chứa chữ cái thứ $i$.

Nếu hai chuỗi chứa cùng các chữ cái thì các số nhị phân của chúng giống nhau. Vì vậy, với mỗi chuỗi, ta dùng một hash table để đếm số lần xuất hiện của số nhị phân tương ứng. Mỗi lần, ta cộng bộ đếm đó vào đáp án rồi tăng bộ đếm của số nhị phân lên $1$.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(n)$. Trong đó, $L$ là tổng độ dài của tất cả các chuỗi và $n$ là số lượng chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def similarPairs(self, words: List[str]) -> int:
        ans = 0
        cnt = Counter()
        for s in words:
            x = 0
            for c in map(ord, s):
                x |= 1 << (c - ord("a"))
            ans += cnt[x]
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int similarPairs(String[] words) {
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (var s : words) {
            int x = 0;
            for (char c : s.toCharArray()) {
                x |= 1 << (c - 'a');
            }
            ans += cnt.merge(x, 1, Integer::sum) - 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int similarPairs(vector<string>& words) {
        int ans = 0;
        unordered_map<int, int> cnt;
        for (const auto& s : words) {
            int x = 0;
            for (auto& c : s) {
                x |= 1 << (c - 'a');
            }
            ans += cnt[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func similarPairs(words []string) (ans int) {
	cnt := map[int]int{}
	for _, s := range words {
		x := 0
		for _, c := range s {
			x |= 1 << (c - 'a')
		}
		ans += cnt[x]
		cnt[x]++
	}
	return
}
```

#### TypeScript

```ts
function similarPairs(words: string[]): number {
    let ans = 0;
    const cnt = new Map<number, number>();
    for (const s of words) {
        let x = 0;
        for (const c of s) {
            x |= 1 << (c.charCodeAt(0) - 97);
        }
        ans += cnt.get(x) || 0;
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn similar_pairs(words: Vec<String>) -> i32 {
        let mut ans = 0;
        let mut cnt: HashMap<i32, i32> = HashMap::new();
        for s in words {
            let mut x = 0;
            for c in s.chars() {
                x |= 1 << ((c as u8) - b'a');
            }
            ans += cnt.get(&x).unwrap_or(&0);
            *cnt.entry(x).or_insert(0) += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
