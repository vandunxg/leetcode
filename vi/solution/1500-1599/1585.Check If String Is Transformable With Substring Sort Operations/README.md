---
comments: true
difficulty: Hard
rating: 2333
source: Weekly Contest 206 Q4
tags:
    - Greedy
    - String
    - Sorting
---

<!-- problem:start -->

# [1585. Check If String Is Transformable With Substring Sort Operations](https://leetcode.com/problems/check-if-string-is-transformable-with-substring-sort-operations)

[中文文档](/solution/1500-1599/1585.Check%20If%20String%20Is%20Transformable%20With%20Substring%20Sort%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code>, hãy biến đổi chuỗi <code>s</code> thành chuỗi <code>t</code> bằng cách thực hiện thao tác sau một số lần bất kỳ:</p>

<ul>
	<li>Chọn một chuỗi con <strong>không rỗng</strong> trong <code>s</code> và sắp xếp trực tiếp để các ký tự theo <strong>thứ tự tăng dần</strong>.

    <ul>
    	<li>For example, applying the operation on the underlined substring in <code>&quot;1<u>4234</u>&quot;</code> results in <code>&quot;1<u>2344</u>&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <code>true</code> nếu <em>có thể biến đổi <code>s</code> thành <code>t</code></em>. Nếu không, trả về <code>false</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;84532&quot;, t = &quot;34852&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể biến đổi s thành t bằng các thao tác sắp xếp sau:
&quot;84<u>53</u>2&quot; (from index 2 to 3) -&gt; &quot;84<u>35</u>2&quot;
&quot;<u>843</u>52&quot; (from index 0 to 2) -&gt; &quot;<u>348</u>52&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;34521&quot;, t = &quot;23415&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể biến đổi s thành t bằng các thao tác sắp xếp sau:
&quot;<u>3452</u>1&quot; -&gt; &quot;<u>2345</u>1&quot;
&quot;234<u>51</u>&quot; -&gt; &quot;234<u>15</u>&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;12345&quot;, t = &quot;12435&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s.length == t.length</code></li>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bubble Sort

<!-- thinking:start -->

> **Tư duy**
>
> Sorting arbitrary substrings of $s$ should produce $t$. Each sort is bubble-sort on adjacent pairs: a larger digit cannot pass a smaller one on its left. $n\le 10^5$, so we cannot simulate the sorts.
>
> Store index queues per digit of $s$. To place the next digit $x$ of $t$, take its leftmost leftover index; if any smaller digit still sits to its left, $x$ cannot move here. Otherwise pop that index. Missing frequency also fails.

<!-- thinking:end -->

Về bản chất, bài toán tương đương với việc xác định liệu có thể đổi chỗ các chuỗi con độ dài 2 trong $s$ bằng bubble sort để thu được t hay không.

Therefore, we use an array $pos$ of length 10 to record the indices of each digit in string $s$, where $pos[i]$ represents the list of indices where digit $i$ appears, sorted in ascending order.

Next, we iterate through string $t$. For each character $t[i]$ in $t$, we convert it to the digit $x$. We check if $pos[x]$ is empty. If it is, it means that the digit in $t$ does not exist in $s$, so we return `false`. Otherwise, to swap the character at the first index of $pos[x]$ to index $i$, all indices of digits less than $x$ must be greater than or equal to the first index of $pos[x]. If this condition is not met, we return `false`. Otherwise, we pop the first index from $pos[x] and continue iterating through string $t$.

Sau khi duyệt xong, ta trả về `true`.

Độ phức tạp thời gian là $O(n \times C)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi $s$, còn $C$ là kích thước tập chữ số, bằng 10 trong bài này.

$ and continue iterating through string $

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isTransformable(self, s: str, t: str) -> bool:
        pos = defaultdict(deque)
        for i, c in enumerate(s):
            pos[int(c)].append(i)
        for c in t:
            x = int(c)
            if not pos[x] or any(pos[i] and pos[i][0] < pos[x][0] for i in range(x)):
                return False
            pos[x].popleft()
        return True
```

#### Java

```java
class Solution {
    public boolean isTransformable(String s, String t) {
        Deque<Integer>[] pos = new Deque[10];
        Arrays.setAll(pos, k -> new ArrayDeque<>());
        for (int i = 0; i < s.length(); ++i) {
            pos[s.charAt(i) - '0'].offer(i);
        }
        for (int i = 0; i < t.length(); ++i) {
            int x = t.charAt(i) - '0';
            if (pos[x].isEmpty()) {
                return false;
            }
            for (int j = 0; j < x; ++j) {
                if (!pos[j].isEmpty() && pos[j].peek() < pos[x].peek()) {
                    return false;
                }
            }
            pos[x].poll();
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isTransformable(string s, string t) {
        queue<int> pos[10];
        for (int i = 0; i < s.size(); ++i) {
            pos[s[i] - '0'].push(i);
        }
        for (char& c : t) {
            int x = c - '0';
            if (pos[x].empty()) {
                return false;
            }
            for (int j = 0; j < x; ++j) {
                if (!pos[j].empty() && pos[j].front() < pos[x].front()) {
                    return false;
                }
            }
            pos[x].pop();
        }
        return true;
    }
};
```

#### Go

```go
func isTransformable(s string, t string) bool {
	pos := [10][]int{}
	for i, c := range s {
		pos[c-'0'] = append(pos[c-'0'], i)
	}
	for _, c := range t {
		x := int(c - '0')
		if len(pos[x]) == 0 {
			return false
		}
		for j := 0; j < x; j++ {
			if len(pos[j]) > 0 && pos[j][0] < pos[x][0] {
				return false
			}
		}
		pos[x] = pos[x][1:]
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
