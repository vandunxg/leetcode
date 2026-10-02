---
comments: true
difficulty: Easy
rating: 1321
source: Weekly Contest 220 Q1
tags:
    - String
---

<!-- problem:start -->

# [1694. Reformat Phone Number](https://leetcode.com/problems/reformat-phone-number)

[中文文档](/solution/1600-1699/1694.Reformat%20Phone%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số điện thoại dưới dạng chuỗi <code>number</code>. <code>number</code> gồm các chữ số, dấu cách <code>&#39; &#39;</code> và/hoặc dấu gạch ngang <code>&#39;-&#39;</code>.</p>

<p>Bạn muốn định dạng lại số điện thoại theo một quy tắc nhất định. Đầu tiên, <strong>loại bỏ</strong> tất cả dấu cách và dấu gạch ngang. Sau đó, <strong>nhóm</strong> các chữ số từ trái sang phải thành các khối có độ dài 3 <strong>cho đến khi</strong> còn lại không quá 4 chữ số. Các chữ số cuối cùng được nhóm như sau:</p>

<ul>
	<li>2 chữ số: Một khối có độ dài 2.</li>
	<li>3 chữ số: Một khối có độ dài 3.</li>
	<li>4 chữ số: Hai khối, mỗi khối có độ dài 2.</li>
</ul>

<p>Sau đó, nối các khối bằng dấu gạch ngang. Lưu ý rằng quá trình định dạng lại <strong>không bao giờ</strong> được tạo ra khối có độ dài 1 và <strong>nhiều nhất</strong> chỉ tạo ra hai khối có độ dài 2.</p>

<p>Trả về <em>số điện thoại sau khi định dạng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;1-23-45 6&quot;
<strong>Đầu ra:</strong> &quot;123-456&quot;
<strong>Giải thích:</strong> Các chữ số là &quot;123456&quot;.
Bước 1: Có nhiều hơn 4 chữ số, nên nhóm 3 chữ số tiếp theo. Khối thứ nhất là &quot;123&quot;.
Bước 2: Còn lại 3 chữ số, nên đặt chúng vào một khối có độ dài 3. Khối thứ hai là &quot;456&quot;.
Nối các khối ta được &quot;123-456&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;123 4-567&quot;
<strong>Đầu ra:</strong> &quot;123-45-67&quot;
<strong>Giải thích: </strong>Các chữ số là &quot;1234567&quot;.
Bước 1: Có nhiều hơn 4 chữ số, nên nhóm 3 chữ số tiếp theo. Khối thứ nhất là &quot;123&quot;.
Bước 2: Còn lại 4 chữ số, nên chia chúng thành hai khối có độ dài 2. Hai khối là &quot;45&quot; và &quot;67&quot;.
Nối các khối ta được &quot;123-45-67&quot;.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> number = &quot;123 4-5678&quot;
<strong>Đầu ra:</strong> &quot;123-456-78&quot;
<strong>Giải thích:</strong> Các chữ số là &quot;12345678&quot;.
Bước 1: Khối thứ nhất là &quot;123&quot;.
Bước 2: Khối thứ hai là &quot;456&quot;.
Bước 3: Còn lại 2 chữ số, nên đặt chúng vào một khối có độ dài 2. Khối thứ ba là &quot;78&quot;.
Nối các khối ta được &quot;123-456-78&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= number.length &lt;= 100</code></li>
	<li><code>number</code> gồm các chữ số và các ký tự <code>&#39;-&#39;</code> và <code>&#39; &#39;</code>.</li>
<li>Có ít nhất <strong>hai</strong> chữ số trong <code>number</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng đơn giản

<!-- thinking:start -->

> **Tư duy**
>
> Loại bỏ dấu cách và dấu gạch ngang, sau đó nhóm thành các khối ba chữ số. Nếu còn dư một chữ số, biến hai khối cuối thành $2+2$; nếu còn dư hai chữ số, tạo một khối riêng.
>
> Sau khi làm sạch, cắt chuỗi thành từng nhóm ba chữ số, xử lý phần dư rồi nối các nhóm bằng dấu gạch ngang.

<!-- thinking:end -->

Trước hết, theo mô tả bài toán, ta loại bỏ tất cả dấu cách và dấu gạch ngang khỏi chuỗi.

Gọi độ dài hiện tại của chuỗi là $n$. Sau đó, ta duyệt chuỗi từ đầu, gom mỗi $3$ ký tự thành một nhóm và thêm vào chuỗi kết quả. Ta có tổng cộng $n / 3$ nhóm.

Nếu cuối chuỗi còn lại $1$ ký tự, ta lấy ký tự cuối của nhóm cuối cùng để tạo một nhóm mới gồm hai ký tự với ký tự này, rồi thêm vào chuỗi kết quả. Nếu còn $2$ ký tự, ta trực tiếp tạo một nhóm mới từ hai ký tự đó và thêm vào chuỗi kết quả.

Cuối cùng, ta thêm dấu gạch ngang giữa các nhóm và trả về chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reformatNumber(self, number: str) -> str:
        number = number.replace("-", "").replace(" ", "")
        n = len(number)
        ans = [number[i * 3 : i * 3 + 3] for i in range(n // 3)]
        if n % 3 == 1:
            ans[-1] = ans[-1][:2]
            ans.append(number[-2:])
        elif n % 3 == 2:
            ans.append(number[-2:])
        return "-".join(ans)
```

#### Java

```java
class Solution {
    public String reformatNumber(String number) {
        number = number.replace("-", "").replace(" ", "");
        int n = number.length();
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < n / 3; ++i) {
            ans.add(number.substring(i * 3, i * 3 + 3));
        }
        if (n % 3 == 1) {
            ans.set(ans.size() - 1, ans.get(ans.size() - 1).substring(0, 2));
            ans.add(number.substring(n - 2));
        } else if (n % 3 == 2) {
            ans.add(number.substring(n - 2));
        }
        return String.join("-", ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reformatNumber(string number) {
        string s;
        for (char c : number) {
            if (c != ' ' && c != '-') {
                s.push_back(c);
            }
        }
        int n = s.size();
        vector<string> res;
        for (int i = 0; i < n / 3; ++i) {
            res.push_back(s.substr(i * 3, 3));
        }
        if (n % 3 == 1) {
            res.back() = res.back().substr(0, 2);
            res.push_back(s.substr(n - 2));
        } else if (n % 3 == 2) {
            res.push_back(s.substr(n - 2));
        }
        string ans;
        for (auto& v : res) {
            ans += v;
            ans += "-";
        }
        ans.pop_back();
        return ans;
    }
};
```

#### Go

```go
func reformatNumber(number string) string {
	number = strings.ReplaceAll(number, " ", "")
	number = strings.ReplaceAll(number, "-", "")
	n := len(number)
	ans := []string{}
	for i := 0; i < n/3; i++ {
		ans = append(ans, number[i*3:i*3+3])
	}
	if n%3 == 1 {
		ans[len(ans)-1] = ans[len(ans)-1][:2]
		ans = append(ans, number[n-2:])
	} else if n%3 == 2 {
		ans = append(ans, number[n-2:])
	}
	return strings.Join(ans, "-")
}
```

#### TypeScript

```ts
function reformatNumber(number: string): string {
    const cs = [...number].filter(c => c !== ' ' && c !== '-');
    const n = cs.length;
    return cs
        .map((v, i) => {
            if (((i + 1) % 3 === 0 && i < n - 2) || (n % 3 === 1 && n - 3 === i)) {
                return v + '-';
            }
            return v;
        })
        .join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn reformat_number(number: String) -> String {
        let cs: Vec<char> = number.chars().filter(|&c| c != ' ' && c != '-').collect();
        let n = cs.len();
        cs.iter()
            .enumerate()
            .map(|(i, c)| {
                if ((i + 1) % 3 == 0 && i < n - 2) || (n % 3 == 1 && i == n - 3) {
                    return c.to_string() + &"-";
                }
                c.to_string()
            })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
