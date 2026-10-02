---
comments: true
difficulty: Medium
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - Bucket Sort
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words)

[中文文档](/solution/0600-0699/0692.Top%20K%20Frequent%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code> và số nguyên <code>k</code>, hãy trả về <em><code>k</code> chuỗi xuất hiện nhiều nhất</em>.</p>

<p>Trả về kết quả đã được <strong>sắp xếp</strong> theo <strong>tần suất</strong> giảm dần. Nếu các từ có cùng tần suất, sắp xếp chúng theo <strong>thứ tự từ điển</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;i&quot;,&quot;love&quot;,&quot;leetcode&quot;,&quot;i&quot;,&quot;love&quot;,&quot;coding&quot;], k = 2
<strong>Đầu ra:</strong> [&quot;i&quot;,&quot;love&quot;]
<strong>Giải thích:</strong> &quot;i&quot; và &quot;love&quot; là hai từ xuất hiện nhiều nhất.
Lưu ý rằng &quot;i&quot; đứng trước &quot;love&quot; vì có thứ tự chữ cái thấp hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;the&quot;,&quot;day&quot;,&quot;is&quot;,&quot;sunny&quot;,&quot;the&quot;,&quot;the&quot;,&quot;the&quot;,&quot;sunny&quot;,&quot;is&quot;,&quot;is&quot;], k = 4
<strong>Đầu ra:</strong> [&quot;the&quot;,&quot;is&quot;,&quot;sunny&quot;,&quot;day&quot;]
<strong>Giải thích:</strong> &quot;the&quot;, &quot;is&quot;, &quot;sunny&quot; và &quot;day&quot; là bốn từ xuất hiện nhiều nhất, với số lần xuất hiện lần lượt là 4, 3, 2 và 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 500</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>k</code> is in the range <code>[1, The number of <strong>unique</strong> words[i]]</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(n log(k))</code> và bộ nhớ phụ <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Trả về $k$ từ xuất hiện nhiều nhất; nếu tần suất bằng nhau thì xếp theo thứ tự từ điển. Dùng heap có độ phức tạp $O(n\log k)$, nhưng ở đây sắp xếp toàn bộ vẫn phù hợp.
>
> Đếm tần suất, sắp xếp các key theo $(-count, word)$ rồi lấy $k$ key đầu tiên.

<!-- thinking:end -->

Ta có thể dùng hash table $\textit{cnt}$ để lưu tần suất của từng từ. Sau đó, sắp xếp các cặp key-value trong hash table theo value; nếu value bằng nhau thì sắp xếp theo key.

Cuối cùng, lấy $k$ key đầu tiên.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        cnt = Counter(words)
        return sorted(cnt, key=lambda x: (-cnt[x], x))[:k]
```

#### Java

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String w : words) {
            cnt.merge(w, 1, Integer::sum);
        }
        Arrays.sort(words, (a, b) -> {
            int c1 = cnt.get(a), c2 = cnt.get(b);
            return c1 == c2 ? a.compareTo(b) : c2 - c1;
        });
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < words.length && ans.size() < k; ++i) {
            if (i == 0 || !words[i].equals(words[i - 1])) {
                ans.add(words[i]);
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
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> cnt;
        for (const auto& w : words) {
            ++cnt[w];
        }
        vector<string> ans;
        for (const auto& [w, _] : cnt) {
            ans.push_back(w);
        }
        ranges::sort(ans, [&](const string& a, const string& b) {
            return cnt[a] > cnt[b] || (cnt[a] == cnt[b] && a < b);
        });
        ans.resize(k);
        return ans;
    }
};
```

#### Go

```go
func topKFrequent(words []string, k int) (ans []string) {
	cnt := map[string]int{}
	for _, w := range words {
		cnt[w]++
	}
	for w := range cnt {
		ans = append(ans, w)
	}
	sort.Slice(ans, func(i, j int) bool { a, b := ans[i], ans[j]; return cnt[a] > cnt[b] || cnt[a] == cnt[b] && a < b })
	return ans[:k]
}
```

#### TypeScript

```ts
function topKFrequent(words: string[], k: number): string[] {
    const cnt: Map<string, number> = new Map();
    for (const w of words) {
        cnt.set(w, (cnt.get(w) || 0) + 1);
    }
    const ans: string[] = Array.from(cnt.keys());
    ans.sort((a, b) => {
        return cnt.get(a) === cnt.get(b) ? a.localeCompare(b) : cnt.get(b)! - cnt.get(a)!;
    });
    return ans.slice(0, k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
