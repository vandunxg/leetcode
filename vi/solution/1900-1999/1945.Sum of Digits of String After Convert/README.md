---
comments: true
difficulty: Easy
rating: 1254
source: Weekly Contest 251 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [1945. Sum of Digits of String After Convert](https://leetcode.com/problems/sum-of-digits-of-string-after-convert)

[中文文档](/solution/1900-1999/1945.Sum%20of%20Digits%20of%20String%20After%20Convert/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>k</code>. Nhiệm vụ của bạn là <em>chuyển đổi</em> chuỗi thành một số nguyên bằng một quy trình đặc biệt, sau đó <em>biến đổi</em> số đó bằng cách liên tục tính tổng các chữ số <code>k</code> lần. Cụ thể, hãy thực hiện các bước sau:</p>

<ol>
	<li><strong>Chuyển đổi</strong> <code>s</code> thành một số nguyên bằng cách thay mỗi chữ cái bằng vị trí của nó trong bảng chữ cái (tức là thay <code>&#39;a&#39;</code> bằng <code>1</code>, <code>&#39;b&#39;</code> bằng <code>2</code>, ..., <code>&#39;z&#39;</code> bằng <code>26</code>).</li>
	<li><strong>B</strong><strong>iến đổi</strong> số nguyên bằng cách thay nó bằng <strong>tổng các chữ số</strong> của nó.</li>
	<li>Lặp lại thao tác <strong>biến đổi</strong> (bước 2) tổng cộng <code>k</code><strong> lần</strong>.</li>
</ol>

<p>Ví dụ, nếu <code>s = &quot;zbax&quot;</code> và <code>k = 2</code>, số nguyên nhận được là <code>8</code> qua các thao tác sau:</p>

<ol>
	<li><strong>Chuyển đổi</strong>: <code>&quot;zbax&quot; ➝ &quot;(26)(2)(1)(24)&quot; ➝ &quot;262124&quot; ➝ 262124</code></li>
	<li><strong>Biến đổi #1</strong>: <code>262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17</code></li>
	<li><strong>Biến đổi #2</strong>: <code>17 ➝ 1 + 7 ➝ 8</code></li>
</ol>

<p>Trả về <strong>số nguyên</strong> <strong>nhận được</strong> sau khi thực hiện các <strong>thao tác</strong> được mô tả ở trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;iiii&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">36</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác được thực hiện như sau:<br />
- Chuyển đổi: &quot;iiii&quot; ➝ &quot;(9)(9)(9)(9)&quot; ➝ &quot;9999&quot; ➝ 9999<br />
- Biến đổi #1: 9999 ➝ 9 + 9 + 9 + 9 ➝ 36<br />
Do đó, số nguyên nhận được là 36.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác được thực hiện như sau:<br />
- Chuyển đổi: &quot;leetcode&quot; ➝ &quot;(12)(5)(5)(20)(3)(15)(4)(5)&quot; ➝ &quot;12552031545&quot; ➝ 12552031545<br />
- Biến đổi #1: 12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33<br />
- Biến đổi #2: 33 ➝ 3 + 3 ➝ 6<br />
Do đó, số nguyên nhận được là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zbax&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 10</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ánh xạ các chữ cái thành vị trí của chúng trong bảng chữ cái, sau đó tính tổng các chữ số $k$ lần. Đề bài đã mô tả trực tiếp thuật toán.
>
> Chuỗi sau khi ánh xạ có độ dài $O(n)$; mỗi lần tính tổng các chữ số sẽ làm giá trị thu nhỏ, và sau $k$ vòng lặp, ta chuyển kết quả trở lại thành một số nguyên.

<!-- thinking:end -->

Ta có thể mô phỏng quy trình được mô tả trong đề bài.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLucky(self, s: str, k: int) -> int:
        s = ''.join(str(ord(c) - ord('a') + 1) for c in s)
        for _ in range(k):
            t = sum(int(c) for c in s)
            s = str(t)
        return int(s)
```

#### Java

```java
class Solution {
    public int getLucky(String s, int k) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            sb.append(c - 'a' + 1);
        }
        s = sb.toString();
        while (k-- > 0) {
            int t = 0;
            for (char c : s.toCharArray()) {
                t += c - '0';
            }
            s = String.valueOf(t);
        }
        return Integer.parseInt(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getLucky(string s, int k) {
        string t;
        for (char c : s) {
            t += to_string(c - 'a' + 1);
        }
        s = t;
        while (k--) {
            int t = 0;
            for (char c : s) {
                t += c - '0';
            }
            s = to_string(t);
        }
        return stoi(s);
    }
};
```

#### Go

```go
func getLucky(s string, k int) int {
	var t strings.Builder
	for _, c := range s {
		t.WriteString(strconv.Itoa(int(c - 'a' + 1)))
	}
	s = t.String()
	for k > 0 {
		k--
		t := 0
		for _, c := range s {
			t += int(c - '0')
		}
		s = strconv.Itoa(t)
	}
	ans, _ := strconv.Atoi(s)
	return ans
}
```

#### TypeScript

```ts
function getLucky(s: string, k: number): number {
    let ans = '';
    for (const c of s) {
        ans += c.charCodeAt(0) - 'a'.charCodeAt(0) + 1;
    }
    for (let i = 0; i < k; i++) {
        let t = 0;
        for (const v of ans) {
            t += Number(v);
        }
        ans = `${t}`;
    }
    return Number(ans);
}
```

#### Rust

```rust
impl Solution {
    pub fn get_lucky(s: String, k: i32) -> i32 {
        let mut ans = String::new();
        for c in s.as_bytes() {
            ans.push_str(&(c - b'a' + 1).to_string());
        }
        for _ in 0..k {
            let mut t = 0;
            for c in ans.as_bytes() {
                t += (c - b'0') as i32;
            }
            ans = t.to_string();
        }
        ans.parse().unwrap()
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @param Integer $k
     * @return Integer
     */
    function getLucky($s, $k) {
        $rs = '';
        for ($i = 0; $i < strlen($s); $i++) {
            $num = ord($s[$i]) - 96;
            $rs = $rs . strval($num);
        }
        while ($k != 0) {
            $sum = 0;
            for ($j = 0; $j < strlen($rs); $j++) {
                $sum += intval($rs[$j]);
            }
            $rs = strval($sum);
            $k--;
        }
        return intval($rs);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
