---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [737. Sentence Similarity II 🔒](https://leetcode.com/problems/sentence-similarity-ii)

[中文文档](/solution/0700-0799/0737.Sentence%20Similarity%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có thể biểu diễn một câu dưới dạng mảng các từ. Ví dụ, câu <code>&quot;I am happy with leetcode&quot;</code> có thể được biểu diễn thành <code>arr = [&quot;I&quot;,&quot;am&quot;,happy&quot;,&quot;with&quot;,&quot;leetcode&quot;]</code>.</p>

<p>Cho hai câu <code>sentence1</code> và <code>sentence2</code>, mỗi câu được biểu diễn dưới dạng mảng chuỗi, cùng mảng các cặp chuỗi <code>similarPairs</code>, trong đó <code>similarPairs[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> cho biết hai từ <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> tương tự nhau.</p>

<p>Trả về <code>true</code><em> nếu <code>sentence1</code> và <code>sentence2</code> tương tự nhau, hoặc </em><code>false</code><em> nếu chúng không tương tự nhau</em>.</p>

<p>Hai câu tương tự nhau nếu:</p>

<ul>
	<li>Chúng có <strong>độ dài bằng nhau</strong> (tức là cùng số từ).</li>
	<li><code>sentence1[i]</code> và <code>sentence2[i]</code> tương tự nhau.</li>
</ul>

<p>Lưu ý rằng một từ luôn tương tự với chính nó, và quan hệ tương tự có tính bắc cầu. Ví dụ, nếu các từ <code>a</code> và <code>b</code> tương tự nhau, đồng thời <code>b</code> và <code>c</code> tương tự nhau, thì <code>a</code> và <code>c</code> cũng <strong>tương tự</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;great&quot;,&quot;acting&quot;,&quot;skills&quot;], sentence2 = [&quot;fine&quot;,&quot;drama&quot;,&quot;talent&quot;], similarPairs = [[&quot;great&quot;,&quot;good&quot;],[&quot;fine&quot;,&quot;good&quot;],[&quot;drama&quot;,&quot;acting&quot;],[&quot;skills&quot;,&quot;talent&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai câu có cùng độ dài và mỗi từ ở vị trí i trong sentence1 đều tương tự với từ tương ứng trong sentence2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;I&quot;,&quot;love&quot;,&quot;leetcode&quot;], sentence2 = [&quot;I&quot;,&quot;love&quot;,&quot;onepiece&quot;], similarPairs = [[&quot;manga&quot;,&quot;onepiece&quot;],[&quot;platform&quot;,&quot;anime&quot;],[&quot;leetcode&quot;,&quot;platform&quot;],[&quot;anime&quot;,&quot;manga&quot;]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> &quot;leetcode&quot; --&gt; &quot;platform&quot; --&gt; &quot;anime&quot; --&gt; &quot;manga&quot; --&gt; &quot;onepiece&quot;.
Vì &quot;leetcode&quot; tương tự với &quot;onepiece&quot; và hai từ đầu tiên giống nhau, hai câu này tương tự nhau.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence1 = [&quot;I&quot;,&quot;love&quot;,&quot;leetcode&quot;], sentence2 = [&quot;I&quot;,&quot;love&quot;,&quot;onepiece&quot;], similarPairs = [[&quot;manga&quot;,&quot;hunterXhunter&quot;],[&quot;platform&quot;,&quot;anime&quot;],[&quot;leetcode&quot;,&quot;platform&quot;],[&quot;anime&quot;,&quot;manga&quot;]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> &quot;leetcode&quot; không tương tự với &quot;onepiece&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence1.length, sentence2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= sentence1[i].length, sentence2[i].length &lt;= 20</code></li>
	<li><code>sentence1[i]</code> và <code>sentence2[i]</code> chỉ gồm chữ cái tiếng Anh viết thường và viết hoa.</li>
	<li><code>0 &lt;= similarPairs.length &lt;= 2000</code></li>
	<li><code>similarPairs[i].length == 2</code></li>
	<li><code>1 &lt;= x<sub>i</sub>.length, y<sub>i</sub>.length &lt;= 20</code></li>
	<li><code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> chỉ gồm các chữ cái tiếng Anh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khác với Sentence Similarity I, quan hệ tương tự ở đây có tính bắc cầu. Chỉ tra cứu từng cặp là chưa đủ; ta cần xác định các thành phần liên thông. Có thể dùng union-find trên các từ trong $\textit{similarPairs}$.
>
> Gán id cho các từ, union từng cặp, rồi so sánh hai câu: các từ giống nhau thì đạt; nếu khác nhau, cả hai từ phải có trong map và có cùng root.
>
> Nếu độ dài hai câu khác nhau thì trả về false ngay. Path compression giúp độ phức tạp gần tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areSentencesSimilarTwo(
        self, sentence1: List[str], sentence2: List[str], similarPairs: List[List[str]]
    ) -> bool:
        if len(sentence1) != len(sentence2):
            return False
        n = len(similarPairs)
        p = list(range(n << 1))

        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        words = {}
        idx = 0
        for a, b in similarPairs:
            if a not in words:
                words[a] = idx
                idx += 1
            if b not in words:
                words[b] = idx
                idx += 1
            p[find(words[a])] = find(words[b])

        for i in range(len(sentence1)):
            if sentence1[i] == sentence2[i]:
                continue
            if (
                sentence1[i] not in words
                or sentence2[i] not in words
                or find(words[sentence1[i]]) != find(words[sentence2[i]])
            ):
                return False
        return True
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean areSentencesSimilarTwo(
        String[] sentence1, String[] sentence2, List<List<String>> similarPairs) {
        if (sentence1.length != sentence2.length) {
            return false;
        }
        int n = similarPairs.size();
        p = new int[n << 1];
        for (int i = 0; i < p.length; ++i) {
            p[i] = i;
        }
        Map<String, Integer> words = new HashMap<>();
        int idx = 0;
        for (List<String> e : similarPairs) {
            String a = e.get(0), b = e.get(1);
            if (!words.containsKey(a)) {
                words.put(a, idx++);
            }
            if (!words.containsKey(b)) {
                words.put(b, idx++);
            }
            p[find(words.get(a))] = find(words.get(b));
        }
        for (int i = 0; i < sentence1.length; ++i) {
            if (Objects.equals(sentence1[i], sentence2[i])) {
                continue;
            }
            if (!words.containsKey(sentence1[i]) || !words.containsKey(sentence2[i])
                || find(words.get(sentence1[i])) != find(words.get(sentence2[i]))) {
                return false;
            }
        }
        return true;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> p;
    bool areSentencesSimilarTwo(vector<string>& sentence1, vector<string>& sentence2, vector<vector<string>>& similarPairs) {
        if (sentence1.size() != sentence2.size())
            return false;
        int n = similarPairs.size();
        p.resize(n << 1);
        for (int i = 0; i < p.size(); ++i)
            p[i] = i;
        unordered_map<string, int> words;
        int idx = 0;
        for (auto e : similarPairs) {
            string a = e[0], b = e[1];
            if (!words.count(a))
                words[a] = idx++;
            if (!words.count(b))
                words[b] = idx++;
            p[find(words[a])] = find(words[b]);
        }
        for (int i = 0; i < sentence1.size(); ++i) {
            if (sentence1[i] == sentence2[i])
                continue;
            if (!words.count(sentence1[i]) || !words.count(sentence2[i]) || find(words[sentence1[i]]) != find(words[sentence2[i]]))
                return false;
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x)
            p[x] = find(p[x]);
        return p[x];
    }
};
```

#### Go

```go
var p []int

func areSentencesSimilarTwo(sentence1 []string, sentence2 []string, similarPairs [][]string) bool {
	if len(sentence1) != len(sentence2) {
		return false
	}
	n := len(similarPairs)
	p = make([]int, (n<<1)+10)
	for i := 0; i < len(p); i++ {
		p[i] = i
	}
	words := make(map[string]int)
	idx := 1
	for _, e := range similarPairs {
		a, b := e[0], e[1]
		if words[a] == 0 {
			words[a] = idx
			idx++
		}
		if words[b] == 0 {
			words[b] = idx
			idx++
		}
		p[find(words[a])] = find(words[b])
	}
	for i := 0; i < len(sentence1); i++ {
		if sentence1[i] == sentence2[i] {
			continue
		}
		if words[sentence1[i]] == 0 || words[sentence2[i]] == 0 || find(words[sentence1[i]]) != find(words[sentence2[i]]) {
			return false
		}
	}
	return true
}

func find(x int) int {
	if p[x] != x {
		p[x] = find(p[x])
	}
	return p[x]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
