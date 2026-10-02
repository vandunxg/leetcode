---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [893. Groups of Special-Equivalent Strings](https://leetcode.com/problems/groups-of-special-equivalent-strings)

[中文文档](/solution/0800-0899/0893.Groups%20of%20Special-Equivalent%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các chuỗi <code>words</code> có cùng độ dài.</p>

<p>Trong một <strong>lượt</strong>, bạn có thể hoán đổi hai ký tự bất kỳ ở vị trí chẵn hoặc hai ký tự bất kỳ ở vị trí lẻ trong chuỗi <code>words[i]</code>.</p>

<p>Hai chuỗi <code>words[i]</code> và <code>words[j]</code> được gọi là <strong>tương đương đặc biệt</strong> nếu sau một số lượt bất kỳ, ta có thể khiến <code>words[i] == words[j]</code>.</p>

<ul>
	<li>Ví dụ, <code>words[i] = &quot;zzxy&quot;</code> và <code>words[j] = &quot;xyzz&quot;</code> <strong>tương đương đặc biệt</strong> vì ta có thể thực hiện các lượt <code>&quot;zzxy&quot; -&gt; &quot;xzzy&quot; -&gt; &quot;xyzz&quot;</code>.</li>
</ul>

<p>Một <strong>nhóm chuỗi tương đương đặc biệt</strong> trong <code>words</code> là tập con không rỗng thỏa mãn:</p>

<ul>
	<li>Mọi cặp chuỗi trong nhóm đều tương đương đặc biệt; và</li>
	<li>Nhóm có kích thước lớn nhất có thể (tức là không tồn tại chuỗi <code>words[i]</code> nằm ngoài nhóm nhưng lại tương đương đặc biệt với mọi chuỗi trong nhóm).</li>
</ul>

<p>Hãy trả về <em>số lượng </em><strong>nhóm chuỗi tương đương đặc biệt</strong><em> trong </em><code>words</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abcd&quot;,&quot;cdab&quot;,&quot;cbad&quot;,&quot;xyzz&quot;,&quot;zzxy&quot;,&quot;zzyx&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 
Một nhóm là [&quot;abcd&quot;, &quot;cdab&quot;, &quot;cbad&quot;], vì chúng đôi một tương đương đặc biệt, và không chuỗi nào khác tương đương đặc biệt với tất cả các chuỗi trong nhóm này.
Hai nhóm còn lại là [&quot;xyzz&quot;, &quot;zzxy&quot;] và [&quot;zzyx&quot;].
Đặc biệt, lưu ý rằng &quot;zzxy&quot; không tương đương đặc biệt với &quot;zzyx&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;acb&quot;,&quot;bac&quot;,&quot;bca&quot;,&quot;cab&quot;,&quot;cba&quot;]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả các chuỗi đều có cùng độ dài.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các vị trí lẻ và chẵn có thể được hoán vị độc lập, nên mỗi nhóm tương đương được xác định bởi cặp multiset ký tự ở hai loại vị trí này. Có tối đa $1000$ chuỗi, mỗi chuỗi dài $20$, vì vậy ta chuẩn hóa từng chuỗi rồi thêm vào một set.
>
> Sắp xếp các chữ cái ở vị trí chẵn và vị trí lẻ, nối hai phần lại thành một signature, rồi đếm số signature khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSpecialEquivGroups(self, words: List[str]) -> int:
        s = {''.join(sorted(word[::2]) + sorted(word[1::2])) for word in words}
        return len(s)
```

#### Java

```java
class Solution {
    public int numSpecialEquivGroups(String[] words) {
        Set<String> s = new HashSet<>();
        for (String word : words) {
            s.add(convert(word));
        }
        return s.size();
    }

    private String convert(String word) {
        List<Character> a = new ArrayList<>();
        List<Character> b = new ArrayList<>();
        for (int i = 0; i < word.length(); ++i) {
            char ch = word.charAt(i);
            if (i % 2 == 0) {
                a.add(ch);
            } else {
                b.add(ch);
            }
        }
        Collections.sort(a);
        Collections.sort(b);
        StringBuilder sb = new StringBuilder();
        for (char c : a) {
            sb.append(c);
        }
        for (char c : b) {
            sb.append(c);
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSpecialEquivGroups(vector<string>& words) {
        unordered_set<string> s;
        for (auto& word : words) {
            string a = "", b = "";
            for (int i = 0; i < word.size(); ++i) {
                if (i & 1)
                    a += word[i];
                else
                    b += word[i];
            }
            sort(a.begin(), a.end());
            sort(b.begin(), b.end());
            s.insert(a + b);
        }
        return s.size();
    }
};
```

#### Go

```go
func numSpecialEquivGroups(words []string) int {
	s := map[string]bool{}
	for _, word := range words {
		a, b := []rune{}, []rune{}
		for i, c := range word {
			if i&1 == 1 {
				a = append(a, c)
			} else {
				b = append(b, c)
			}
		}
		sort.Slice(a, func(i, j int) bool {
			return a[i] < a[j]
		})
		sort.Slice(b, func(i, j int) bool {
			return b[i] < b[j]
		})
		s[string(a)+string(b)] = true
	}
	return len(s)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
