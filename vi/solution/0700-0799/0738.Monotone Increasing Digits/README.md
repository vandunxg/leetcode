---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [738. Monotone Increasing Digits](https://leetcode.com/problems/monotone-increasing-digits)

[中文文档](/solution/0700-0799/0738.Monotone%20Increasing%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên có <strong>các chữ số tăng đơn điệu</strong> khi và chỉ khi mọi cặp chữ số liền kề <code>x</code> và <code>y</code> đều thỏa mãn <code>x &lt;= y</code>.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số lớn nhất nhỏ hơn hoặc bằng </em><code>n</code><em> có <strong>các chữ số tăng đơn điệu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 9
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1234
<strong>Đầu ra:</strong> 1234
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 332
<strong>Đầu ra:</strong> 299
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm số lớn nhất $\le n$ có các chữ số không giảm. Thử lần lượt $n, n-1, \ldots$ sẽ quá chậm, nên ta cần điều chỉnh các chữ số.
>
> Sau vị trí giảm đầu tiên, đổi phần đuôi thành các chữ số 9 vẫn có thể khiến phần đầu lớn hơn $n$. Giảm dần dãy chữ số bên trái cho đến khi các chữ số không giảm trở lại, rồi đổi hậu tố thành toàn chữ số 9.
>
> Duyệt đến vị trí giảm đầu tiên, lùi lại và giảm chữ số khi cần, rồi điền các chữ số 9 vào phần còn lại. Có $O(\log n)$ chữ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        s = list(str(n))
        i = 1
        while i < len(s) and s[i - 1] <= s[i]:
            i += 1
        if i < len(s):
            while i and s[i - 1] > s[i]:
                s[i - 1] = str(int(s[i - 1]) - 1)
                i -= 1
            i += 1
            while i < len(s):
                s[i] = '9'
                i += 1
        return int(''.join(s))
```

#### Java

```java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        char[] s = String.valueOf(n).toCharArray();
        int i = 1;
        for (; i < s.length && s[i - 1] <= s[i]; ++i)
            ;
        if (i < s.length) {
            for (; i > 0 && s[i - 1] > s[i]; --i) {
                --s[i - 1];
            }
            ++i;
            for (; i < s.length; ++i) {
                s[i] = '9';
            }
        }
        return Integer.parseInt(String.valueOf(s));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string s = to_string(n);
        int i = 1;
        for (; i < s.size() && s[i - 1] <= s[i]; ++i)
            ;
        if (i < s.size()) {
            for (; i > 0 && s[i - 1] > s[i]; --i) {
                --s[i - 1];
            }
            ++i;
            for (; i < s.size(); ++i) {
                s[i] = '9';
            }
        }
        return stoi(s);
    }
};
```

#### Go

```go
func monotoneIncreasingDigits(n int) int {
	s := []byte(strconv.Itoa(n))
	i := 1
	for ; i < len(s) && s[i-1] <= s[i]; i++ {
	}
	if i < len(s) {
		for ; i > 0 && s[i-1] > s[i]; i-- {
			s[i-1]--
		}
		i++
		for ; i < len(s); i++ {
			s[i] = '9'
		}
	}
	ans, _ := strconv.Atoi(string(s))
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
