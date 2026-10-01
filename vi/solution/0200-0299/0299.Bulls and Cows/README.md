---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [299. Bulls and Cows](https://leetcode.com/problems/bulls-and-cows)

[中文文档](/solution/0200-0299/0299.Bulls%20and%20Cows/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi trò <strong><a href="https://en.wikipedia.org/wiki/Bulls_and_Cows" target="_blank">Bulls and Cows</a></strong> với một người bạn.</p>

<p>Bạn viết ra một số bí mật và nhờ người bạn đoán số đó. Mỗi khi bạn của bạn đưa ra một dự đoán, bạn sẽ gợi ý bằng các thông tin sau:</p>

<ul>
	<li>Số lượng &quot;bulls&quot;: các chữ số trong dự đoán nằm đúng vị trí.</li>
	<li>Số lượng &quot;cows&quot;: các chữ số trong dự đoán cũng có trong số bí mật nhưng nằm sai vị trí. Cụ thể, đó là các chữ số trong dự đoán chưa được tính là bull và có thể sắp xếp lại để trở thành bull.</li>
</ul>

<p>Cho số bí mật <code>secret</code> và dự đoán của bạn mình <code>guess</code>, hãy trả về <em>gợi ý cho dự đoán đó</em>.</p>

<p>Gợi ý có định dạng <code>&quot;xAyB&quot;</code>, trong đó <code>x</code> là số lượng bulls và <code>y</code> là số lượng cows. Lưu ý rằng cả <code>secret</code> và <code>guess</code> đều có thể chứa chữ số trùng lặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> secret = &quot;1807&quot;, guess = &quot;7810&quot;
<strong>Đầu ra:</strong> &quot;1A3B&quot;
<strong>Giải thích:</strong> Các bulls được nối với nhau bằng dấu &#39;|&#39;, còn các cows được gạch chân:
&quot;1807&quot;
  |
&quot;<u>7</u>8<u>10</u>&quot;</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> secret = &quot;1123&quot;, guess = &quot;0111&quot;
<strong>Đầu ra:</strong> &quot;1A1B&quot;
<strong>Giải thích:</strong> Các bulls được nối với nhau bằng dấu &#39;|&#39;, còn các cows được gạch chân:
&quot;1123&quot;        &quot;1123&quot;
  |     hoặc    |
&quot;01<u>1</u>1&quot;        &quot;011<u>1</u>&quot;
Lưu ý rằng chỉ một trong hai chữ số 1 chưa khớp được tính là cow, vì các chữ số chưa được tính là bull chỉ có thể sắp xếp lại để một chữ số 1 trở thành bull.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= secret.length, guess.length &lt;= 1000</code></li>
	<li><code>secret.length == guess.length</code></li>
	<li><code>secret</code> và <code>guess</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Bull là các chữ số trùng nhau ở cùng vị trí; cow là các chữ số trùng nhau nhưng ở vị trí khác nhau. Đếm số chữ số khớp đúng vị trí vào $x$, rồi thống kê các chữ số còn lại ở mỗi bên.
>
> Với mỗi chữ số, cộng giá trị nhỏ hơn trong hai số đếm còn lại vào $y$.

<!-- thinking:end -->

Ta tạo hai bộ đếm $cnt1$ và $cnt2$ để đếm số lần xuất hiện của từng chữ số trong số bí mật và trong dự đoán của bạn mình. Đồng thời, ta dùng biến $x$ để đếm số bulls.

Sau đó, ta duyệt số bí mật và dự đoán cùng lúc. Nếu hai chữ số hiện tại giống nhau, ta tăng $x$ lên $1$. Nếu không, ta tăng bộ đếm cho chữ số tương ứng trong số bí mật và trong dự đoán.

Cuối cùng, ta duyệt từng chữ số trong $cnt1$, lấy giá trị nhỏ hơn giữa số đếm của chữ số đó trong $cnt1$ và $cnt2$, rồi cộng giá trị này vào biến $y$.

Cuối cùng, ta trả về các giá trị $x$ và $y$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của số bí mật và dự đoán. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của tập ký tự. Trong bài này, tập ký tự gồm các chữ số nên $|\Sigma| = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getHint(self, secret: str, guess: str) -> str:
        cnt1, cnt2 = Counter(), Counter()
        x = 0
        for a, b in zip(secret, guess):
            if a == b:
                x += 1
            else:
                cnt1[a] += 1
                cnt2[b] += 1
        y = sum(min(cnt1[c], cnt2[c]) for c in cnt1)
        return f"{x}A{y}B"
```

#### Java

```java
class Solution {
    public String getHint(String secret, String guess) {
        int x = 0, y = 0;
        int[] cnt1 = new int[10];
        int[] cnt2 = new int[10];
        for (int i = 0; i < secret.length(); ++i) {
            int a = secret.charAt(i) - '0', b = guess.charAt(i) - '0';
            if (a == b) {
                ++x;
            } else {
                ++cnt1[a];
                ++cnt2[b];
            }
        }
        for (int i = 0; i < 10; ++i) {
            y += Math.min(cnt1[i], cnt2[i]);
        }
        return String.format("%dA%dB", x, y);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getHint(string secret, string guess) {
        int x = 0, y = 0;
        int cnt1[10]{};
        int cnt2[10]{};
        for (int i = 0; i < secret.size(); ++i) {
            int a = secret[i] - '0', b = guess[i] - '0';
            if (a == b) {
                ++x;
            } else {
                ++cnt1[a];
                ++cnt2[b];
            }
        }
        for (int i = 0; i < 10; ++i) {
            y += min(cnt1[i], cnt2[i]);
        }
        return to_string(x) + "A" + to_string(y) + "B";
    }
};
```

#### Go

```go
func getHint(secret string, guess string) string {
	x, y := 0, 0
	cnt1 := [10]int{}
	cnt2 := [10]int{}
	for i, c := range secret {
		a, b := int(c-'0'), int(guess[i]-'0')
		if a == b {
			x++
		} else {
			cnt1[a]++
			cnt2[b]++
		}
	}
	for i, c := range cnt1 {
		y += min(c, cnt2[i])

	}
	return fmt.Sprintf("%dA%dB", x, y)
}
```

#### TypeScript

```ts
function getHint(secret: string, guess: string): string {
    const cnt1: number[] = Array(10).fill(0);
    const cnt2: number[] = Array(10).fill(0);
    let x: number = 0;
    for (let i = 0; i < secret.length; ++i) {
        if (secret[i] === guess[i]) {
            ++x;
        } else {
            ++cnt1[+secret[i]];
            ++cnt2[+guess[i]];
        }
    }
    let y: number = 0;
    for (let i = 0; i < 10; ++i) {
        y += Math.min(cnt1[i], cnt2[i]);
    }
    return `${x}A${y}B`;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $secret
     * @param String $guess
     * @return String
     */
    function getHint($secret, $guess) {
        $cnt1 = array_fill(0, 10, 0);
        $cnt2 = array_fill(0, 10, 0);
        $x = 0;
        for ($i = 0; $i < strlen($secret); ++$i) {
            if ($secret[$i] === $guess[$i]) {
                ++$x;
            } else {
                ++$cnt1[(int) $secret[$i]];
                ++$cnt2[(int) $guess[$i]];
            }
        }
        $y = 0;
        for ($i = 0; $i < 10; ++$i) {
            $y += min($cnt1[$i], $cnt2[$i]);
        }
        return "{$x}A{$y}B";
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
