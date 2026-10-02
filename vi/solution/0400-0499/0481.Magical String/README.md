---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [481. Magical String](https://leetcode.com/problems/magical-string)

[中文文档](/solution/0400-0499/0481.Magical%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi kỳ diệu <code>s</code> chỉ gồm <code>&#39;1&#39;</code> và <code>&#39;2&#39;</code>, đồng thời tuân theo quy tắc sau:</p>

<ul>
	<li>Nối các độ dài của những nhóm ký tự giống nhau liên tiếp gồm <code>&#39;1&#39;</code> và <code>&#39;2&#39;</code> sẽ tạo thành chính chuỗi <code>s</code>.</li>
</ul>

<p>Một vài phần tử đầu tiên của <code>s</code> là <code>s = &quot;1221121221221121122&hellip;&hellip;&quot;</code>. Nếu gom các ký tự <code>1</code> và <code>2</code> liên tiếp trong <code>s</code> thành từng nhóm, ta được <code>&quot;1 22 11 2 1 22 1 22 11 2 11 22 ......&quot;</code>. Đếm số lần xuất hiện của <code>1</code> hoặc <code>2</code> trong mỗi nhóm sẽ thu được dãy&nbsp;<code>&quot;1 2 2 1 1 2 1 2 2 1 2 2 ......&quot;</code>.</p>

<p>Ta thấy rằng nối các số đếm này lại sẽ thu được chính <code>s</code>.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về số lượng ký tự <code>1</code> trong <code>n</code> phần tử đầu tiên của chuỗi kỳ diệu <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 6 phần tử đầu tiên của chuỗi kỳ diệu s là &quot;122112&quot;, trong đó có ba ký tự 1, nên trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng quá trình tạo chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi kỳ diệu tự mô tả độ dài các nhóm ký tự liên tiếp của nó: một $1$, hai $2$, sau đó luân phiên các nhóm $1$ và $2$ với độ dài lấy từ chính chuỗi. Ta cần đếm số ký tự $1$ trong $n$ ký tự đầu tiên.
>
> Bắt đầu với tiền tố đã biết là $122$. Con trỏ $i$ cho biết nhóm kế tiếp có độ dài bao nhiêu; giá trị vừa ghi gần nhất quyết định nhóm đó gồm $1$ hay $2$ ($3-\textit{pre}$). Dừng khi độ dài chuỗi đạt $n$.
>
> Ta vừa tạo chuỗi vừa đếm trong một lượt. Độ dài mỗi nhóm được lấy từ các ký tự đã ghi, nên $i$ không vượt quá độ dài hiện tại của chuỗi.

<!-- thinking:end -->

Theo đề bài, độ dài của mỗi nhóm trong chuỗi $s$ được xác định bởi các chữ số của chính chuỗi $s$.

Hai nhóm đầu tiên trong chuỗi $s$ là $1$ và $22$, lần lượt được xác định bởi chữ số thứ nhất và thứ hai của chuỗi $s$. Ngoài ra, nhóm thứ nhất chỉ gồm $1$, nhóm thứ hai chỉ gồm $2$, nhóm thứ ba chỉ gồm $1$, và cứ thế luân phiên.

Vì đã biết hai nhóm đầu tiên, ta khởi tạo chuỗi $s$ là $122$, rồi bắt đầu tạo từ nhóm thứ ba. Nhóm thứ ba được xác định bởi chữ số thứ ba của chuỗi $s$ (chỉ số $i=2$), nên lúc này ta đặt con trỏ $i$ tại chữ số thứ ba $2$ của $s$.

```
1 2 2
    ^
    i
```

Chữ số tại vị trí con trỏ $i$ là $2$, nghĩa là nhóm thứ ba có độ dài 2. Vì nhóm trước đó là $2$ và các nhóm luân phiên giá trị, nhóm thứ ba gồm hai chữ số $1$, tức là $11$. Sau khi tạo nhóm, con trỏ $i$ chuyển sang vị trí kế tiếp, trỏ tới chữ số thứ tư $1$ của chuỗi $s$.

```
1 2 2 1 1
      ^
      i
```

Lúc này, chữ số tại vị trí con trỏ $i$ là $1$, nghĩa là nhóm thứ tư có độ dài 1. Vì nhóm trước đó là $1$ và các nhóm luân phiên giá trị, nhóm thứ tư gồm một chữ số $2$, tức là $2$. Sau khi tạo nhóm, con trỏ $i$ chuyển sang vị trí kế tiếp, trỏ tới chữ số thứ năm $1$ của chuỗi $s$.

```
1 2 2 1 1 2
        ^
        i
```

Theo quy tắc này, ta lần lượt mô phỏng quá trình tạo chuỗi cho đến khi độ dài của $s$ lớn hơn hoặc bằng $n$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def magicalString(self, n: int) -> int:
        s = [1, 2, 2]
        i = 2
        while len(s) < n:
            pre = s[-1]
            cur = 3 - pre
            s += [cur] * s[i]
            i += 1
        return s[:n].count(1)
```

#### Java

```java
class Solution {
    public int magicalString(int n) {
        List<Integer> s = new ArrayList<>(List.of(1, 2, 2));
        for (int i = 2; s.size() < n; ++i) {
            int pre = s.get(s.size() - 1);
            int cur = 3 - pre;
            for (int j = 0; j < s.get(i); ++j) {
                s.add(cur);
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (s.get(i) == 1) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int magicalString(int n) {
        vector<int> s = {1, 2, 2};
        for (int i = 2; s.size() < n; ++i) {
            int pre = s.back();
            int cur = 3 - pre;
            for (int j = 0; j < s[i]; ++j) {
                s.emplace_back(cur);
            }
        }
        return count(s.begin(), s.begin() + n, 1);
    }
};
```

#### Go

```go
func magicalString(n int) (ans int) {
	s := []int{1, 2, 2}
	for i := 2; len(s) < n; i++ {
		pre := s[len(s)-1]
		cur := 3 - pre
		for j := 0; j < s[i]; j++ {
			s = append(s, cur)
		}
	}
	for _, c := range s[:n] {
		if c == 1 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function magicalString(n: number): number {
    const s: number[] = [1, 2, 2];
    for (let i = 2; s.length < n; ++i) {
        let pre = s[s.length - 1];
        let cur = 3 - pre;
        for (let j = 0; j < s[i]; ++j) {
            s.push(cur);
        }
    }
    return s.slice(0, n).filter(x => x === 1).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn magical_string(n: i32) -> i32 {
        let mut s = vec![1, 2, 2];
        let mut i = 2;

        while s.len() < n as usize {
            let pre = s[s.len() - 1];
            let cur = 3 - pre;
            for _ in 0..s[i] {
                s.push(cur);
            }
            i += 1;
        }

        s.iter().take(n as usize).filter(|&&x| x == 1).count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
