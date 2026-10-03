---
comments: true
difficulty: Easy
rating: 1253
source: Biweekly Contest 79 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2283. Check if Number Has Equal Digit Count and Digit Value](https://leetcode.com/problems/check-if-number-has-equal-digit-count-and-digit-value)

[中文文档](/solution/2200-2299/2283.Check%20if%20Number%20Has%20Equal%20Digit%20Count%20and%20Digit%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>num</code> có độ dài <code>n</code> và chỉ gồm các chữ số.</p>

<p>Trả về <code>true</code> <em>nếu với <strong>mọi</strong> chỉ số </em><code>i</code><em> trong khoảng </em><code>0 &lt;= i &lt; n</code><em>, chữ số </em><code>i</code><em> xuất hiện </em><code>num[i]</code><em> lần trong </em><code>num</code><em>, ngược lại trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;1210&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
num[0] = &#39;1&#39;. Chữ số 0 xuất hiện một lần trong num.
num[1] = &#39;2&#39;. Chữ số 1 xuất hiện hai lần trong num.
num[2] = &#39;1&#39;. Chữ số 2 xuất hiện một lần trong num.
num[3] = &#39;0&#39;. Chữ số 3 không xuất hiện trong num.
Điều kiện đúng với mọi chỉ số trong &quot;1210&quot;, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;030&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
num[0] = &#39;0&#39;. Chữ số 0 lẽ ra phải xuất hiện 0 lần, nhưng thực tế xuất hiện hai lần trong num.
num[1] = &#39;3&#39;. Chữ số 1 lẽ ra phải xuất hiện ba lần, nhưng thực tế không xuất hiện trong num.
num[2] = &#39;0&#39;. Chữ số 2 không xuất hiện trong num.
Cả chỉ số 0 và 1 đều vi phạm điều kiện, nên trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == num.length</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $num[i]$ phải bằng số lần chữ số $i$ xuất hiện. Chuỗi có độ dài tối đa là $10$, nên ta chỉ cần đếm rồi kiểm tra theo chỉ số.
>
> So sánh một $\textit{Counter}$ của các chữ số với $num[i]$ tại từng vị trí.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{cnt}$ có độ dài $10$ để đếm số lần xuất hiện của mỗi chữ số trong chuỗi $\textit{num}$. Sau đó, ta duyệt qua từng chữ số trong chuỗi $\textit{num}$ và kiểm tra xem số lần xuất hiện của chữ số đó có bằng chính chữ số đó hay không. Nếu điều kiện này đúng với mọi chữ số, ta trả về $\text{true}$; ngược lại, ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(|\Sigma|)$. Ở đây, $n$ là độ dài của chuỗi $\textit{num}$, còn $|\Sigma|$ là miền các giá trị chữ số có thể có, bằng $10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def digitCount(self, num: str) -> bool:
        cnt = Counter(int(x) for x in num)
        return all(cnt[i] == int(x) for i, x in enumerate(num))
```

#### Java

```java
class Solution {
    public boolean digitCount(String num) {
        int[] cnt = new int[10];
        int n = num.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[num.charAt(i) - '0'];
        }
        for (int i = 0; i < n; ++i) {
            if (num.charAt(i) - '0' != cnt[i]) {
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
    bool digitCount(string num) {
        int cnt[10]{};
        for (char& c : num) {
            ++cnt[c - '0'];
        }
        for (int i = 0; i < num.size(); ++i) {
            if (cnt[i] != num[i] - '0') {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func digitCount(num string) bool {
	cnt := [10]int{}
	for _, c := range num {
		cnt[c-'0']++
	}
	for i, c := range num {
		if int(c-'0') != cnt[i] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function digitCount(num: string): boolean {
    const cnt: number[] = Array(10).fill(0);
    for (const c of num) {
        ++cnt[+c];
    }
    for (let i = 0; i < num.length; ++i) {
        if (cnt[i] !== +num[i]) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn digit_count(num: String) -> bool {
        let mut cnt = vec![0; 10];
        for c in num.chars() {
            let x = c.to_digit(10).unwrap() as usize;
            cnt[x] += 1;
        }
        for (i, c) in num.chars().enumerate() {
            let x = c.to_digit(10).unwrap() as usize;
            if cnt[i] != x {
                return false;
            }
        }
        true
    }
}
```

#### C

```c
bool digitCount(char* num) {
    int cnt[10] = {0};
    for (int i = 0; num[i] != '\0'; ++i) {
        ++cnt[num[i] - '0'];
    }
    for (int i = 0; num[i] != '\0'; ++i) {
        if (cnt[i] != num[i] - '0') {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
