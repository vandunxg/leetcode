---
comments: true
difficulty: Easy
rating: 1204
source: Weekly Contest 242 Q1
tags:
    - String
---

<!-- problem:start -->

# [1869. Longer Contiguous Segments of Ones than Zeros](https://leetcode.com/problems/longer-contiguous-segments-of-ones-than-zeros)

[中文文档](/solution/1800-1899/1869.Longer%20Contiguous%20Segments%20of%20Ones%20than%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code>, hãy trả về <code>true</code><em> nếu đoạn liên tiếp <strong>dài nhất</strong> gồm các </em><code>1</code>&#39;<em> dài hơn <strong>nghiêm ngặt</strong> đoạn liên tiếp <strong>dài nhất</strong> gồm các </em><code>0</code>&#39;<em> trong </em><code>s</code>, hoặc trả về <code>false</code><em> trong trường hợp ngược lại</em>.</p>

<ul>
	<li>Ví dụ, trong <code>s = &quot;<u>11</u>01<u>000</u>10&quot;</code>, đoạn liên tiếp dài nhất gồm các <code>1</code> có độ dài <code>2</code>, còn đoạn liên tiếp dài nhất gồm các <code>0</code> có độ dài <code>3</code>.</li>
</ul>

<p>Lưu ý rằng nếu không có <code>0</code> nào, đoạn liên tiếp dài nhất gồm các <code>0</code> được xem là có độ dài <code>0</code>. Điều tương tự cũng áp dụng khi không có <code>1</code> nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1101&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Đoạn liên tiếp dài nhất gồm các số 1 có độ dài 2: &quot;<u>11</u>01&quot;
Đoạn liên tiếp dài nhất gồm các số 0 có độ dài 1: &quot;11<u>0</u>1&quot;
Đoạn gồm các số 1 dài hơn, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;111000&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Đoạn liên tiếp dài nhất gồm các số 1 có độ dài 3: &quot;<u>111</u>000&quot;
Đoạn liên tiếp dài nhất gồm các số 0 có độ dài 3: &quot;111<u>000</u>&quot;
Đoạn gồm các số 1 không dài hơn, nên trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;110100010&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Đoạn liên tiếp dài nhất gồm các số 1 có độ dài 2: &quot;<u>11</u>0100010&quot;
Đoạn liên tiếp dài nhất gồm các số 0 có độ dài 3: &quot;1101<u>000</u>10&quot;
Đoạn gồm các số 1 không dài hơn, nên trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> So sánh đoạn liên tiếp dài nhất của các số 1 với đoạn liên tiếp dài nhất của các số 0. Có thể theo dõi cả hai trong một lượt duyệt, nhưng dùng hai lần gọi hàm sẽ rõ ràng hơn.
>
> $f(x)$ duyệt $s$ và ghi nhận đoạn liên tiếp dài nhất của $x$. Đáp án là liệu $f(1)>f(0)$ có đúng hay không.

<!-- thinking:end -->

Ta xây dựng hàm $f(x)$, biểu diễn độ dài của chuỗi con liên tiếp dài nhất trong chuỗi $s$ được tạo bởi $x$. Nếu $f(1) > f(0)$, trả về `true`; ngược lại, trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkZeroOnes(self, s: str) -> bool:
        def f(x: str) -> int:
            cnt = mx = 0
            for c in s:
                if c == x:
                    cnt += 1
                    mx = max(mx, cnt)
                else:
                    cnt = 0
            return mx

        return f("1") > f("0")
```

#### Java

```java
class Solution {
    public boolean checkZeroOnes(String s) {
        return f(s, '1') > f(s, '0');
    }

    private int f(String s, char x) {
        int cnt = 0, mx = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == x) {
                mx = Math.max(mx, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkZeroOnes(string s) {
        auto f = [&](char x) {
            int cnt = 0, mx = 0;
            for (char& c : s) {
                if (c == x) {
                    mx = max(mx, ++cnt);
                } else {
                    cnt = 0;
                }
            }
            return mx;
        };
        return f('1') > f('0');
    }
};
```

#### Go

```go
func checkZeroOnes(s string) bool {
	f := func(x rune) int {
		cnt, mx := 0, 0
		for _, c := range s {
			if c == x {
				cnt++
				mx = max(mx, cnt)
			} else {
				cnt = 0
			}
		}
		return mx
	}
	return f('1') > f('0')
}
```

#### TypeScript

```ts
function checkZeroOnes(s: string): boolean {
    const f = (x: string): number => {
        let [mx, cnt] = [0, 0];
        for (const c of s) {
            if (c === x) {
                mx = Math.max(mx, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return mx;
    };
    return f('1') > f('0');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var checkZeroOnes = function (s) {
    const f = x => {
        let [mx, cnt] = [0, 0];
        for (const c of s) {
            if (c === x) {
                mx = Math.max(mx, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return mx;
    };
    return f('1') > f('0');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
