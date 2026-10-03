---
comments: true
difficulty: Medium
rating: 1577
source: Weekly Contest 295 Q2
tags:
    - String
---

<!-- problem:start -->

# [2288. Apply Discount to Prices](https://leetcode.com/problems/apply-discount-to-prices)

[中文文档](/solution/2200-2299/2288.Apply%20Discount%20to%20Prices/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Câu</strong> là một chuỗi gồm các từ được ngăn cách bằng một dấu cách, trong đó mỗi từ có thể chứa chữ số, chữ cái thường và ký hiệu đô la <code>&#39;$&#39;</code>. Một từ biểu diễn một <strong>giá tiền</strong> nếu đó là một dãy chữ số đứng sau ký hiệu đô la.</p>

<ul>
	<li>Ví dụ, <code>&quot;$100&quot;</code>, <code>&quot;$23&quot;</code> và <code>&quot;$6&quot;</code> biểu diễn các giá tiền, còn <code>&quot;100&quot;</code>, <code>&quot;$&quot;</code> và <code>&quot;$1e5&quot;</code> thì không.</li>
</ul>

<p>Cho một chuỗi <code>sentence</code> biểu diễn một câu và một số nguyên <code>discount</code>. Với mỗi từ biểu diễn một giá tiền, áp dụng mức giảm <code>discount%</code> cho giá tiền đó và <strong>cập nhật</strong> từ tương ứng trong câu. Tất cả giá tiền sau khi cập nhật phải được biểu diễn với <strong>chính xác hai</strong> chữ số thập phân.</p>

<p>Trả về <em>chuỗi biểu diễn câu sau khi được thay đổi</em>.</p>

<p>Lưu ý rằng mỗi giá tiền có <strong>nhiều nhất</strong> <code>10</code> chữ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;there are $1 $2 and 5$ candies in the shop&quot;, discount = 50
<strong>Đầu ra:</strong> &quot;there are $0.50 $1.00 and 5$ candies in the shop&quot;
<strong>Giải thích:</strong>
Các từ biểu diễn giá tiền là &quot;$1&quot; và &quot;$2&quot;.
- Giảm 50% cho &quot;$1&quot; yields &quot;$0.50&quot;, nên &quot;$1&quot; is replaced by &quot;$0.50&quot;.
- Giảm 50% cho &quot;$2&quot; yields &quot;$1&quot;. Vì cần có chính xác 2 chữ số thập phân sau giá tiền, ta thay &quot;$2&quot; with &quot;$1.00&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;1 2 $3 4 $5 $6 7 8$ $9 $10$&quot;, discount = 100
<strong>Đầu ra:</strong> &quot;1 2 $0.00 4 $0.00 $0.00 7 8$ $0.00 $10$&quot;
<strong>Giải thích:</strong>
Áp dụng mức giảm 100% cho bất kỳ giá tiền nào cũng cho kết quả 0.
Các từ biểu diễn giá tiền là &quot;$3&quot;, &quot;$5&quot;, &quot;$6&quot;, and &quot;$9&quot;.
Mỗi từ được thay bằng &quot;$0.00&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 10<sup>5</sup></code></li>
	<li><code>sentence</code> chỉ gồm các chữ cái tiếng Anh thường, chữ số, <code>&#39; &#39;</code> và <code>&#39;$&#39;</code>.</li>
	<li><code>sentence</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li>Tất cả các từ trong <code>sentence</code> được ngăn cách bằng một dấu cách.</li>
	<li>Tất cả giá tiền là các số <strong>dương</strong> không có số 0 ở đầu.</li>
	<li>Tất cả giá tiền có <strong>nhiều nhất</strong> <code>10</code> chữ số.</li>
	<li><code>0 &lt;= discount &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một token có dạng $\texttt{\$}$ followed by a positive integer is a price and should be discounted to two decimals. The sentence length is $10^5$, nên chỉ cần tách câu theo dấu cách.
>
> Thay thế một token khi token bắt đầu bằng $\texttt{\$}$ và phần còn lại chỉ gồm các chữ số, sau đó ghép các token lại.

<!-- thinking:end -->

Ta có thể tách sentence thành một mảng các từ theo dấu cách, sau đó duyệt qua mảng các từ. Với mỗi từ, nếu từ đó biểu diễn một giá tiền, ta cập nhật nó thành giá tiền sau khi áp dụng mức giảm. Cuối cùng, nối các từ đã cập nhật thành một chuỗi, trong đó các từ được ngăn cách bằng dấu cách.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi `sentence`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def discountPrices(self, sentence: str, discount: int) -> str:
        ans = []
        for w in sentence.split():
            if w[0] == '$' and w[1:].isdigit():
                w = f'${int(w[1:]) * (1 - discount / 100):.2f}'
            ans.append(w)
        return ' '.join(ans)
```

#### Java

```java
class Solution {
    public String discountPrices(String sentence, int discount) {
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; ++i) {
            if (check(words[i])) {
                double t = Long.parseLong(words[i].substring(1)) * (1 - discount / 100.0);
                words[i] = String.format("$%.2f", t);
            }
        }
        return String.join(" ", words);
    }

    private boolean check(String s) {
        if (s.charAt(0) != '$' || s.length() == 1) {
            return false;
        }
        for (int i = 1; i < s.length(); ++i) {
            if (!Character.isDigit(s.charAt(i))) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string discountPrices(string sentence, int discount) {
        istringstream is(sentence);
        string w;
        string ans;
        auto check = [](string s) {
            if (s[0] != '$' || s.size() == 1) {
                return false;
            }
            for (int i = 1; i < s.size(); ++i) {
                if (!isdigit(s[i])) {
                    return false;
                }
            }
            return true;
        };
        while (is >> w) {
            if (check(w)) {
                long long v = stoll(w.substr(1)) * (100 - discount);
                char t[20];
                sprintf(t, "$%lld.%02lld", v / 100, v % 100);
                ans += t;
            } else {
                ans += w;
            }
            ans += ' ';
        }
        ans.pop_back();
        return ans;
    }
};
```

#### Go

```go
func discountPrices(sentence string, discount int) string {
	words := strings.Split(sentence, " ")
	for i, w := range words {
		if w[0] == '$' {
			if v, err := strconv.Atoi(w[1:]); err == nil {
				words[i] = fmt.Sprintf("$%.2f", float64(v*(100-discount))/100)
			}
		}
	}
	return strings.Join(words, " ")
}
```

#### TypeScript

```ts
function discountPrices(sentence: string, discount: number): string {
    const sell = (100 - discount) / 100;
    const reg = new RegExp(/^(\$)(([1-9]\d*\.?\d*)|(0\.\d*))$/g);
    const words = sentence.split(' ').map(d => {
        if (!reg.test(d)) return d;
        return d.replace(reg, (s, $1, $2) => {
            return `$${(sell * $2).toFixed(2)}`;
        });
    });
    return words.join(' ');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
