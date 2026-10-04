---
comments: true
difficulty: Easy
rating: 1405
source: Biweekly Contest 107 Q1
tags:
    - Array
    - Hash Table
    - String
    - Simulation
---

<!-- problem:start -->

# [2744. Find Maximum Number of String Pairs](https://leetcode.com/problems/find-maximum-number-of-string-pairs)

[中文文档](/solution/2700-2799/2744.Find%20Maximum%20Number%20of%20String%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>words</code> gồm các chuỗi <strong>phân biệt</strong>.</p>

<p>Chuỗi <code>words[i]</code> có thể ghép cặp với chuỗi <code>words[j]</code> nếu:</p>

<ul>
	<li>Chuỗi <code>words[i]</code> bằng chuỗi đảo ngược của <code>words[j]</code>.</li>
	<li><code>0 &lt;= i &lt; j &lt; words.length</code>.</li>
</ul>

<p>Trả về <em>số lượng cặp <strong>lớn nhất</strong> có thể tạo từ mảng </em><code>words</code><em>.</em></p>

<p>Lưu ý rằng mỗi chuỗi chỉ có thể thuộc về <strong>nhiều nhất một</strong> cặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cd&quot;,&quot;ac&quot;,&quot;dc&quot;,&quot;ca&quot;,&quot;zz&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể tạo 2 cặp chuỗi như sau:
- Ta ghép chuỗi thứ 0<sup>th</sup> với chuỗi thứ 2<sup>nd</sup>, vì chuỗi đảo ngược của word[0] là &quot;dc&quot; và bằng words[2].
- Ta ghép chuỗi thứ 1<sup>st</sup> với chuỗi thứ 3<sup>rd</sup>, vì chuỗi đảo ngược của word[1] là &quot;ca&quot; và bằng words[3].
Có thể chứng minh rằng 2 là số lượng cặp lớn nhất có thể tạo.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;ab&quot;,&quot;ba&quot;,&quot;cc&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể tạo 1 cặp chuỗi như sau:
- Ta ghép chuỗi thứ 0<sup>th</sup> với chuỗi thứ 1<sup>st</sup>, vì chuỗi đảo ngược của words[1] là &quot;ab&quot; và bằng words[0].
Có thể chứng minh rằng 1 là số lượng cặp lớn nhất có thể tạo.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aa&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Trong ví dụ này, ta không thể tạo được cặp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 50</code></li>
	<li><code>words[i].length == 2</code></li>
	<li><code>words</code> gồm các chuỗi phân biệt.</li>
	<li><code>words[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Hai chỉ số khác nhau tạo thành một cặp khi các chuỗi là đảo ngược của nhau, và mỗi chuỗi được sử dụng nhiều nhất một lần. Sắp xếp rồi ghép cặp cũng được, nhưng vì các chuỗi có độ dài $2$, chỉ cần đếm online là đủ.
>
> Duyệt từ trái sang phải: nếu chuỗi đảo ngược của từ hiện tại đã xuất hiện, tạo một cặp, sau đó tăng số đếm của từ đó. Vì mỗi chuỗi chỉ được sử dụng một lần, số đếm dương nghĩa là đã tìm thấy một cặp.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $cnt$ để lưu số lần xuất hiện của mỗi chuỗi đảo ngược trong mảng $words$.

Ta duyệt qua mảng $words$. Với mỗi chuỗi $w$, ta cộng số lần xuất hiện của chuỗi đảo ngược của nó vào đáp án, sau đó tăng số đếm của $w$ lên $1$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $words$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumNumberOfStringPairs(self, words: List[str]) -> int:
        cnt = Counter()
        ans = 0
        for w in words:
            ans += cnt[w[::-1]]
            cnt[w] += 1
        return ans
```

#### Java

```java
class Solution {
    public int maximumNumberOfStringPairs(String[] words) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0;
        for (var w : words) {
            int a = w.charAt(0) - 'a', b = w.charAt(1) - 'a';
            ans += cnt.getOrDefault(b << 5 | a, 0);
            cnt.merge(a << 5 | b, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumNumberOfStringPairs(vector<string>& words) {
        unordered_map<int, int> cnt;
        int ans = 0;
        for (auto& w : words) {
            int a = w[0] - 'a', b = w[1] - 'a';
            ans += cnt[b << 5 | a];
            cnt[a << 5 | b]++;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumNumberOfStringPairs(words []string) (ans int) {
	cnt := map[int]int{}
	for _, w := range words {
		a, b := int(w[0]-'a'), int(w[1]-'a')
		ans += cnt[b<<5|a]
		cnt[a<<5|b]++
	}
	return
}
```

#### TypeScript

```ts
function maximumNumberOfStringPairs(words: string[]): number {
    const cnt: { [key: number]: number } = {};
    let ans = 0;
    for (const w of words) {
        const [a, b] = [w.charCodeAt(0) - 97, w.charCodeAt(w.length - 1) - 97];
        ans += cnt[(b << 5) | a] || 0;
        cnt[(a << 5) | b] = (cnt[(a << 5) | b] || 0) + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
