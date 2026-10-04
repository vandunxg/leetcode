---
comments: true
difficulty: Medium
rating: 1311
source: Weekly Contest 399 Q2
tags:
    - String
---

<!-- problem:start -->

# [3163. String Compression III](https://leetcode.com/problems/string-compression-iii)

[中文文档](/solution/3100-3199/3163.String%20Compression%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>, hãy nén chuỗi theo thuật toán sau:</p>

<ul>
    <li>Bắt đầu với một chuỗi rỗng <code>comp</code>. Khi <code>word</code> <strong>chưa</strong> rỗng, thực hiện thao tác sau:

    <ul>
        <li>Xóa tiền tố dài nhất của <code>word</code> gồm một <em>ký tự duy nhất</em> <code>c</code> lặp lại <strong>tối đa</strong> 9 lần.</li>
        <li>Nối độ dài của tiền tố, sau đó là <code>c</code>, vào <code>comp</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về chuỗi <code>comp</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abcde&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;1a1b1c1d1e&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>comp = &quot;&quot;</code>. Thực hiện thao tác 5 lần, lần lượt chọn <code>&quot;a&quot;</code>, <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code>, <code>&quot;d&quot;</code> và <code>&quot;e&quot;</code> làm tiền tố trong mỗi thao tác.</p>

<p>Với mỗi tiền tố, nối <code>&quot;1&quot;</code> rồi đến ký tự đó vào <code>comp</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aaaaaaaaaaaaaabb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;9a5a2b&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>comp = &quot;&quot;</code>. Thực hiện thao tác 3 lần, lần lượt chọn <code>&quot;aaaaaaaaa&quot;</code>, <code>&quot;aaaaa&quot;</code> và <code>&quot;bb&quot;</code> làm tiền tố.</p>

<ul>
    <li>Với tiền tố <code>&quot;aaaaaaaaa&quot;</code>, nối <code>&quot;9&quot;</code> rồi đến <code>&quot;a&quot;</code> vào <code>comp</code>.</li>
    <li>Với tiền tố <code>&quot;aaaaa&quot;</code>, nối <code>&quot;5&quot;</code> rồi đến <code>&quot;a&quot;</code> vào <code>comp</code>.</li>
    <li>Với tiền tố <code>&quot;bb&quot;</code>, nối <code>&quot;2&quot;</code> rồi đến <code>&quot;b&quot;</code> vào <code>comp</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Khi nén, ta ghi số lượng (tối đa $9$) rồi đến ký tự cho mỗi đoạn liên tiếp. Việc tự quản lý chỉ số thường dễ xử lý sai các lần tách tại $9$.
>
> `groupby` đã trả về các đoạn liên tiếp; sau đó chia mỗi đoạn thành các khối có kích thước tối đa $9$.
>
> Với một đoạn có độ dài $k$, thêm `str(x)+c` với $x=\min(9,k)$ cho đến khi $k$ được xử lý hết.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ để đếm số lần xuất hiện liên tiếp của mỗi ký tự. Giả sử ký tự hiện tại $c$ xuất hiện liên tiếp $k$ lần, ta chia $k$ thành nhiều giá trị $x$, mỗi $x$ không vượt quá $9$, sau đó ghép $x$ và $c$, rồi nối từng cặp $x$ và $c$ vào kết quả.

Cuối cùng, trả về kết quả.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def compressedString(self, word: str) -> str:
        g = groupby(word)
        ans = []
        for c, v in g:
            k = len(list(v))
            while k:
                x = min(9, k)
                ans.append(str(x) + c)
                k -= x
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String compressedString(String word) {
        StringBuilder ans = new StringBuilder();
        int n = word.length();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && word.charAt(j) == word.charAt(i)) {
                ++j;
            }
            int k = j - i;
            while (k > 0) {
                int x = Math.min(9, k);
                ans.append(x).append(word.charAt(i));
                k -= x;
            }
            i = j;
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string compressedString(string word) {
        string ans;
        int n = word.length();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && word[j] == word[i]) {
                ++j;
            }
            int k = j - i;
            while (k > 0) {
                int x = min(9, k);
                ans.push_back('0' + x);
                ans.push_back(word[i]);
                k -= x;
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func compressedString(word string) string {
    ans := []byte{}
    n := len(word)
    for i := 0; i < n; {
        j := i + 1
        for j < n && word[j] == word[i] {
            j++
        }
        k := j - i
        for k > 0 {
            x := min(9, k)
            ans = append(ans, byte('0'+x))
            ans = append(ans, word[i])
            k -= x
        }
        i = j
    }
    return string(ans)
}
```

#### TypeScript

```ts
function compressedString(word: string): string {
    const ans: string[] = [];
    const n = word.length;
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && word[j] === word[i]) {
            ++j;
        }
        let k = j - i;
        while (k) {
            const x = Math.min(k, 9);
            ans.push(x + word[i]);
            k -= x;
        }
        i = j;
    }
    return ans.join('');
}
```

#### JavaScript

```js
/**
 * @param {string} word
 * @return {string}
 */
var compressedString = function (word) {
    const ans = [];
    const n = word.length;
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && word[j] === word[i]) {
            ++j;
        }
        let k = j - i;
        while (k) {
            const x = Math.min(k, 9);
            ans.push(x + word[i]);
            k -= x;
        }
        i = j;
    }
    return ans.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 sử dụng helper để gom nhóm. Hai con trỏ có thể tách khi ký tự thay đổi hoặc khi đoạn liên tiếp đạt độ dài $9$, không cần tạo các danh sách trung gian.
>
> Giữ vị trí bắt đầu đoạn liên tiếp là $j$. Khi $i$ chạm cuối chuỗi, gặp ký tự mới hoặc độ dài đạt $9$, ghi $i-j$ và $word[j]$, sau đó đặt $j=i$.
>
> Quá trình duyệt kết thúc tại $n$. Vẫn có độ phức tạp tuyến tính tương tự, đồng thời sát hơn với yêu cầu “tối đa chín” trong đề bài.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function compressedString(word: string): string {
    let res = '';

    for (let i = 1, j = 0; i <= word.length; i++) {
        if (word[i] !== word[j] || i - j === 9) {
            res += i - j + word[j];
            j = i;
        }
    }

    return res;
}
```

#### JavaScript

```js
function compressedString(word) {
    let res = '';

    for (let i = 1, j = 0; i <= word.length; i++) {
        if (word[i] !== word[j] || i - j === 9) {
            res += i - j + word[j];
            j = i;
        }
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: RegExp

<!-- thinking:start -->

> **Tư duy**
>
> Hai con trỏ vẫn phải tự biểu diễn việc tách đoạn. Mẫu `(.)\1{0,8}` khớp từ một đến chín ký tự giống nhau.
>
> Tìm kiếm toàn cục sẽ lấy trọn từng đoạn như vậy; số lượng chính là độ dài của kết quả khớp.
>
> Nối `len(m[0])` và ký tự đã bắt được. Kết quả giống các phương pháp trước nhưng cần ít phép tính hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function compressedString(word: string): string {
    const regex = /(.)\1{0,8}/g;
    let m: RegExpMatchArray | null = null;
    let res = '';

    while ((m = regex.exec(word))) {
        res += m[0].length + m[1];
    }

    return res;
}
```

#### JavaScript

```js
function compressedString(word) {
    const regex = /(.)\1{0,8}/g;
    let m = null;
    let res = '';

    while ((m = regex.exec(word))) {
        res += m[0].length + m[1];
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
