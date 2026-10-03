---
comments: true
difficulty: Hard
rating: 1944
source: Weekly Contest 287 Q4
tags:
    - Design
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2227. Encrypt and Decrypt Strings](https://leetcode.com/problems/encrypt-and-decrypt-strings)

[中文文档](/solution/2200-2299/2227.Encrypt%20and%20Decrypt%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng ký tự <code>keys</code> chứa các ký tự <strong>không trùng lặp</strong> và một mảng chuỗi <code>values</code> chứa các chuỗi có độ dài 2. Bạn cũng được cho một mảng chuỗi <code>dictionary</code> chứa tất cả các chuỗi gốc được phép sau khi giải mã. Hãy triển khai một cấu trúc dữ liệu có thể mã hóa hoặc giải mã một chuỗi được <strong>đánh chỉ số từ 0</strong>.</p>

<p>Một chuỗi được <strong>mã hóa</strong> theo quy trình sau:</p>

<ol>
	<li>Với mỗi ký tự <code>c</code> trong chuỗi, ta tìm chỉ số <code>i</code> thỏa mãn <code>keys[i] == c</code> trong <code>keys</code>.</li>
	<li>Thay <code>c</code> bằng <code>values[i]</code> trong chuỗi.</li>
</ol>

<p>Lưu ý rằng nếu một ký tự của chuỗi <strong>không xuất hiện</strong> trong <code>keys</code>, không thể thực hiện quá trình mã hóa và trả về chuỗi rỗng <code>&quot;&quot;</code>.</p>

<p>Một chuỗi được <strong>giải mã</strong> theo quy trình sau:</p>

<ol>
	<li>Với mỗi chuỗi con <code>s</code> có độ dài 2 xuất hiện tại một chỉ số chẵn trong chuỗi, ta tìm một <code>i</code> sao cho <code>values[i] == s</code>. Nếu có nhiều <code>i</code> hợp lệ, ta chọn <strong>bất kỳ</strong> một giá trị nào trong số đó. Điều này có nghĩa là một chuỗi có thể giải mã thành nhiều chuỗi khác nhau.</li>
	<li>Thay <code>s</code> bằng <code>keys[i]</code> trong chuỗi.</li>
</ol>

<p>Hãy triển khai lớp <code>Encrypter</code>:</p>

<ul>
	<li><code>Encrypter(char[] keys, String[] values, String[] dictionary)</code> khởi tạo lớp <code>Encrypter</code> với <code>keys, values</code> và <code>dictionary</code>.</li>
	<li><code>String encrypt(String word1)</code> mã hóa <code>word1</code> theo quy trình được mô tả ở trên và trả về chuỗi đã mã hóa.</li>
	<li><code>int decrypt(String word2)</code> trả về số chuỗi có thể giải mã từ <code>word2</code> và đồng thời xuất hiện trong <code>dictionary</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Encrypter&quot;, &quot;encrypt&quot;, &quot;decrypt&quot;]
[[[&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;], [&quot;ei&quot;, &quot;zf&quot;, &quot;ei&quot;, &quot;am&quot;], [&quot;abcd&quot;, &quot;acbd&quot;, &quot;adbc&quot;, &quot;badc&quot;, &quot;dacb&quot;, &quot;cadb&quot;, &quot;cbda&quot;, &quot;abad&quot;]], [&quot;abcd&quot;], [&quot;eizfeiam&quot;]]
<strong>Đầu ra</strong>
[null, &quot;eizfeiam&quot;, 2]

<strong>Giải thích</strong>
Encrypter encrypter = new Encrypter([[&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;], [&quot;ei&quot;, &quot;zf&quot;, &quot;ei&quot;, &quot;am&quot;], [&quot;abcd&quot;, &quot;acbd&quot;, &quot;adbc&quot;, &quot;badc&quot;, &quot;dacb&quot;, &quot;cadb&quot;, &quot;cbda&quot;, &quot;abad&quot;]);
encrypter.encrypt(&quot;abcd&quot;); // trả về &quot;eizfeiam&quot;.
&nbsp;                          // &#39;a&#39; ánh xạ tới &quot;ei&quot;, &#39;b&#39; ánh xạ tới &quot;zf&quot;, &#39;c&#39; ánh xạ tới &quot;ei&quot;, và &#39;d&#39; ánh xạ tới &quot;am&quot;.
encrypter.decrypt(&quot;eizfeiam&quot;); // trả về 2.
&nbsp;                               // &quot;ei&quot; có thể ánh xạ tới &#39;a&#39; hoặc &#39;c&#39;, &quot;zf&quot; ánh xạ tới &#39;b&#39;, và &quot;am&quot; ánh xạ tới &#39;d&#39;.
&nbsp;                               // Vì vậy, các chuỗi có thể nhận được sau khi giải mã là &quot;abad&quot;, &quot;cbad&quot;, &quot;abcd&quot; và &quot;cbcd&quot;.
&nbsp;                               // 2 trong số các chuỗi đó, &quot;abad&quot; và &quot;abcd&quot;, xuất hiện trong từ điển, nên đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= keys.length == values.length &lt;= 26</code></li>
	<li><code>values[i].length == 2</code></li>
	<li><code>1 &lt;= dictionary.length &lt;= 100</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 100</code></li>
	<li>Tất cả <code>keys[i]</code> và <code>dictionary[i]</code> đều <strong>không trùng lặp</strong>.</li>
	<li><code>1 &lt;= word1.length &lt;= 2000</code></li>
	<li><code>2 &lt;= word2.length &lt;= 200</code></li>
	<li>Mọi <code>word1[i]</code> đều xuất hiện trong <code>keys</code>.</li>
	<li><code>word2.length</code> là số chẵn.</li>
	<li><code>keys</code>, <code>values[i]</code>, <code>dictionary[i]</code>, <code>word1</code> và <code>word2</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li>Có nhiều nhất <code>200</code> lần gọi <code>encrypt</code> và <code>decrypt</code> được thực hiện <strong>tổng cộng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Mã hóa thay mỗi ký tự bằng một chuỗi cố định có độ dài $2$. Bài toán giải mã yêu cầu đếm số từ trong từ điển khi mã hóa sẽ cho ra một chuỗi đã cho. Từ điển có nhiều nhất $100$ từ, nhưng hàm giải mã có thể được gọi nhiều lần, nên việc tính lại ánh xạ cho từng truy vấn là lãng phí.
>
> Ta xây dựng ánh xạ từ ký tự sang chuỗi mã hóa và đếm số lần mã hóa của mọi từ trong từ điển vào $\textit{cnt}$. Hàm $\textit{encrypt}$ nối các ánh xạ tương ứng (hoặc trả về chuỗi rỗng nếu thiếu khóa); còn $\textit{decrypt}$ chỉ cần tra cứu một lần trong $\textit{cnt}$.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{mp}$ để lưu kết quả mã hóa của mỗi ký tự, và một bảng băm khác $\textit{cnt}$ để lưu số lần xuất hiện của mỗi kết quả mã hóa.

Trong hàm khởi tạo, ta duyệt qua $\textit{keys}$ và $\textit{values}$, lưu mỗi ký tự cùng kết quả mã hóa tương ứng vào $\textit{mp}$. Sau đó, ta duyệt qua $\textit{dictionary}$ để đếm số lần xuất hiện của mỗi kết quả mã hóa. Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của $\textit{keys}$ và $\textit{dictionary}$.

Trong hàm mã hóa, ta duyệt qua từng ký tự của chuỗi đầu vào $\textit{word1}$, tra cứu kết quả mã hóa của ký tự đó và nối các kết quả lại. Nếu một ký tự không có kết quả mã hóa tương ứng, nghĩa là không thể thực hiện mã hóa, và ta trả về chuỗi rỗng. Độ phức tạp thời gian là $O(k)$, trong đó $k$ là độ dài của $\textit{word1}$.

Trong hàm giải mã, ta trả về trực tiếp số lần xuất hiện của $\textit{word2}$ trong $\textit{cnt}$. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(n + m)$.

<!-- tabs:start -->

#### Python3

```python
class Encrypter:
    def __init__(self, keys: List[str], values: List[str], dictionary: List[str]):
        self.mp = dict(zip(keys, values))
        self.cnt = Counter(self.encrypt(v) for v in dictionary)

    def encrypt(self, word1: str) -> str:
        res = []
        for c in word1:
            if c not in self.mp:
                return ''
            res.append(self.mp[c])
        return ''.join(res)

    def decrypt(self, word2: str) -> int:
        return self.cnt[word2]


# Your Encrypter object will be instantiated and called as such:
# obj = Encrypter(keys, values, dictionary)
# param_1 = obj.encrypt(word1)
# param_2 = obj.decrypt(word2)
```

#### Java

```java
class Encrypter {
    private Map<Character, String> mp = new HashMap<>();
    private Map<String, Integer> cnt = new HashMap<>();

    public Encrypter(char[] keys, String[] values, String[] dictionary) {
        for (int i = 0; i < keys.length; ++i) {
            mp.put(keys[i], values[i]);
        }
        for (String w : dictionary) {
            cnt.merge(encrypt(w), 1, Integer::sum);
        }
    }

    public String encrypt(String word1) {
        StringBuilder sb = new StringBuilder();
        for (char c : word1.toCharArray()) {
            if (!mp.containsKey(c)) {
                return "";
            }
            sb.append(mp.get(c));
        }
        return sb.toString();
    }

    public int decrypt(String word2) {
        return cnt.getOrDefault(word2, 0);
    }
}

/**
 * Your Encrypter object will be instantiated and called as such:
 * Encrypter obj = new Encrypter(keys, values, dictionary);
 * String param_1 = obj.encrypt(word1);
 * int param_2 = obj.decrypt(word2);
 */
```

#### C++

```cpp
class Encrypter {
public:
    unordered_map<string, int> cnt;
    unordered_map<char, string> mp;

    Encrypter(vector<char>& keys, vector<string>& values, vector<string>& dictionary) {
        for (int i = 0; i < keys.size(); ++i) {
            mp[keys[i]] = values[i];
        }
        for (auto v : dictionary) {
            cnt[encrypt(v)]++;
        }
    }

    string encrypt(string word1) {
        string res = "";
        for (char c : word1) {
            if (!mp.count(c)) {
                return "";
            }
            res += mp[c];
        }
        return res;
    }

    int decrypt(string word2) {
        return cnt[word2];
    }
};

/**
 * Your Encrypter object will be instantiated and called as such:
 * Encrypter* obj = new Encrypter(keys, values, dictionary);
 * string param_1 = obj->encrypt(word1);
 * int param_2 = obj->decrypt(word2);
 */
```

#### Go

```go
type Encrypter struct {
	mp  map[byte]string
	cnt map[string]int
}

func Constructor(keys []byte, values []string, dictionary []string) Encrypter {
	mp := map[byte]string{}
	cnt := map[string]int{}
	for i, k := range keys {
		mp[k] = values[i]
	}
	e := Encrypter{mp, cnt}
	for _, v := range dictionary {
		e.cnt[e.Encrypt(v)]++
	}
	return e
}

func (this *Encrypter) Encrypt(word1 string) string {
	var ans strings.Builder
	for _, c := range word1 {
		if v, ok := this.mp[byte(c)]; ok {
			ans.WriteString(v)
		} else {
			return ""
		}
	}
	return ans.String()
}

func (this *Encrypter) Decrypt(word2 string) int {
	return this.cnt[word2]
}

/**
 * Your Encrypter object will be instantiated and called as such:
 * obj := Constructor(keys, values, dictionary);
 * param_1 := obj.Encrypt(word1);
 * param_2 := obj.Decrypt(word2);
 */
```

#### TypeScript

```ts
class Encrypter {
    private mp: Map<string, string> = new Map();
    private cnt: Map<string, number> = new Map();

    constructor(keys: string[], values: string[], dictionary: string[]) {
        for (let i = 0; i < keys.length; i++) {
            this.mp.set(keys[i], values[i]);
        }
        for (const w of dictionary) {
            const encrypted = this.encrypt(w);
            if (encrypted !== '') {
                this.cnt.set(encrypted, (this.cnt.get(encrypted) || 0) + 1);
            }
        }
    }

    encrypt(word: string): string {
        let res = '';
        for (const c of word) {
            if (!this.mp.has(c)) {
                return '';
            }
            res += this.mp.get(c);
        }
        return res;
    }

    decrypt(word: string): number {
        return this.cnt.get(word) || 0;
    }
}

/**
 * Your Encrypter object will be instantiated and called as such:
 * const obj = new Encrypter(keys, values, dictionary);
 * const param_1 = obj.encrypt(word1);
 * const param_2 = obj.decrypt(word2);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
