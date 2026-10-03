---
comments: true
difficulty: Medium
rating: 1588
source: Biweekly Contest 49 Q2
tags:
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1813. Sentence Similarity III](https://leetcode.com/problems/sentence-similarity-iii)

[中文文档](/solution/1800-1899/1813.Sentence%20Similarity%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>sentence1</code> và <code>sentence2</code>, mỗi chuỗi biểu diễn một <strong>câu</strong> gồm nhiều từ. Một câu là danh sách các <strong>từ</strong> được ngăn cách bằng <strong>đúng một</strong> dấu cách, không có dấu cách ở đầu hoặc cuối. Mỗi từ chỉ gồm các chữ cái tiếng Anh viết hoa và viết thường.</p>

<p>Hai câu <code>s1</code> và <code>s2</code> được xem là <strong>tương tự</strong> nếu có thể chèn một câu bất kỳ (<em>có thể rỗng</em>) vào bên trong một trong hai câu để hai câu trở nên giống hệt nhau. <strong>Lưu ý</strong> rằng câu được chèn phải được ngăn cách với các từ có sẵn bằng dấu cách.</p>

<p>Ví dụ:</p>

<ul>
	<li><code>s1 = &quot;Hello Jane&quot;</code> và <code>s2 = &quot;Hello my name is Jane&quot;</code> có thể trở nên giống nhau bằng cách chèn <code>&quot;my name is&quot;</code> vào giữa <code>&quot;Hello&quot;</code><font face="monospace"> </font>và <code>&quot;Jane&quot;</code><font face="monospace"> trong s1.</font></li>
	<li><font face="monospace"><code>s1 = &quot;Frog cool&quot;</code> </font>và<font face="monospace"> <code>s2 = &quot;Frogs are cool&quot;</code> </font><strong>không</strong> tương tự, vì dù có thể chèn câu <code>&quot;s are&quot;</code> vào <code>s1</code>, câu được chèn không được ngăn cách với <code>&quot;Frog&quot;</code> bằng dấu cách.</li>
</ul>

<p>Với hai câu <code>sentence1</code> và <code>sentence2</code>, trả về <strong>true</strong> nếu <code>sentence1</code> và <code>sentence2</code> <strong>tương tự</strong>. Ngược lại, trả về <strong>false</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sentence1 = &quot;My name is Haley&quot;, sentence2 = &quot;My Haley&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>sentence2</code> có thể trở thành <code>sentence1</code> bằng cách chèn &quot;name is&quot; vào giữa &quot;My&quot; và &quot;Haley&quot;.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sentence1 = &quot;of&quot;, sentence2 = &quot;A lot of words&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể chèn một câu duy nhất vào một trong hai câu để biến chúng thành giống nhau.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">sentence1 = &quot;Eating right now&quot;, sentence2 = &quot;Eating&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>sentence2</code> có thể trở thành <code>sentence1</code> bằng cách chèn &quot;right now&quot; vào cuối câu.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence1.length, sentence2.length &lt;= 100</code></li>
	<li><code>sentence1</code> và <code>sentence2</code> chỉ gồm các chữ cái tiếng Anh viết thường, viết hoa và dấu cách.</li>
	<li>Các từ trong <code>sentence1</code> và <code>sentence2</code> được ngăn cách bằng đúng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Câu ngắn hơn phải trở thành câu dài hơn bằng cách chèn một đoạn từ liên tiếp, nghĩa là nó khớp với tiền tố nối với hậu tố của câu dài hơn. Thử mọi vị trí chèn vừa rối vừa dễ nhầm với việc chèn ở nhiều vị trí.
>
> Tách cả hai câu rồi hoán đổi để câu thứ nhất không ngắn hơn. Đếm tiền tố chung từ trái sang phải và hậu tố chung từ phải sang trái. Nếu tổng hai độ dài này bao phủ toàn bộ câu ngắn hơn, phần chưa khớp là một đoạn duy nhất và chỉ cần một lần chèn.

<!-- thinking:end -->

Ta tách hai câu thành hai mảng từ `words1` và `words2` theo dấu cách. Gọi độ dài của `words1` và `words2` lần lượt là $m$ và $n$, đồng thời giả sử $m \ge nn.

Ta dùng hai con trỏ $i$ và $j$, ban đầu $i = j = 0$. Trước hết, ta lặp để kiểm tra `words1[i]` có bằng `words2[i]` không; nếu có, con trỏ $i$ tiếp tục dịch sang phải. Sau đó, ta lặp để kiểm tra `words1[m - 1 - j]` có bằng `words2[n - 1 - j]` không; nếu có, con trỏ $j$ tiếp tục dịch sang phải.

Sau vòng lặp, nếu $i + j \ge n$, nghĩa là hai câu tương tự nhau, ta trả về `true`; ngược lại, trả về `false`.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của hai câu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areSentencesSimilar(self, sentence1: str, sentence2: str) -> bool:
        words1, words2 = sentence1.split(), sentence2.split()
        m, n = len(words1), len(words2)
        if m < n:
            words1, words2 = words2, words1
            m, n = n, m
        i = j = 0
        while i < n and words1[i] == words2[i]:
            i += 1
        while j < n and words1[m - 1 - j] == words2[n - 1 - j]:
            j += 1
        return i + j >= n
```

#### Java

```java
class Solution {
    public boolean areSentencesSimilar(String sentence1, String sentence2) {
        var words1 = sentence1.split(" ");
        var words2 = sentence2.split(" ");
        if (words1.length < words2.length) {
            var t = words1;
            words1 = words2;
            words2 = t;
        }
        int m = words1.length, n = words2.length;
        int i = 0, j = 0;
        while (i < n && words1[i].equals(words2[i])) {
            ++i;
        }
        while (j < n && words1[m - 1 - j].equals(words2[n - 1 - j])) {
            ++j;
        }
        return i + j >= n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool areSentencesSimilar(string sentence1, string sentence2) {
        auto words1 = split(sentence1, ' ');
        auto words2 = split(sentence2, ' ');
        if (words1.size() < words2.size()) {
            swap(words1, words2);
        }
        int m = words1.size(), n = words2.size();
        int i = 0, j = 0;
        while (i < n && words1[i] == words2[i]) {
            ++i;
        }
        while (j < n && words1[m - 1 - j] == words2[n - 1 - j]) {
            ++j;
        }
        return i + j >= n;
    }

    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }
};
```

#### Go

```go
func areSentencesSimilar(sentence1 string, sentence2 string) bool {
	words1, words2 := strings.Fields(sentence1), strings.Fields(sentence2)
	if len(words1) < len(words2) {
		words1, words2 = words2, words1
	}
	m, n := len(words1), len(words2)
	i, j := 0, 0
	for i < n && words1[i] == words2[i] {
		i++
	}
	for j < n && words1[m-1-j] == words2[n-1-j] {
		j++
	}
	return i+j >= n
}
```

#### TypeScript

```ts
function areSentencesSimilar(sentence1: string, sentence2: string): boolean {
    const [words1, words2] = [sentence1.split(' '), sentence2.split(' ')];
    const [m, n] = [words1.length, words2.length];

    if (m > n) return areSentencesSimilar(sentence2, sentence1);

    let [l, r] = [0, 0];
    for (let i = 0; i < n; i++) {
        if (l === i && words1[i] === words2[i]) l++;
        if (r === i && words2[n - i - 1] === words1[m - r - 1]) r++;
    }

    return l + r >= m;
}
```

#### JavaScript

```js
function areSentencesSimilar(sentence1, sentence2) {
    const [words1, words2] = [sentence1.split(' '), sentence2.split(' ')];
    const [m, n] = [words1.length, words2.length];

    if (m > n) return areSentencesSimilar(sentence2, sentence1);

    let [l, r] = [0, 0];
    for (let i = 0; i < n; i++) {
        if (l === i && words1[i] === words2[i]) l++;
        if (r === i && words2[n - i - 1] === words1[m - r - 1]) r++;
    }

    return l + r >= m;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
