---
comments: true
difficulty: Medium
rating: 1445
source: Weekly Contest 251 Q2
tags:
    - Greedy
    - Array
    - String
---

<!-- problem:start -->

# [1946. Largest Number After Mutating Substring](https://leetcode.com/problems/largest-number-after-mutating-substring)

[中文文档](/solution/1900-1999/1946.Largest%20Number%20After%20Mutating%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>num</code> biểu diễn một số nguyên lớn. Đồng thời, bạn được cung cấp một mảng số nguyên <strong>0-indexed</strong> <code>change</code> có độ dài <code>10</code>, ánh xạ mỗi chữ số <code>0-9</code> sang một chữ số khác. Cụ thể hơn, chữ số <code>d</code> được ánh xạ tới chữ số <code>change[d]</code>.</p>

<p>Bạn có thể <strong>chọn</strong> <b>biến đổi một substring duy nhất</b> của <code>num</code>. Để biến đổi một substring, thay mỗi chữ số <code>num[i]</code> bằng chữ số mà nó được ánh xạ tới trong <code>change</code> (tức là thay <code>num[i]</code> bằng <code>change[num[i]]</code>).</p>

<p>Trả về <em>một chuỗi biểu diễn số nguyên <strong>lớn nhất</strong> có thể tạo ra sau khi <strong>biến đổi</strong> (hoặc không biến đổi) <strong>một substring duy nhất</strong> của </em><code>num</code>.</p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;<u>1</u>32&quot;, change = [9,8,5,0,3,6,4,2,6,8]
<strong>Đầu ra:</strong> &quot;<u>8</u>32&quot;
<strong>Giải thích:</strong> Thay thế substring &quot;1&quot;:
- 1 được ánh xạ tới change[1] = 8.
Vì vậy, &quot;<u>1</u>32&quot; trở thành &quot;<u>8</u>32&quot;.
&quot;832&quot; là số lớn nhất có thể tạo ra, nên trả về nó.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;<u>021</u>&quot;, change = [9,4,3,5,7,2,1,9,0,6]
<strong>Đầu ra:</strong> &quot;<u>934</u>&quot;
<strong>Giải thích:</strong> Thay thế substring &quot;021&quot;:
- 0 được ánh xạ tới change[0] = 9.
- 2 được ánh xạ tới change[2] = 3.
- 1 được ánh xạ tới change[1] = 4.
Vì vậy, &quot;<u>021</u>&quot; trở thành &quot;<u>934</u>&quot;.
&quot;934&quot; là số lớn nhất có thể tạo ra, nên trả về nó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;5&quot;, change = [1,4,7,5,3,2,5,6,9,4]
<strong>Đầu ra:</strong> &quot;5&quot;
<strong>Giải thích:</strong> &quot;5&quot; vốn đã là số lớn nhất có thể tạo ra, nên trả về nó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> chỉ gồm các chữ số <code>0-9</code>.</li>
	<li><code>change.length == 10</code></li>
	<li><code>0 &lt;= change[d] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể biến đổi một substring liên tiếp và cần tạo ra kết quả lớn nhất theo thứ tự từ điển. Vì vậy, ta bắt đầu thay thế ngay khi một chữ số tăng lên và dừng tại lần giảm đầu tiên.
>
> Trước khi bắt đầu biến đổi, bỏ qua các chữ số không tăng. Sau khi đã bắt đầu, nếu chữ số được ánh xạ nhỏ hơn chữ số hiện tại thì phải kết thúc đoạn; nếu bằng nhau thì có thể tiếp tục.
>
> Chỉ cần duyệt từ trái sang phải một lần để chọn ra đoạn duy nhất đó.

<!-- thinking:end -->

Theo đề bài, ta có thể bắt đầu từ chữ số đầu tiên của chuỗi và tham lam thực hiện việc thay thế liên tiếp cho đến khi gặp một chữ số nhỏ hơn chữ số hiện tại.

Trước tiên, ta chuyển chuỗi $\textit{num}$ thành một mảng ký tự $\textit{s}$ và dùng biến $\textit{changed}$ để ghi nhận liệu đã có thay đổi nào xảy ra hay chưa, ban đầu $\textit{changed} = \text{false}$.

Sau đó, ta duyệt qua mảng ký tự $\textit{s}$. Với mỗi ký tự $\textit{c}$, ta chuyển nó thành số $\textit{d} = \text{change}[\text{int}(\textit{c})]$. Nếu đã có thay đổi và $\textit{d} < \textit{c}$, nghĩa là không thể tiếp tục thay đổi, nên ta lập tức thoát khỏi vòng lặp. Ngược lại, nếu $\textit{d} > \textit{c}$, nghĩa là ta có thể thay $\textit{c}$ bằng $\textit{d}$. Khi đó, ta đặt $\textit{changed} = \text{true}$ và thay $\textit{s}[i]$ bằng $\textit{d}$.

Cuối cùng, ta chuyển mảng ký tự $\textit{s}$ trở lại thành chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $\textit{num}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumNumber(self, num: str, change: List[int]) -> str:
        s = list(num)
        changed = False
        for i, c in enumerate(s):
            d = str(change[int(c)])
            if changed and d < c:
                break
            if d > c:
                changed = True
                s[i] = d
        return "".join(s)
```

#### Java

```java
class Solution {
    public String maximumNumber(String num, int[] change) {
        char[] s = num.toCharArray();
        boolean changed = false;
        for (int i = 0; i < s.length; ++i) {
            char d = (char) (change[s[i] - '0'] + '0');
            if (changed && d < s[i]) {
                break;
            }
            if (d > s[i]) {
                changed = true;
                s[i] = d;
            }
        }
        return new String(s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maximumNumber(string num, vector<int>& change) {
        int n = num.size();
        bool changed = false;
        for (int i = 0; i < n; ++i) {
            char d = '0' + change[num[i] - '0'];
            if (changed && d < num[i]) {
                break;
            }
            if (d > num[i]) {
                changed = true;
                num[i] = d;
            }
        }
        return num;
    }
};
```

#### Go

```go
func maximumNumber(num string, change []int) string {
	s := []byte(num)
	changed := false
	for i, c := range num {
		d := byte('0' + change[c-'0'])
		if changed && d < s[i] {
			break
		}
		if d > s[i] {
			s[i] = d
			changed = true
		}
	}
	return string(s)
}
```

#### TypeScript

```ts
function maximumNumber(num: string, change: number[]): string {
    const s = num.split('');
    let changed = false;
    for (let i = 0; i < s.length; ++i) {
        const d = change[+s[i]].toString();
        if (changed && d < s[i]) {
            break;
        }
        if (d > s[i]) {
            s[i] = d;
            changed = true;
        }
    }
    return s.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_number(num: String, change: Vec<i32>) -> String {
        let mut s: Vec<char> = num.chars().collect();
        let mut changed = false;
        for i in 0..s.len() {
            let d = (change[s[i] as usize - '0' as usize] + '0' as i32) as u8 as char;
            if changed && d < s[i] {
                break;
            }
            if d > s[i] {
                changed = true;
                s[i] = d;
            }
        }
        s.into_iter().collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {string} num
 * @param {number[]} change
 * @return {string}
 */
var maximumNumber = function (num, change) {
    const s = num.split('');
    let changed = false;
    for (let i = 0; i < s.length; ++i) {
        const d = change[+s[i]].toString();
        if (changed && d < s[i]) {
            break;
        }
        if (d > s[i]) {
            s[i] = d;
            changed = true;
        }
    }
    return s.join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
