---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.13. Re-Space](https://leetcode.cn/problems/re-space-lcci)

[中文文档](/lcci/17.13.Re-Space/README.md)

## Mô tả

<!-- description:start -->

<p>Ôi không! Bạn đã vô tình xóa toàn bộ khoảng trắng, dấu câu và chữ hoa trong một tài liệu dài. Một câu như &quot;I reset the computer. It still didn&#39;t boot!&quot; đã trở thành &quot;iresetthecomputeritstilldidntboot&#39;&#39;&quot;. Bạn sẽ xử lý dấu câu và chữ hoa sau; lúc này, bạn cần chèn lại khoảng trắng. Hầu hết các từ đều có trong từ điển nhưng một vài từ thì không. Cho một từ điển (danh sách các chuỗi) và tài liệu (một chuỗi), hãy thiết kế một thuật toán để tách lại tài liệu sao cho số ký tự không nhận dạng được là nhỏ nhất. Trả về số ký tự không nhận dạng được.</p>

<p><strong>Lưu ý: </strong>Bài toán này hơi khác so với bài gốc trong sách.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>

dictionary = [&quot;looked&quot;,&quot;just&quot;,&quot;like&quot;,&quot;her&quot;,&quot;brother&quot;]

sentence = &quot;jesslookedjustliketimherbrother&quot;

<strong>Đầu ra: </strong> 7

<strong>Giải thích: </strong> Sau khi tách lại, ta được &quot;<strong>jess</strong> looked just like <strong>tim</strong> her brother&quot;, trong đó có 7 ký tự không nhận dạng được.

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>0 &lt;= len(sentence) &lt;= 1000</code></li>
	<li><code><font face="sans-serif, Arial, Verdana, Trebuchet MS">The total number of characters in&nbsp;</font>dictionary</code>&nbsp;nhỏ hơn hoặc bằng 150000.</li>
	<li>Trong&nbsp;<code>dictionary</code> và&nbsp;<code>sentence</code> chỉ có các chữ cái viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chèn khoảng trắng sao cho số ký tự không nhận dạng được là nhỏ nhất. Số cách tách có thể tăng theo cấp số mũ; kiểm tra các đoạn $O(n^2)$ phù hợp với giới hạn.
>
> $dp[i]$ là số ký tự không nhận dạng được ít nhất trong $i$ ký tự đầu tiên. Đoạn cuối hoặc là một ký tự không nhận dạng được, hoặc là một từ trong từ điển $sentence[j:i]$, chuyển trạng thái từ $dp[j]$.
>
> Một set cho phép kiểm tra phần tử có thuộc tập hợp trong $O(1)$. $dp[0]=0$ và ta điền các giá trị cho đến $n$. Không cần trie khi $n$ vẫn ở mức vừa phải.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def respace(self, dictionary: List[str], sentence: str) -> int:
        s = set(dictionary)
        n = len(sentence)
        dp = [0] * (n + 1)
        for i in range(1, n + 1):
            dp[i] = dp[i - 1] + 1
            for j in range(i):
                if sentence[j:i] in s:
                    dp[i] = min(dp[i], dp[j])
        return dp[-1]
```

#### Java

```java
class Solution {
    public int respace(String[] dictionary, String sentence) {
        Set<String> dict = new HashSet<>(Arrays.asList(dictionary));
        int n = sentence.length();
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            dp[i] = dp[i - 1] + 1;
            for (int j = 0; j < i; ++j) {
                if (dict.contains(sentence.substring(j, i))) {
                    dp[i] = Math.min(dp[i], dp[j]);
                }
            }
        }
        return dp[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int respace(vector<string>& dictionary, string sentence) {
        unordered_set<string> s(dictionary.begin(), dictionary.end());
        int n = sentence.size();
        vector<int> dp(n + 1);
        for (int i = 1; i <= n; ++i) {
            dp[i] = dp[i - 1] + 1;
            for (int j = 0; j < i; ++j) {
                if (s.count(sentence.substr(j, i - j))) {
                    dp[i] = min(dp[i], dp[j]);
                }
            }
        }
        return dp[n];
    }
};
```

#### Go

```go
func respace(dictionary []string, sentence string) int {
	s := map[string]bool{}
	for _, v := range dictionary {
		s[v] = true
	}
	n := len(sentence)
	dp := make([]int, n+1)
	for i := 1; i <= n; i++ {
		dp[i] = dp[i-1] + 1
		for j := 0; j < i; j++ {
			if s[sentence[j:i]] {
				dp[i] = min(dp[i], dp[j])
			}
		}
	}
	return dp[n]
}
```

#### Swift

```swift
class TrieNode {
    var children: [TrieNode?] = Array(repeating: nil, count: 26)
    var isEndOfWord = false
}

class Trie {
    private let root = TrieNode()

    func insert(_ word: String) {
        var node = root
        for char in word {
            let index = Int(char.asciiValue! - Character("a").asciiValue!)
            if node.children[index] == nil {
                node.children[index] = TrieNode()
            }
            node = node.children[index]!
        }
        node.isEndOfWord = true
    }

    func search(_ sentence: Array<Character>, start: Int, end: Int) -> Bool {
        var node = root
        for i in start...end {
            let index = Int(sentence[i].asciiValue! - Character("a").asciiValue!)
            guard let nextNode = node.children[index] else {
                return false
            }
            node = nextNode
        }
        return node.isEndOfWord
    }
}

class Solution {
    func respace(_ dictionary: [String], _ sentence: String) -> Int {
        let n = sentence.count
        guard n > 0 else { return 0 }
        let trie = Trie()
        dictionary.forEach { trie.insert($0) }
        let chars = Array(sentence)
        var dp = Array(repeating: Int.max, count: n + 1)
        dp[0] = 0
        for i in 1...n {
            dp[i] = dp[i - 1] + 1
            for j in 0..<i {
                if trie.search(chars, start: j, end: i - 1) {
                    dp[i] = min(dp[i], dp[j])
                }
            }
        }
        return dp[n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
