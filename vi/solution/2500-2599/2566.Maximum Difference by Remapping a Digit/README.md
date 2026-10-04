---
comments: true
difficulty: Easy
rating: 1396
source: Biweekly Contest 98 Q1
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2566. Maximum Difference by Remapping a Digit](https://leetcode.com/problems/maximum-difference-by-remapping-a-digit)

[中文文档](/solution/2500-2599/2566.Maximum%20Difference%20by%20Remapping%20a%20Digit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num</code>. Bạn biết rằng Bob sẽ lén <strong>ánh xạ lại</strong> một trong <code>10</code> chữ số có thể có (từ <code>0</code> đến <code>9</code>) thành một chữ số khác.</p>

<p>Hãy trả về <em>hiệu giữa giá trị lớn nhất và nhỏ nhất mà Bob có thể tạo ra bằng cách ánh xạ lại <strong>chính xác</strong> <strong>một</strong> chữ số trong </em><code>num</code>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Khi Bob ánh xạ chữ số <font face="monospace">d1</font>&nbsp;thành chữ số khác <font face="monospace">d2</font>, Bob thay thế tất cả các lần xuất hiện của <code>d1</code>&nbsp;trong <code>num</code>&nbsp;bằng <code>d2</code>.</li>
	<li>Bob có thể ánh xạ một chữ số thành chính nó, khi đó <code>num</code>&nbsp;không thay đổi.</li>
	<li>Bob có thể ánh xạ các chữ số khác nhau để lần lượt thu được giá trị nhỏ nhất và lớn nhất.</li>
	<li>Số thu được sau khi ánh xạ lại có thể chứa các số 0 ở đầu.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 11891
<strong>Đầu ra:</strong> 99009
<strong>Giải thích:</strong>
Để đạt giá trị lớn nhất, Bob có thể ánh xạ chữ số 1 thành chữ số 9, thu được 99899.
Để đạt giá trị nhỏ nhất, Bob có thể ánh xạ chữ số 1 thành chữ số 0, thu được 890.
Hiệu giữa hai số này là 99009.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 90
<strong>Đầu ra:</strong> 99
<strong>Giải thích:</strong>
Giá trị lớn nhất mà hàm có thể trả về là 99 (nếu thay 0 bằng 9), còn giá trị nhỏ nhất là 0 (nếu thay 9 bằng 0).
Do đó, ta trả về 99.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ánh xạ một chữ số trong toàn bộ số để lần lượt tối đa hóa và tối thiểu hóa một giá trị, sau đó lấy hiệu. Có thể duyệt tất cả các cặp chữ số vì chỉ có ít chữ số, nhưng phép ánh xạ tối ưu là duy nhất.
>
> Để có giá trị nhỏ nhất, ta thay mọi bản sao của chữ số đầu tiên bằng $0$. Để có giá trị lớn nhất, ta thay mọi bản sao của chữ số khác $9$ ở ngoài cùng bên trái bằng $9$. Một số chỉ gồm các chữ số $9$ vốn đã đạt giá trị lớn nhất.

<!-- thinking:end -->

Trước hết, ta chuyển số thành một chuỗi $s$.

Để tìm giá trị nhỏ nhất, ta chỉ cần tìm chữ số đầu tiên $s[0]$ trong chuỗi $s$, sau đó thay tất cả $s[0]$ trong chuỗi bằng $0$.

Để tìm giá trị lớn nhất, ta cần tìm chữ số đầu tiên $s[i]$ trong chuỗi $s$ khác $9$, sau đó thay tất cả $s[i]$ trong chuỗi bằng $9$.

Cuối cùng, trả về hiệu giữa giá trị lớn nhất và nhỏ nhất.

Độ phức tạp thời gian là $O(\log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là kích thước của số $\textit{num}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMaxDifference(self, num: int) -> int:
        s = str(num)
        mi = int(s.replace(s[0], '0'))
        for c in s:
            if c != '9':
                return int(s.replace(c, '9')) - mi
        return num - mi
```

#### Java

```java
class Solution {
    public int minMaxDifference(int num) {
        String s = String.valueOf(num);
        int mi = Integer.parseInt(s.replace(s.charAt(0), '0'));
        for (char c : s.toCharArray()) {
            if (c != '9') {
                return Integer.parseInt(s.replace(c, '9')) - mi;
            }
        }
        return num - mi;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMaxDifference(int num) {
        string s = to_string(num);
        string t = s;
        char first = s[0];
        for (char& c : s) {
            if (c == first) {
                c = '0';
            }
        }
        int mi = stoi(s);
        for (int i = 0; i < t.size(); ++i) {
            if (t[i] != '9') {
                char second = t[i];
                for (int j = i; j < t.size(); ++j) {
                    if (t[j] == second) {
                        t[j] = '9';
                    }
                }
                return stoi(t) - mi;
            }
        }
        return num - mi;
    }
};
```

#### Go

```go
func minMaxDifference(num int) int {
	s := []byte(strconv.Itoa(num))
	first := s[0]
	for i := range s {
		if s[i] == first {
			s[i] = '0'
		}
	}
	mi, _ := strconv.Atoi(string(s))
	t := []byte(strconv.Itoa(num))
	for i := range t {
		if t[i] != '9' {
			second := t[i]
			for j := i; j < len(t); j++ {
				if t[j] == second {
					t[j] = '9'
				}
			}
			mx, _ := strconv.Atoi(string(t))
			return mx - mi
		}
	}
	return num - mi
}
```

#### TypeScript

```ts
function minMaxDifference(num: number): number {
    const s = num.toString();
    const mi = +s.replaceAll(s[0], '0');
    for (const c of s) {
        if (c !== '9') {
            const mx = +s.replaceAll(c, '9');
            return mx - mi;
        }
    }
    return num - mi;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_max_difference(num: i32) -> i32 {
        let s = num.to_string();
        let mi = s.replace(s.chars().next().unwrap(), "0").parse::<i32>().unwrap();
        for c in s.chars() {
            if c != '9' {
                let mx = s.replace(c, "9").parse::<i32>().unwrap();
                return mx - mi;
            }
        }
        num - mi
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number}
 */
var minMaxDifference = function (num) {
    const s = num.toString();
    const mi = +s.replaceAll(s[0], '0');
    for (const c of s) {
        if (c !== '9') {
            const mx = +s.replaceAll(c, '9');
            return mx - mi;
        }
    }
    return num - mi;
};
```

#### C

```c
int minMaxDifference(int num) {
    char s[12];
    sprintf(s, "%d", num);

    int mi;
    {
        char tmp[12];
        char t = s[0];
        for (int i = 0; s[i]; i++) {
            tmp[i] = (s[i] == t) ? '0' : s[i];
        }
        tmp[strlen(s)] = '\0';
        mi = atoi(tmp);
    }

    for (int i = 0; s[i]; i++) {
        char c = s[i];
        if (c != '9') {
            char tmp[12];
            for (int j = 0; s[j]; j++) {
                tmp[j] = (s[j] == c) ? '9' : s[j];
            }
            tmp[strlen(s)] = '\0';
            return atoi(tmp) - mi;
        }
    }

    return num - mi;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
