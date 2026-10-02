---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Bucket Sort
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency)

[中文文档](/solution/0400-0499/0451.Sort%20Characters%20By%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy sắp xếp các ký tự theo <strong>thứ tự tần suất giảm dần</strong>. <strong>Tần suất</strong> của một ký tự là số lần ký tự đó xuất hiện trong chuỗi.</p>

<p>Hãy trả về <em>chuỗi sau khi sắp xếp</em>. Nếu có nhiều đáp án, trả về <em>bất kỳ đáp án nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;tree&quot;
<strong>Đầu ra:</strong> &quot;eert&quot;
<strong>Giải thích:</strong> &#39;e&#39; xuất hiện hai lần, còn &#39;r&#39; và &#39;t&#39; mỗi ký tự xuất hiện một lần.
Vì vậy, &#39;e&#39; phải đứng trước cả &#39;r&#39; và &#39;t&#39;. Do đó, &quot;eetr&quot; cũng là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cccaaa&quot;
<strong>Đầu ra:</strong> &quot;aaaccc&quot;
<strong>Giải thích:</strong> Cả &#39;c&#39; và &#39;a&#39; đều xuất hiện ba lần, nên &quot;cccaaa&quot; và &quot;aaaccc&quot; đều là đáp án hợp lệ.
Lưu ý, &quot;cacaca&quot; không hợp lệ vì các ký tự giống nhau phải nằm liền nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Aabb&quot;
<strong>Đầu ra:</strong> &quot;bbAa&quot;
<strong>Giải thích:</strong> &quot;bbaA&quot; cũng là đáp án hợp lệ, còn &quot;Aabb&quot; thì không.
Lưu ý, &#39;A&#39; và &#39;a&#39; được xem là hai ký tự khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>s</code> gồm chữ cái tiếng Anh viết hoa, viết thường và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Cần sắp xếp lại các ký tự theo tần suất giảm dần; nếu bằng nhau thì có thể chọn thứ tự tùy ý. Bucket theo từng tần suất đòi hỏi giới hạn tần suất, trong khi bảng ký tự nhỏ nên chỉ cần sắp xếp các cặp key-value.
>
> Đếm bằng hash map, sắp xếp theo tần suất giảm dần, rồi lặp lại mỗi ký tự $v$ lần.
>
> Ta chỉ sắp xếp các ký tự phân biệt chứ không sắp xếp toàn bộ chuỗi, nên phần log phụ thuộc vào kích thước bảng ký tự.

<!-- thinking:end -->

Ta dùng hash table $\textit{cnt}$ để đếm số lần xuất hiện của từng ký tự trong chuỗi $s$. Sau đó, sắp xếp các cặp key-value trong $\textit{cnt}$ theo số lần xuất hiện giảm dần. Cuối cùng, nối các ký tự theo thứ tự đã sắp xếp.

Độ phức tạp thời gian là $O(n + k \times \log k)$ và độ phức tạp không gian là $O(n + k)$, trong đó $n$ là độ dài chuỗi $s$, còn $k$ là số lượng ký tự phân biệt.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def frequencySort(self, s: str) -> str:
        cnt = Counter(s)
        return ''.join(c * v for c, v in sorted(cnt.items(), key=lambda x: -x[1]))
```

#### Java

```java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> cnt = new HashMap<>(52);
        for (int i = 0; i < s.length(); ++i) {
            cnt.merge(s.charAt(i), 1, Integer::sum);
        }
        List<Character> cs = new ArrayList<>(cnt.keySet());
        cs.sort((a, b) -> cnt.get(b) - cnt.get(a));
        StringBuilder ans = new StringBuilder();
        for (char c : cs) {
            for (int v = cnt.get(c); v > 0; --v) {
                ans.append(c);
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> cnt;
        for (char& c : s) {
            ++cnt[c];
        }
        vector<char> cs;
        for (auto& [c, _] : cnt) {
            cs.push_back(c);
        }
        sort(cs.begin(), cs.end(), [&](char& a, char& b) {
            return cnt[a] > cnt[b];
        });
        string ans;
        for (char& c : cs) {
            ans += string(cnt[c], c);
        }
        return ans;
    }
};
```

#### Go

```go
func frequencySort(s string) string {
	cnt := map[byte]int{}
	for i := range s {
		cnt[s[i]]++
	}
	cs := make([]byte, 0, len(s))
	for c := range cnt {
		cs = append(cs, c)
	}
	sort.Slice(cs, func(i, j int) bool { return cnt[cs[i]] > cnt[cs[j]] })
	ans := make([]byte, 0, len(s))
	for _, c := range cs {
		ans = append(ans, bytes.Repeat([]byte{c}, cnt[c])...)
	}
	return string(ans)
}
```

#### TypeScript

```ts
function frequencySort(s: string): string {
    const cnt: Map<string, number> = new Map();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    const cs = Array.from(cnt.keys()).sort((a, b) => cnt.get(b)! - cnt.get(a)!);
    const ans: string[] = [];
    for (const c of cs) {
        ans.push(c.repeat(cnt.get(c)!));
    }
    return ans.join('');
}
```

#### Rust

```rust
use std::collections::HashMap;
impl Solution {
    pub fn frequency_sort(s: String) -> String {
        let mut cnt = HashMap::new();
        for c in s.chars() {
            cnt.insert(c, cnt.get(&c).unwrap_or(&0) + 1);
        }
        let mut cs = cnt.into_iter().collect::<Vec<(char, i32)>>();
        cs.sort_unstable_by(|(_, a), (_, b)| b.cmp(&a));
        cs.into_iter()
            .map(|(c, v)| vec![c; v as usize].into_iter().collect::<String>())
            .collect()
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return String
     */
    function frequencySort($s) {
        $cnt = array_count_values(str_split($s));
        $cs = array_keys($cnt);
        usort($cs, function ($a, $b) use ($cnt) {
            return $cnt[$b] <=> $cnt[$a];
        });
        $ans = '';
        foreach ($cs as $c) {
            $ans .= str_repeat($c, $cnt[$c]);
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
