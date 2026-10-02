---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - String
    - Parentheses
---

<!-- problem:start -->

# [921. Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid)

[中文文档](/solution/0900-0999/0921.Minimum%20Add%20to%20Make%20Parentheses%20Valid/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi dấu ngoặc hợp lệ khi và chỉ khi thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li>Đó là chuỗi rỗng,</li>
	<li>Có thể biểu diễn thành <code>AB</code> (<code>A</code> nối với <code>B</code>), trong đó <code>A</code> và <code>B</code> đều là chuỗi hợp lệ, hoặc</li>
	<li>Có thể biểu diễn thành <code>(A)</code>, trong đó <code>A</code> là chuỗi hợp lệ.</li>
</ul>

<p>Cho chuỗi dấu ngoặc <code>s</code>. Trong một thao tác, bạn có thể chèn một dấu ngoặc vào bất kỳ vị trí nào trong chuỗi.</p>

<ul>
	<li>Ví dụ, nếu <code>s = &quot;()))&quot;</code>, bạn có thể chèn dấu ngoặc mở để được <code>&quot;(<strong>(</strong>)))&quot;</code> hoặc chèn dấu ngoặc đóng để được <code>&quot;())<strong>)</strong>)&quot;</code>.</li>
</ul>

<p>Trả về <em>số thao tác ít nhất cần thực hiện để biến </em><code>s</code><em> thành chuỗi hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;())&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;(((&quot;
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> chỉ có thể là <code>&#39;(&#39;</code> hoặc <code>&#39;)&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + stack

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần thêm ít dấu ngoặc nhất để chuỗi hợp lệ. Mỗi dấu ngoặc còn lại trong stack dùng để kiểm tra cặp cần được ghép với một dấu tương ứng. Đẩy dấu ngoặc mở vào stack; khi gặp dấu ngoặc đóng, nếu có dấu ngoặc mở phù hợp thì lấy nó ra, nếu không thì giữ dấu ngoặc đóng lại. Kích thước stack cuối cùng là đáp án.

<!-- thinking:end -->

Đây là bài toán kinh điển về ghép cặp dấu ngoặc, có thể giải bằng phương pháp "Greedy + stack".

Duyệt từng ký tự $c$ trong chuỗi $s$:

- Nếu $c$ là dấu ngoặc mở, đẩy trực tiếp $c$ vào stack;
- Nếu $c$ là dấu ngoặc đóng, nếu stack không rỗng và phần tử trên cùng là dấu ngoặc mở thì lấy phần tử đó khỏi stack, tức là đã ghép được một cặp; nếu không, đẩy $c$ vào stack.

Sau khi duyệt xong, số phần tử còn lại trong stack chính là số dấu ngoặc cần thêm.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        stk = []
        for c in s:
            if c == ')' and stk and stk[-1] == '(':
                stk.pop()
            else:
                stk.append(c)
        return len(stk)
```

#### Java

```java
class Solution {
    public int minAddToMakeValid(String s) {
        Deque<Character> stk = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == ')' && !stk.isEmpty() && stk.peek() == '(') {
                stk.pop();
            } else {
                stk.push(c);
            }
        }
        return stk.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        string stk;
        for (char c : s) {
            if (c == ')' && stk.size() && stk.back() == '(')
                stk.pop_back();
            else
                stk.push_back(c);
        }
        return stk.size();
    }
};
```

#### Go

```go
func minAddToMakeValid(s string) int {
	stk := []rune{}
	for _, c := range s {
		if c == ')' && len(stk) > 0 && stk[len(stk)-1] == '(' {
			stk = stk[:len(stk)-1]
		} else {
			stk = append(stk, c)
		}
	}
	return len(stk)
}
```

#### TypeScript

```ts
function minAddToMakeValid(s: string): number {
    const stk: string[] = [];
    for (const c of s) {
        if (c === ')' && stk.length > 0 && stk.at(-1)! === '(') {
            stk.pop();
        } else {
            stk.push(c);
        }
    }
    return stk.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Greedy + đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần lưu số dấu ngoặc mở chưa ghép cặp, không cần lưu các ký tự trong stack. $cnt$ đếm số dấu ngoặc mở chưa ghép cặp, các dấu ngoặc đóng chưa ghép được cộng vào $ans$, rồi cuối cùng cộng phần $cnt$ còn lại vào đáp án. Cách này chỉ cần thêm không gian hằng số.

<!-- thinking:end -->

Lời giải 1 dùng stack để ghép cặp dấu ngoặc, nhưng ta cũng có thể thực hiện trực tiếp bằng cách đếm.

Định nghĩa biến `cnt` để lưu số dấu ngoặc mở hiện đang chờ ghép cặp, và biến `ans` để lưu đáp án. Ban đầu, cả hai biến đều bằng $0$.

Duyệt từng ký tự $c$ trong chuỗi $s$:

- Nếu $c$ là dấu ngoặc mở, tăng `cnt` thêm $1$;
- Nếu $c$ là dấu ngoặc đóng và $cnt > 0$, tức là có dấu ngoặc mở để ghép cặp, giảm `cnt` đi $1$; nếu không, dấu ngoặc đóng hiện tại không thể ghép cặp nên tăng `ans` thêm $1$.

Sau khi duyệt xong, cộng `cnt` vào `ans`; đó là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        ans = cnt = 0
        for c in s:
            if c == '(':
                cnt += 1
            elif cnt:
                cnt -= 1
            else:
                ans += 1
        ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public int minAddToMakeValid(String s) {
        int ans = 0, cnt = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                ++cnt;
            } else if (cnt > 0) {
                --cnt;
            } else {
                ++ans;
            }
        }
        ans += cnt;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int ans = 0, cnt = 0;
        for (char c : s) {
            if (c == '(')
                ++cnt;
            else if (cnt)
                --cnt;
            else
                ++ans;
        }
        ans += cnt;
        return ans;
    }
};
```

#### Go

```go
func minAddToMakeValid(s string) int {
	ans, cnt := 0, 0
	for _, c := range s {
		if c == '(' {
			cnt++
		} else if cnt > 0 {
			cnt--
		} else {
			ans++
		}
	}
	ans += cnt
	return ans
}
```

#### TypeScript

```ts
function minAddToMakeValid(s: string): number {
    let [ans, cnt] = [0, 0];
    for (const c of s) {
        if (c === '(') {
            ++cnt;
        } else if (cnt) {
            --cnt;
        } else {
            ++ans;
        }
    }
    ans += cnt;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Thay thế + đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp `()` đã ghép không ảnh hưởng đến đáp án nên có thể xóa đi. Lặp lại thao tác thay thế một cặp `()`; nếu độ dài không đổi thì không còn cặp nào có thể ghép và độ dài hiện tại là số dấu ngoặc cần thêm, nếu không thì tiếp tục gọi đệ quy. Vì $n\le 1000$, số lần thay thế này vẫn chấp nhận được.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function minAddToMakeValid(s: string): number {
    const l = s.length;
    s = s.replace('()', '');

    return s.length === l ? l : minAddToMakeValid(s);
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var minAddToMakeValid = function (s) {
    const l = s.length;
    s = s.replace('()', '');
    return s.length === l ? l : minAddToMakeValid(s);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
