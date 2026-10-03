---
comments: true
difficulty: Easy
rating: 1264
source: Weekly Contest 225 Q1
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1736. Latest Time by Replacing Hidden Digits](https://leetcode.com/problems/latest-time-by-replacing-hidden-digits)

[中文文档](/solution/1700-1799/1736.Latest%20Time%20by%20Replacing%20Hidden%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>time</code> có dạng <code> hh:mm</code>, trong đó một số chữ số bị ẩn (được biểu diễn bằng <code>?</code>).</p>

<p>Thời gian hợp lệ nằm trong đoạn từ <code>00:00</code> đến <code>23:59</code>, kể cả hai đầu mút.</p>

<p>Trả về <em>thời gian hợp lệ muộn nhất có thể tạo từ</em> <code>time</code><em> bằng cách thay thế các chữ số bị ẩn</em> <em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> time = &quot;2?:?0&quot;
<strong>Output:</strong> &quot;23:50&quot;
<strong>Giải thích:</strong> Giờ muộn nhất bắt đầu bằng chữ số &#39;2&#39; là 23 và phút muộn nhất kết thúc bằng chữ số &#39;0&#39; là 50.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> time = &quot;0?:3?&quot;
<strong>Output:</strong> &quot;09:39&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> time = &quot;1?:22&quot;
<strong>Output:</strong> &quot;19:22&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>time</code> có định dạng <code>hh:mm</code>.</li>
	<li>Đảm bảo có thể tạo được thời gian hợp lệ từ chuỗi đã cho.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Thời gian hợp lệ nằm trong khoảng $00$:$00$– $23$:$59$. Ta cần điền các chữ số ẩn để tạo thời gian muộn nhất. Các ràng buộc có tính cục bộ, nên chọn chữ số lớn nhất có thể từ trái sang phải.
>
> Chữ số hàng chục của giờ phụ thuộc vào việc chữ số hàng đơn vị đã nằm trong $4$– $9$ hay chưa; chữ số hàng đơn vị của giờ phụ thuộc vào việc hàng chục có bằng $2$ hay không; hai chữ số phút tối đa lần lượt là $5$ và $9$.

<!-- thinking:end -->

Ta xử lý từng chữ số của chuỗi theo thứ tự, với các quy tắc sau:

1. Chữ số thứ nhất: Nếu chữ số thứ hai đã xác định và nằm trong đoạn $[4, 9]$, chữ số thứ nhất chỉ có thể là $1$. Nếu không, chữ số thứ nhất có thể lớn nhất là $2$.
1. Chữ số thứ hai: Nếu chữ số thứ nhất đã xác định và bằng $2$, chữ số thứ hai có thể lớn nhất là $3$. Nếu không, chữ số thứ hai có thể lớn nhất là $9$.
1. Chữ số thứ ba: Chữ số thứ ba có thể lớn nhất là $5$.
1. Chữ số thứ tư: Chữ số thứ tư có thể lớn nhất là $9$.

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTime(self, time: str) -> str:
        t = list(time)
        if t[0] == '?':
            t[0] = '1' if '4' <= t[1] <= '9' else '2'
        if t[1] == '?':
            t[1] = '3' if t[0] == '2' else '9'
        if t[3] == '?':
            t[3] = '5'
        if t[4] == '?':
            t[4] = '9'
        return ''.join(t)
```

#### Java

```java
class Solution {
    public String maximumTime(String time) {
        char[] t = time.toCharArray();
        if (t[0] == '?') {
            t[0] = t[1] >= '4' && t[1] <= '9' ? '1' : '2';
        }
        if (t[1] == '?') {
            t[1] = t[0] == '2' ? '3' : '9';
        }
        if (t[3] == '?') {
            t[3] = '5';
        }
        if (t[4] == '?') {
            t[4] = '9';
        }
        return new String(t);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maximumTime(string time) {
        if (time[0] == '?') {
            time[0] = (time[1] >= '4' && time[1] <= '9') ? '1' : '2';
        }
        if (time[1] == '?') {
            time[1] = (time[0] == '2') ? '3' : '9';
        }
        if (time[3] == '?') {
            time[3] = '5';
        }
        if (time[4] == '?') {
            time[4] = '9';
        }
        return time;
    }
};
```

#### Go

```go
func maximumTime(time string) string {
	t := []byte(time)
	if t[0] == '?' {
		if t[1] >= '4' && t[1] <= '9' {
			t[0] = '1'
		} else {
			t[0] = '2'
		}
	}
	if t[1] == '?' {
		if t[0] == '2' {
			t[1] = '3'
		} else {
			t[1] = '9'
		}
	}
	if t[3] == '?' {
		t[3] = '5'
	}
	if t[4] == '?' {
		t[4] = '9'
	}
	return string(t)
}
```

#### JavaScript

```js
/**
 * @param {string} time
 * @return {string}
 */
var maximumTime = function (time) {
    const t = Array.from(time);
    if (t[0] === '?') {
        t[0] = t[1] >= '4' && t[1] <= '9' ? '1' : '2';
    }
    if (t[1] === '?') {
        t[1] = t[0] == '2' ? '3' : '9';
    }
    if (t[3] === '?') {
        t[3] = '5';
    }
    if (t[4] === '?') {
        t[4] = '9';
    }
    return t.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
