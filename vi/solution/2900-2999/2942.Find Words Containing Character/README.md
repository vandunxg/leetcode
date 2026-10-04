---
comments: true
difficulty: Easy
rating: 1182
source: Biweekly Contest 118 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2942. Find Words Containing Character](https://leetcode.com/problems/find-words-containing-character)

[中文文档](/solution/2900-2999/2942.Find%20Words%20Containing%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi được đánh chỉ số từ <strong>0</strong> <code>words</code> và một ký tự <code>x</code>.</p>

<p>Trả về <em>một <strong>mảng các chỉ số</strong> biểu diễn những từ chứa ký tự </em><code>x</code>.</p>

<p><strong>Lưu ý</strong> rằng mảng được trả về có thể theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;leet&quot;,&quot;code&quot;], x = &quot;e&quot;
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong> &quot;e&quot; xuất hiện trong cả hai từ: &quot;l<strong><u>ee</u></strong>t&quot; và &quot;cod<u><strong>e</strong></u>&quot;. Vì vậy, ta trả về các chỉ số 0 và 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;bcd&quot;,&quot;aaaa&quot;,&quot;cbc&quot;], x = &quot;a&quot;
<strong>Đầu ra:</strong> [0,2]
<strong>Giải thích:</strong> &quot;a&quot; xuất hiện trong &quot;<strong><u>a</u></strong>bc&quot; và &quot;<u><strong>aaaa</strong></u>&quot;. Vì vậy, ta trả về các chỉ số 0 và 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;bcd&quot;,&quot;aaaa&quot;,&quot;cbc&quot;], x = &quot;z&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> &quot;z&quot; không xuất hiện trong bất kỳ từ nào. Vì vậy, ta trả về một mảng rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 50</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 50</code></li>
	<li><code>x</code> là một chữ cái tiếng Anh viết thường.</li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Trả về các chỉ số của những từ chứa $x$. Cả danh sách và mỗi từ đều có độ dài không quá $50$, nên chỉ cần kiểm tra sự xuất hiện của ký tự này trong từng từ.
>
> Một list comprehension thu thập các chỉ số theo thứ tự; không cần thêm biến chỉ số nào.

<!-- thinking:end -->

Ta duyệt trực tiếp từng chuỗi `words[i]` trong mảng chuỗi `words`. Nếu `x` xuất hiện trong `words[i]`, ta thêm `i` vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi trong mảng `words`. Không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWordsContaining(self, words: List[str], x: str) -> List[int]:
        return [i for i, w in enumerate(words) if x in w]
```

#### Java

```java
class Solution {
    public List<Integer> findWordsContaining(String[] words, char x) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < words.length; ++i) {
            if (words[i].indexOf(x) != -1) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findWordsContaining(vector<string>& words, char x) {
        vector<int> ans;
        for (int i = 0; i < words.size(); ++i) {
            if (words[i].find(x) != string::npos) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findWordsContaining(words []string, x byte) (ans []int) {
	for i, w := range words {
		for _, c := range w {
			if byte(c) == x {
				ans = append(ans, i)
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findWordsContaining(words: string[], x: string): number[] {
    return words.flatMap((w, i) => (w.includes(x) ? [i] : []));
}
```

#### Rust

```rust
impl Solution {
    pub fn find_words_containing(words: Vec<String>, x: char) -> Vec<i32> {
        words.into_iter().enumerate()
            .filter_map(|(i, w)| w.contains(x).then(|| i as i32))
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
