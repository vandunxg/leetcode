---
comments: true
difficulty: Easy
rating: 1331
source: Weekly Contest 291 Q1
tags:
    - Greedy
    - String
    - Enumeration
---

<!-- problem:start -->

# [2259. Remove Digit From Number to Maximize Result](https://leetcode.com/problems/remove-digit-from-number-to-maximize-result)

[中文文档](/solution/2200-2299/2259.Remove%20Digit%20From%20Number%20to%20Maximize%20Result/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>number</code> biểu diễn một <strong>số nguyên dương</strong> và một ký tự <code>digit</code>.</p>

<p>Hãy trả về <em>chuỗi kết quả sau khi xóa <strong>đúng một lần xuất hiện</strong> của </em><code>digit</code><em> khỏi </em><code>number</code><em> sao cho giá trị của chuỗi kết quả ở dạng <strong>thập phân</strong> là <strong>lớn nhất</strong></em>. Các trường hợp kiểm thử được tạo sao cho <code>digit</code> xuất hiện ít nhất một lần trong <code>number</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;123&quot;, digit = &quot;3&quot;
<strong>Đầu ra:</strong> &quot;12&quot;
<strong>Giải thích:</strong> Có duy nhất một &#39;3&#39; trong &quot;123&quot;. Sau khi xóa &#39;3&#39;, kết quả là &quot;12&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;1231&quot;, digit = &quot;1&quot;
<strong>Đầu ra:</strong> &quot;231&quot;
<strong>Giải thích:</strong> Ta có thể xóa &#39;1&#39; đầu tiên để được &quot;231&quot; hoặc xóa &#39;1&#39; thứ hai để được &quot;123&quot;.
Vì 231 &gt; 123, ta trả về &quot;231&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;551&quot;, digit = &quot;5&quot;
<strong>Đầu ra:</strong> &quot;51&quot;
<strong>Giải thích:</strong> Ta có thể xóa &#39;5&#39; thứ nhất hoặc thứ hai khỏi &quot;551&quot;.
Cả hai cách đều cho chuỗi &quot;51&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= number.length &lt;= 100</code></li>
	<li><code>number</code> chỉ gồm các chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li><code>digit</code> là một chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li><code>digit</code> xuất hiện ít nhất một lần trong <code>number</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải xóa đúng một lần xuất hiện của chữ số đã cho để tối đa hóa chuỗi thập phân còn lại. Độ dài chuỗi không quá $100$, nên ta có thể tạo chuỗi cho mỗi cách xóa.
>
> Duyệt các chỉ số $i$ chứa $\textit{digit}$ và lấy giá trị lớn nhất của $number[:i]+number[i+1:]$. Thứ tự từ điển trùng với thứ tự số.

<!-- thinking:end -->

Ta có thể duyệt qua tất cả vị trí $\textit{i}$ trong chuỗi $\textit{number}$. Nếu $\textit{number}[i] = \textit{digit}$, ta lấy tiền tố $\textit{number}[0:i]$ và hậu tố $\textit{number}[i+1:]$ của $\textit{number}$, rồi nối chúng lại. Đây là kết quả sau khi xóa $\textit{number}[i]$. Sau đó, ta lấy giá trị lớn nhất trong tất cả kết quả có thể tạo ra.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài chuỗi $\textit{number}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDigit(self, number: str, digit: str) -> str:
        return max(
            number[:i] + number[i + 1 :] for i, d in enumerate(number) if d == digit
        )
```

#### Java

```java
class Solution {
    public String removeDigit(String number, char digit) {
        String ans = "0";
        for (int i = 0, n = number.length(); i < n; ++i) {
            char d = number.charAt(i);
            if (d == digit) {
                String t = number.substring(0, i) + number.substring(i + 1);
                if (ans.compareTo(t) < 0) {
                    ans = t;
                }
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
    string removeDigit(string number, char digit) {
        string ans = "0";
        for (int i = 0, n = number.size(); i < n; ++i) {
            char d = number[i];
            if (d == digit) {
                string t = number.substr(0, i) + number.substr(i + 1, n - i);
                if (ans < t) {
                    ans = t;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeDigit(number string, digit byte) string {
	ans := "0"
	for i, d := range number {
		if d == rune(digit) {
			t := number[:i] + number[i+1:]
			if strings.Compare(ans, t) < 0 {
				ans = t
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function removeDigit(number: string, digit: string): string {
    const n = number.length;
    let last = -1;
    for (let i = 0; i < n; ++i) {
        if (number[i] === digit) {
            last = i;
            if (i + 1 < n && number[i] < number[i + 1]) {
                break;
            }
        }
    }
    return number.substring(0, last) + number.substring(last + 1);
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $number
     * @param String $digit
     * @return String
     */
    function removeDigit($number, $digit) {
        $max = 0;
        for ($i = 0; $i < strlen($number); $i++) {
            if ($number[$i] == $digit) {
                $tmp = substr($number, 0, $i) . substr($number, $i + 1);
                if ($tmp > $max) {
                    $max = $tmp;
                }
            }
        }
        return $max;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo ra nhiều chuỗi. Khi duyệt từ trái sang phải, nếu một lần xuất hiện của $\textit{digit}$ nhỏ hơn ký tự tiếp theo, xóa nó sẽ đưa một chữ số có hàng cao hơn lên trước và là lựa chọn tối ưu. Nếu không, ta xóa lần xuất hiện cuối cùng để một chữ số lớn không bị thay thế bởi chữ số nhỏ hơn.
>
> Theo dõi chỉ số cuối cùng và dừng sớm khi gặp một cặp tăng nghiêm ngặt. Cách duyệt này có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta có thể duyệt qua tất cả vị trí $\textit{i}$ trong chuỗi $\textit{number}$. Nếu $\textit{number}[i] = \textit{digit}$, ta lưu vị trí xuất hiện cuối cùng của $\textit{digit}$ vào $\textit{last}$. Nếu $\textit{i} + 1 < \textit{n}$ và $\textit{number}[i] < \textit{number}[i + 1]$, ta có thể trả về ngay $\textit{number}[0:i] + \textit{number}[i+1:]$ là kết quả sau khi xóa $\textit{number}[i]$. Bởi vì nếu $\textit{number}[i] < \textit{number}[i + 1]$, việc xóa $\textit{number}[i]$ sẽ tạo ra một số lớn hơn.

Sau khi duyệt xong, ta trả về $\textit{number}[0:\textit{last}] + \textit{number}[\textit{last}+1:]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDigit(self, number: str, digit: str) -> str:
        last = -1
        n = len(number)
        for i, d in enumerate(number):
            if d == digit:
                last = i
                if i + 1 < n and d < number[i + 1]:
                    break
        return number[:last] + number[last + 1 :]
```

#### Java

```java
class Solution {
    public String removeDigit(String number, char digit) {
        int last = -1;
        int n = number.length();
        for (int i = 0; i < n; ++i) {
            char d = number.charAt(i);
            if (d == digit) {
                last = i;
                if (i + 1 < n && d < number.charAt(i + 1)) {
                    break;
                }
            }
        }
        return number.substring(0, last) + number.substring(last + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeDigit(string number, char digit) {
        int n = number.size();
        int last = -1;
        for (int i = 0; i < n; ++i) {
            char d = number[i];
            if (d == digit) {
                last = i;
                if (i + 1 < n && number[i] < number[i + 1]) {
                    break;
                }
            }
        }
        return number.substr(0, last) + number.substr(last + 1);
    }
};
```

#### Go

```go
func removeDigit(number string, digit byte) string {
	last := -1
	n := len(number)
	for i := range number {
		if number[i] == digit {
			last = i
			if i+1 < n && number[i] < number[i+1] {
				break
			}
		}
	}
	return number[:last] + number[last+1:]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
