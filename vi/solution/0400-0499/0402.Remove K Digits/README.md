---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - String
    - Monotonic Stack
---

<!-- problem:start -->

# [402. Remove K Digits](https://leetcode.com/problems/remove-k-digits)

[中文文档](/solution/0400-0499/0402.Remove%20K%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi num biểu diễn một số nguyên không âm <code>num</code> và số nguyên <code>k</code>, hãy trả về <em>số nguyên nhỏ nhất có thể thu được sau khi xóa</em> <code>k</code> <em>chữ số khỏi</em> <code>num</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;1432219&quot;, k = 3
<strong>Đầu ra:</strong> &quot;1219&quot;
<strong>Giải thích:</strong> Xóa ba chữ số 4, 3 và 2 để tạo thành số mới 1219, là số nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;10200&quot;, k = 1
<strong>Đầu ra:</strong> &quot;200&quot;
<strong>Giải thích:</strong> Xóa chữ số 1 ở đầu thì số còn lại là 200. Lưu ý kết quả không được có các số 0 ở đầu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;10&quot;, k = 2
<strong>Đầu ra:</strong> &quot;0&quot;
<strong>Giải thích:</strong> Xóa tất cả chữ số thì không còn chữ số nào, tương ứng với số 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
	<li><code>num</code> không có số 0 ở đầu, trừ trường hợp chính nó là số 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa $k$ chữ số, ta cần số còn lại nhỏ nhất. Khi so sánh hai số nguyên có cùng độ dài, ta xét từ chữ số đầu tiên khác nhau; vì vậy nên xóa các chữ số lớn ở bên trái trước.
>
> Duyệt từ trái sang phải bằng một stack đơn điệu không giảm; mỗi khi chữ số hiện tại nhỏ hơn phần tử trên đỉnh, pop phần tử đó và dùng một lượt xóa. Sau đó giữ lại đúng độ dài còn lại và bỏ các số 0 ở đầu.
>
> Cần duyệt từ trái sang phải: chữ số nhỏ hơn chỉ có thể chiếm hàng cao hơn sau khi chữ số lớn hơn ở bên trái bị xóa.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        stk = []
        remain = len(num) - k
        for c in num:
            while k and stk and stk[-1] > c:
                stk.pop()
                k -= 1
            stk.append(c)
        return ''.join(stk[:remain]).lstrip('0') or '0'
```

#### Java

```java
class Solution {
    public String removeKdigits(String num, int k) {
        StringBuilder stk = new StringBuilder();
        for (char c : num.toCharArray()) {
            while (k > 0 && stk.length() > 0 && stk.charAt(stk.length() - 1) > c) {
                stk.deleteCharAt(stk.length() - 1);
                --k;
            }
            stk.append(c);
        }
        for (; k > 0; --k) {
            stk.deleteCharAt(stk.length() - 1);
        }
        int i = 0;
        for (; i < stk.length() && stk.charAt(i) == '0'; ++i) {
        }
        String ans = stk.substring(i);
        return "".equals(ans) ? "0" : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {
        string stk;
        for (char& c : num) {
            while (k && stk.size() && stk.back() > c) {
                stk.pop_back();
                --k;
            }
            stk += c;
        }
        while (k--) {
            stk.pop_back();
        }
        int i = 0;
        for (; i < stk.size() && stk[i] == '0'; ++i) {
        }
        string ans = stk.substr(i);
        return ans == "" ? "0" : ans;
    }
};
```

#### Go

```go
func removeKdigits(num string, k int) string {
	stk, remain := make([]byte, 0), len(num)-k
	for i := 0; i < len(num); i++ {
		n := len(stk)
		for k > 0 && n > 0 && stk[n-1] > num[i] {
			stk = stk[:n-1]
			n, k = n-1, k-1
		}
		stk = append(stk, num[i])
	}

	for i := 0; i < len(stk) && i < remain; i++ {
		if stk[i] != '0' {
			return string(stk[i:remain])
		}
	}
	return "0"
}
```

#### TypeScript

```ts
function removeKdigits(num: string, k: number): string {
    const stk: string[] = [];
    for (const c of num) {
        while (k && stk.length > 0 && stk[stk.length - 1] > c) {
            stk.pop();
            k--;
        }
        stk.push(c);
    }
    while (k--) {
        stk.pop();
    }
    return stk.join('').replace(/^0*/g, '') || '0';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
