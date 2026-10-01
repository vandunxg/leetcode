---
comments: true
difficulty: Easy
tags:
    - Array
    - String
---

<!-- problem:start -->

# [243. Shortest Word Distance 🔒](https://leetcode.com/problems/shortest-word-distance)

[中文文档](/solution/0200-0299/0243.Shortest%20Word%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>wordsDict</code> và hai chuỗi khác nhau đã tồn tại trong mảng là <code>word1</code> và <code>word2</code>, hãy trả về <em>khoảng cách ngắn nhất giữa hai từ này trong danh sách</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> wordsDict = [&quot;practice&quot;, &quot;makes&quot;, &quot;perfect&quot;, &quot;coding&quot;, &quot;makes&quot;], word1 = &quot;coding&quot;, word2 = &quot;practice&quot;
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> wordsDict = [&quot;practice&quot;, &quot;makes&quot;, &quot;perfect&quot;, &quot;coding&quot;, &quot;makes&quot;], word1 = &quot;makes&quot;, word2 = &quot;coding&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= wordsDict.length &lt;= 3 * 10<sup>4</sup></code></li>
    <li><code>1 &lt;= wordsDict[i].length &lt;= 10</code></li>
    <li><code>wordsDict[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
    <li><code>word1</code> và <code>word2</code> đều có trong <code>wordsDict</code>.</li>
    <li><code>word1 != word2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $word1\neq word2$, khoảng cách ngắn nhất là khoảng cách giữa các chỉ số xuất hiện gần đây nhất của hai từ. Theo dõi hai vị trí đó và cập nhật giá trị nhỏ nhất khi duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestDistance(self, wordsDict: List[str], word1: str, word2: str) -> int:
        i = j = -1
        ans = inf
        for k, w in enumerate(wordsDict):
            if w == word1:
                i = k
            if w == word2:
                j = k
            if i != -1 and j != -1:
                ans = min(ans, abs(i - j))
        return ans
```

#### Java

```java
class Solution {
    public int shortestDistance(String[] wordsDict, String word1, String word2) {
        int ans = 0x3f3f3f3f;
        for (int k = 0, i = -1, j = -1; k < wordsDict.length; ++k) {
            if (wordsDict[k].equals(word1)) {
                i = k;
            }
            if (wordsDict[k].equals(word2)) {
                j = k;
            }
            if (i != -1 && j != -1) {
                ans = Math.min(ans, Math.abs(i - j));
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
    int shortestDistance(vector<string>& wordsDict, string word1, string word2) {
        int ans = INT_MAX;
        for (int k = 0, i = -1, j = -1; k < wordsDict.size(); ++k) {
            if (wordsDict[k] == word1) {
                i = k;
            }
            if (wordsDict[k] == word2) {
                j = k;
            }
            if (i != -1 && j != -1) {
                ans = min(ans, abs(i - j));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestDistance(wordsDict []string, word1 string, word2 string) int {
	ans := 0x3f3f3f3f
	i, j := -1, -1
	for k, w := range wordsDict {
		if w == word1 {
			i = k
		}
		if w == word2 {
			j = k
		}
		if i != -1 && j != -1 {
			ans = min(ans, abs(i-j))
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
