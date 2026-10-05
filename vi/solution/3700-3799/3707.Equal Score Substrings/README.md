---
comments: true
difficulty: Easy
rating: 1262
source: Biweekly Contest 167 Q1
tags:
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [3707. Equal Score Substrings](https://leetcode.com/problems/equal-score-substrings)

[中文文档](/solution/3700-3799/3707.Equal%20Score%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường.</p>

<p><strong>Điểm số</strong> của một chuỗi là tổng vị trí của các ký tự trong bảng chữ cái, trong đó <code>&#39;a&#39; = 1</code>, <code>&#39;b&#39; = 2</code>, ..., <code>&#39;z&#39; = 26</code>.</p>

<p>Hãy xác định xem có tồn tại chỉ số <code>i</code> sao cho chuỗi có thể được chia tại đó thành hai <strong>không rỗng</strong> <strong><strong><span data-keyword="substring-nonempty">chuỗi con</span></strong></strong> <code>s[0..i]</code> và <code>s[(i + 1)..(n - 1)]</code> có điểm số <strong>bằng nhau</strong> hay không.</p>

<p>Trả về <code>true</code> nếu tồn tại cách chia như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;adcb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chia tại chỉ số <code>i = 1</code>:</p>

<ul>
	<li>Chuỗi con bên trái = <code>s[0..1] = &quot;ad&quot;</code> với <code>score = 1 + 4 = 5</code></li>
	<li>Chuỗi con bên phải = <code>s[2..3] = &quot;cb&quot;</code> với <code>score = 3 + 2 = 5</code></li>
</ul>

<p>Hai chuỗi con có điểm số bằng nhau, nên kết quả là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;bace&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:​​​​​​</strong></p>

<p><strong>​​​​​​​</strong>Không có cách chia nào tạo ra hai điểm số bằng nhau, nên kết quả là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có $n-1$ vị trí chia, nhưng việc tính lại hai phía từ đầu sẽ lặp lại các phép tính. Điểm số của các ký tự có tính chất cộng dồn theo tiền tố: ban đầu đặt tổng điểm của toàn chuỗi làm điểm bên phải, rồi lần lượt chuyển từng ký tự từ phải sang trái; khi hai điểm bằng nhau, ta tìm được một cách chia hợp lệ.

<!-- thinking:end -->

Trước tiên, ta tính tổng điểm của chuỗi, ký hiệu là $r$. Sau đó, ta duyệt $n-1$ ký tự đầu tiên từ trái sang phải, tính điểm tiền tố $l$ và cập nhật điểm hậu tố $r$. Nếu tại một vị trí $i$ nào đó, điểm tiền tố $l$ bằng điểm hậu tố $r$, nghĩa là tồn tại chỉ số $i$ có thể chia chuỗi thành hai chuỗi con có điểm số bằng nhau, nên ta trả về $\textit{true}$. Nếu duyệt hết mà không tìm thấy chỉ số như vậy, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreBalance(self, s: str) -> bool:
        l = 0
        r = sum(ord(c) - ord("a") + 1 for c in s)
        for c in s[:-1]:
            x = ord(c) - ord("a") + 1
            l += x
            r -= x
            if l == r:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean scoreBalance(String s) {
        int n = s.length();
        int l = 0, r = 0;
        for (int i = 0; i < n; ++i) {
            int x = s.charAt(i) - 'a' + 1;
            r += x;
        }
        for (int i = 0; i < n - 1; ++i) {
            int x = s.charAt(i) - 'a' + 1;
            l += x;
            r -= x;
            if (l == r) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool scoreBalance(string s) {
        int l = 0, r = 0;
        for (char c : s) {
            int x = c - 'a' + 1;
            r += x;
        }
        for (int i = 0; i < s.size() - 1; ++i) {
            int x = s[i] - 'a' + 1;
            l += x;
            r -= x;
            if (l == r) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func scoreBalance(s string) bool {
	var l, r int
	for _, c := range s {
		x := int(c-'a') + 1
		r += x
	}
	for _, c := range s[:len(s)-1] {
		x := int(c-'a') + 1
		l += x
		r -= x
		if l == r {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function scoreBalance(s: string): boolean {
    let [l, r] = [0, 0];
    for (const c of s) {
        const x = c.charCodeAt(0) - 96;
        r += x;
    }
    for (let i = 0; i < s.length - 1; ++i) {
        const x = s[i].charCodeAt(0) - 96;
        l += x;
        r -= x;
        if (l === r) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
