---
comments: true
difficulty: Medium
rating: 2004
source: Biweekly Contest 56 Q3
tags:
    - Greedy
    - Math
    - String
    - Game Theory
---

<!-- problem:start -->

# [1927. Sum Game](https://leetcode.com/problems/sum-game)

[中文文档](/solution/1900-1999/1927.Sum%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, trong đó <strong>Alice</strong><strong>&nbsp;đi trước</strong>.</p>

<p>Cho một chuỗi <code>num</code> có <strong>độ dài chẵn</strong>, chỉ gồm các chữ số và ký tự <code>&#39;?&#39;</code>. Ở mỗi lượt, nếu trong <code>num</code> vẫn còn ít nhất một ký tự <code>&#39;?&#39;</code>, người chơi sẽ thực hiện các bước sau:</p>

<ol>
	<li>Chọn một chỉ số <code>i</code> sao cho <code>num[i] == &#39;?&#39;</code>.</li>
	<li>Thay <code>num[i]</code> bằng một chữ số bất kỳ từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ol>

<p>Trò chơi kết thúc khi không còn ký tự <code>&#39;?&#39;</code> nào trong <code>num</code>.</p>

<p>Để Bob thắng, tổng các chữ số trong nửa đầu của <code>num</code> phải <strong>bằng</strong> tổng các chữ số trong nửa sau. Để Alice thắng, hai tổng phải <strong>khác nhau</strong>.</p>

<ul>
	<li>Ví dụ, nếu trò chơi kết thúc với <code>num = &quot;243801&quot;</code>, Bob thắng vì <code>2+4+3 = 8+0+1</code>. Nếu trò chơi kết thúc với <code>num = &quot;243803&quot;</code>, Alice thắng vì <code>2+4+3 != 8+0+3</code>.</li>
</ul>

<p>Giả sử Alice và Bob đều chơi <strong>tối ưu</strong>, hãy trả về <code>true</code> <em>nếu Alice thắng và </em><code>false</code> <em>nếu Bob thắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;5023&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có nước đi nào được thực hiện.
Tổng của nửa đầu bằng tổng của nửa sau: 5 + 0 = 2 + 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;25??&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Alice có thể thay một trong hai ký tự &#39;?&#39; bằng &#39;9&#39;, khi đó Bob không thể làm cho hai tổng bằng nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;?3295???&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể chứng minh Bob luôn thắng. Một kết quả có thể xảy ra là:
- Alice thay ký tự &#39;?&#39; đầu tiên bằng &#39;9&#39;. num = &quot;93295???&quot;.
- Bob thay một trong các ký tự &#39;?&#39; ở nửa bên phải bằng &#39;9&#39;. num = &quot;932959??&quot;.
- Alice thay một trong các ký tự &#39;?&#39; ở nửa bên phải bằng &#39;2&#39;. num = &quot;9329592?&quot;.
- Bob thay ký tự &#39;?&#39; cuối cùng ở nửa bên phải bằng &#39;7&#39;. num = &quot;93295927&quot;.
Bob thắng vì 9 + 3 + 2 + 9 = 5 + 9 + 2 + 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num.length</code> là <strong>số chẵn</strong>.</li>
	<li><code>num</code> chỉ gồm các chữ số và ký tự <code>&#39;?&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Alice muốn hai nửa có tổng khác nhau. Cây trò chơi có số nhánh tăng theo cấp số mũ theo số lượng dấu hỏi, nhưng khi chơi tối ưu, bài toán được rút gọn thành tính chẵn lẻ và độ lệch số học.
>
> Khi số lượng `?` là lẻ, Alice được điền thêm một ô và có thể buộc hai tổng khác nhau. Khi số lượng này là chẵn, các dấu còn lại có thể được ghép cặp; Bob có thể làm hai tổng bằng nhau khi và chỉ khi độ lệch tổng hiện tại bằng $9$ lần một nửa hiệu số lượng dấu hỏi ở hai nửa.
>
> Chỉ cần đếm số dấu `?` và tổng các chữ số ở mỗi nửa là có thể xác định người thắng mà không cần mô phỏng các nước đi.

<!-- thinking:end -->

Nếu số lượng `'?'` là lẻ, Alice chắc chắn thắng vì cô ấy có thể chọn thay dấu `'?'` cuối cùng bằng một chữ số bất kỳ, khiến tổng của nửa đầu khác tổng của nửa sau.

Nếu số lượng `'?'` là chẵn, Alice sẽ cố làm cho tổng của hai nửa khác nhau bằng cách đặt $9$ vào nửa có tổng hiện tại lớn hơn và $0$ vào nửa có tổng hiện tại nhỏ hơn. Ngược lại, Bob sẽ cố làm cho hai tổng bằng nhau bằng cách đặt vào nửa còn lại một chữ số tương ứng với chữ số Alice đã đặt.

Do đó, tất cả các dấu `'?'` còn lại với số lượng chẵn sẽ tập trung ở một nửa. Giả sử độ lệch hiện tại giữa tổng của hai nửa là $d$.

Xét trường hợp còn lại hai dấu `'?'` và độ lệch là $x$:

- Nếu $x \lt 9$, Alice chắc chắn thắng vì cô ấy có thể thay một trong các dấu `'?'` bằng $9$, khiến hai tổng khác nhau.
- Nếu $x \gt 9$, Alice chắc chắn thắng vì cô ấy có thể thay một trong các dấu `'?'` bằng $0$, khiến hai tổng khác nhau.
- Nếu $x = 9$, Bob chắc chắn thắng. Giả sử Alice thay một chữ số bằng $a$, khi đó Bob có thể thay dấu `'?'` còn lại bằng $9 - a$, khiến hai tổng bằng nhau.

Vì vậy, nếu độ lệch giữa tổng của hai nửa là $d = \frac{9 \times \textit{cnt}}{2}$, trong đó $\textit{cnt}$ là số dấu `'?'` còn lại, Bob chắc chắn thắng; nếu không, Alice chắc chắn thắng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumGame(self, num: str) -> bool:
        n = len(num)
        cnt1 = num[: n // 2].count("?")
        cnt2 = num[n // 2 :].count("?")
        s1 = sum(int(x) for x in num[: n // 2] if x != "?")
        s2 = sum(int(x) for x in num[n // 2 :] if x != "?")
        return (cnt1 + cnt2) % 2 == 1 or s1 - s2 != 9 * (cnt2 - cnt1) // 2
```

#### Java

```java
class Solution {
    public boolean sumGame(String num) {
        int n = num.length();
        int cnt1 = 0, cnt2 = 0;
        int s1 = 0, s2 = 0;
        for (int i = 0; i < n / 2; ++i) {
            if (num.charAt(i) == '?') {
                cnt1++;
            } else {
                s1 += num.charAt(i) - '0';
            }
        }
        for (int i = n / 2; i < n; ++i) {
            if (num.charAt(i) == '?') {
                cnt2++;
            } else {
                s2 += num.charAt(i) - '0';
            }
        }
        return (cnt1 + cnt2) % 2 == 1 || s1 - s2 != 9 * (cnt2 - cnt1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool sumGame(string num) {
        int n = num.size();
        int cnt1 = 0, cnt2 = 0;
        int s1 = 0, s2 = 0;
        for (int i = 0; i < n / 2; ++i) {
            if (num[i] == '?') {
                cnt1++;
            } else {
                s1 += num[i] - '0';
            }
        }
        for (int i = n / 2; i < n; ++i) {
            if (num[i] == '?') {
                cnt2++;
            } else {
                s2 += num[i] - '0';
            }
        }
        return (cnt1 + cnt2) % 2 == 1 || (s1 - s2) != 9 * (cnt2 - cnt1) / 2;
    }
};
```

#### Go

```go
func sumGame(num string) bool {
	n := len(num)
	var cnt1, cnt2, s1, s2 int
	for i := 0; i < n/2; i++ {
		if num[i] == '?' {
			cnt1++
		} else {
			s1 += int(num[i] - '0')
		}
	}
	for i := n / 2; i < n; i++ {
		if num[i] == '?' {
			cnt2++
		} else {
			s2 += int(num[i] - '0')
		}
	}
	return (cnt1+cnt2)%2 == 1 || s1-s2 != (cnt2-cnt1)*9/2
}
```

#### TypeScript

```ts
function sumGame(num: string): boolean {
    const n = num.length;
    let [cnt1, cnt2, s1, s2] = [0, 0, 0, 0];
    for (let i = 0; i < n >> 1; ++i) {
        if (num[i] === '?') {
            ++cnt1;
        } else {
            s1 += num[i].charCodeAt(0) - '0'.charCodeAt(0);
        }
    }
    for (let i = n >> 1; i < n; ++i) {
        if (num[i] === '?') {
            ++cnt2;
        } else {
            s2 += num[i].charCodeAt(0) - '0'.charCodeAt(0);
        }
    }
    return (cnt1 + cnt2) % 2 === 1 || 2 * (s1 - s2) !== 9 * (cnt2 - cnt1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
