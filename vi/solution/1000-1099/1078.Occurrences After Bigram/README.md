---
comments: true
difficulty: Easy
rating: 1274
source: Weekly Contest 140 Q1
tags:
    - String
---

<!-- problem:start -->

# [1078. Occurrences After Bigram](https://leetcode.com/problems/occurrences-after-bigram)

[中文文档](/solution/1000-1099/1078.Occurrences%20After%20Bigram/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>first</code> và <code>second</code>. Xét các cụm từ xuất hiện trong văn bản theo dạng <code>&quot;first second third&quot;</code>, trong đó <code>second</code> đứng ngay sau <code>first</code>, còn <code>third</code> đứng ngay sau <code>second</code>.</p>

<p>Với mỗi lần xuất hiện của cụm <code>&quot;first second third&quot;</code>, trả về từ <code>third</code>. Kết quả là một mảng chứa tất cả các từ đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> text = "alice is a good girl she is a good student", first = "a", second = "good"
<strong>Đầu ra:</strong> ["girl","student"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> text = "we will we will rock you", first = "we", second = "will"
<strong>Đầu ra:</strong> ["we","rock"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 1000</code></li>
	<li><code>text</code> chỉ gồm chữ cái tiếng Anh viết thường và dấu cách.</li>
	<li>Các từ trong <code>text</code> được ngăn cách bằng <strong>một dấu cách duy nhất</strong>.</li>
	<li><code>1 &lt;= first.length, second.length &lt;= 10</code></li>
	<li><code>first</code> và <code>second</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>text</code> không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm từ đứng sau một bigram (cặp từ liền kề) đã cho. Vì các từ trong text chỉ được ngăn cách bằng một dấu cách, chỉ cần tách chuỗi rồi kiểm tra từng bộ ba từ; độ dài text tối đa là $\le 1000$.
>
> Với mỗi chỉ số $i$, xét $(words[i],words[i+1],words[i+2])$ và lấy từ thứ ba nếu hai từ đầu khớp.
>
> Một lượt duyệt tuyến tính sẽ tìm được mọi lần xuất hiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findOcurrences(self, text: str, first: str, second: str) -> List[str]:
        words = text.split()
        ans = []
        for i in range(len(words) - 2):
            a, b, c = words[i : i + 3]
            if a == first and b == second:
                ans.append(c)
        return ans
```

#### Java

```java
class Solution {

    public String[] findOcurrences(String text, String first, String second) {
        String[] words = text.split(" ");
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < words.length - 2; ++i) {
            if (first.equals(words[i]) && second.equals(words[i + 1])) {
                ans.add(words[i + 2]);
            }
        }
        return ans.toArray(new String[0]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findOcurrences(string text, string first, string second) {
        istringstream is(text);
        vector<string> words;
        string word;
        while (is >> word) {
            words.emplace_back(word);
        }
        vector<string> ans;
        int n = words.size();
        for (int i = 0; i < n - 2; ++i) {
            if (words[i] == first && words[i + 1] == second) {
                ans.emplace_back(words[i + 2]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findOcurrences(text string, first string, second string) (ans []string) {
	words := strings.Split(text, " ")
	n := len(words)
	for i := 0; i < n-2; i++ {
		if words[i] == first && words[i+1] == second {
			ans = append(ans, words[i+2])
		}
	}
	return
}
```

#### TypeScript

```ts
function findOcurrences(text: string, first: string, second: string): string[] {
    const words = text.split(' ');
    const n = words.length;
    const ans: string[] = [];
    for (let i = 0; i < n - 2; i++) {
        if (words[i] === first && words[i + 1] === second) {
            ans.push(words[i + 2]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
