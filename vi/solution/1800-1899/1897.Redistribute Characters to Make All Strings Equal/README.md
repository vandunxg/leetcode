---
comments: true
difficulty: Easy
rating: 1309
source: Weekly Contest 245 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1897. Redistribute Characters to Make All Strings Equal](https://leetcode.com/problems/redistribute-characters-to-make-all-strings-equal)

[中文文档](/solution/1800-1899/1897.Redistribute%20Characters%20to%20Make%20All%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> (đánh chỉ số từ <strong>0</strong>).</p>

<p>Trong một thao tác, chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code>, trong đó <code>words[i]</code> là một chuỗi không rỗng, rồi chuyển <strong>bất kỳ</strong> ký tự nào từ <code>words[i]</code> đến <strong>bất kỳ</strong> vị trí nào trong <code>words[j]</code>.</p>

<p>Trả về <code>true</code> <em>nếu có thể làm cho<strong> mọi</strong> chuỗi trong </em><code>words</code><em> <strong>bằng nhau </strong>bằng cách thực hiện <strong>bất kỳ</strong> số lượng thao tác nào</em>,<em> ngược lại </em><code>false</code> <em>nếu không</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;aabc&quot;,&quot;bc&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chuyển chữ &#39;a&#39; đầu tiên trong <code>words[1] to the front of words[2],
to make </code><code>words[1]</code> = &quot;abc&quot; và words[2] = &quot;abc&quot;.
Lúc này mọi chuỗi đều bằng &quot;abc&quot;, nên trả về <code>true</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;ab&quot;,&quot;a&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể làm cho tất cả chuỗi bằng nhau bằng thao tác đã cho.
</pre>

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

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự có thể được chuyển tự do giữa các chuỗi. Có thể làm các chuỗi bằng nhau khi và chỉ khi tổng số lần xuất hiện của mỗi ký tự chia hết cho số chuỗi.
>
> Đếm tần suất trên toàn bộ danh sách rồi kiểm tra mỗi số đếm có là bội của $n$ hay không.

<!-- thinking:end -->

Theo mô tả bài toán, chỉ cần số lần xuất hiện của mỗi ký tự chia hết cho độ dài mảng chuỗi thì có thể phân phối lại các ký tự để làm tất cả chuỗi bằng nhau.

Vì vậy, ta dùng một hash table hoặc một mảng số nguyên $\textit{cnt}$ có độ dài $26$ để đếm số lần xuất hiện của mỗi ký tự. Cuối cùng, ta kiểm tra xem số lần xuất hiện của từng ký tự có chia hết cho độ dài mảng chuỗi hay không.

Độ phức tạp thời gian là $O(L)$, còn độ phức tạp không gian là $O(|\Sigma|)$. Ở đây, $L$ là tổng độ dài của tất cả chuỗi trong mảng $\textit{words}$, còn $\Sigma$ là tập ký tự gồm các chữ cái viết thường, nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeEqual(self, words: List[str]) -> bool:
        cnt = Counter()
        for w in words:
            for c in w:
                cnt[c] += 1
        n = len(words)
        return all(v % n == 0 for v in cnt.values())
```

#### Java

```java
class Solution {
    public boolean makeEqual(String[] words) {
        int[] cnt = new int[26];
        for (var w : words) {
            for (char c : w.toCharArray()) {
                ++cnt[c - 'a'];
            }
        }
        int n = words.length;
        for (int v : cnt) {
            if (v % n != 0) {
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
    bool makeEqual(vector<string>& words) {
        int cnt[26]{};
        for (const auto& w : words) {
            for (const auto& c : w) {
                ++cnt[c - 'a'];
            }
        }
        int n = words.size();
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] % n != 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func makeEqual(words []string) bool {
	cnt := [26]int{}
	for _, w := range words {
		for _, c := range w {
			cnt[c-'a']++
		}
	}
	n := len(words)
	for _, v := range cnt {
		if v%n != 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function makeEqual(words: string[]): boolean {
    const cnt: Record<string, number> = {};
    for (const w of words) {
        for (const c of w) {
            cnt[c] = (cnt[c] || 0) + 1;
        }
    }
    const n = words.length;
    return Object.values(cnt).every(v => v % n === 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn make_equal(words: Vec<String>) -> bool {
        let mut cnt = std::collections::HashMap::new();

        for word in words.iter() {
            for c in word.chars() {
                *cnt.entry(c).or_insert(0) += 1;
            }
        }

        let n = words.len();
        cnt.values().all(|&v| v % n == 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
