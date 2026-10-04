---
comments: true
difficulty: Medium
rating: 1855
source: Weekly Contest 435 Q2
tags:
    - Hash Table
    - Math
    - String
    - Counting
---

<!-- problem:start -->

# [3443. Maximum Manhattan Distance After K Changes](https://leetcode.com/problems/maximum-manhattan-distance-after-k-changes)

[中文文档](/solution/3400-3499/3443.Maximum%20Manhattan%20Distance%20After%20K%20Changes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;N&#39;</code>, <code>&#39;S&#39;</code>, <code>&#39;E&#39;</code> và <code>&#39;W&#39;</code>, trong đó <code>s[i]</code> biểu thị một bước di chuyển trên lưới vô hạn:</p>

<ul>
	<li><code>&#39;N&#39;</code> : Di chuyển về phía bắc 1 đơn vị.</li>
	<li><code>&#39;S&#39;</code> : Di chuyển về phía nam 1 đơn vị.</li>
	<li><code>&#39;E&#39;</code> : Di chuyển về phía đông 1 đơn vị.</li>
	<li><code>&#39;W&#39;</code> : Di chuyển về phía tây 1 đơn vị.</li>
</ul>

<p>Ban đầu, bạn ở gốc tọa độ <code>(0, 0)</code>. Bạn có thể thay đổi <strong>nhiều nhất</strong> <code>k</code> ký tự thành một trong bốn hướng.</p>

<p>Hãy tìm <strong>khoảng cách Manhattan</strong> <strong>lớn nhất</strong> từ gốc tọa độ có thể đạt được <strong>tại bất kỳ thời điểm nào</strong> khi thực hiện các bước di chuyển <strong>theo đúng thứ tự</strong>.</p>
Khoảng cách <strong>Manhattan</strong> giữa hai ô <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và <code>(x<sub>j</sub>, y<sub>j</sub>)</code> là <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;NWSE&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay đổi <code>s[2]</code> từ <code>&#39;S&#39;</code> thành <code>&#39;N&#39;</code>. Chuỗi <code>s</code> trở thành <code>&quot;NWNE&quot;</code>.</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Di chuyển</th>
			<th style="border: 1px solid black;">Vị trí (x, y)</th>
			<th style="border: 1px solid black;">Khoảng cách Manhattan</th>
			<th style="border: 1px solid black;">Lớn nhất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">s[0] == &#39;N&#39;</td>
			<td style="border: 1px solid black;">(0, 1)</td>
			<td style="border: 1px solid black;">0 + 1 = 1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">s[1] == &#39;W&#39;</td>
			<td style="border: 1px solid black;">(-1, 1)</td>
			<td style="border: 1px solid black;">1 + 1 = 2</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">s[2] == &#39;N&#39;</td>
			<td style="border: 1px solid black;">(-1, 2)</td>
			<td style="border: 1px solid black;">1 + 2 = 3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">s[3] == &#39;E&#39;</td>
			<td style="border: 1px solid black;">(0, 2)</td>
			<td style="border: 1px solid black;">0 + 2 = 2</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>

<p>Khoảng cách Manhattan lớn nhất từ gốc tọa độ có thể đạt được là 3. Do đó, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;NSWWEW&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thay đổi <code>s[1]</code> từ <code>&#39;S&#39;</code> thành <code>&#39;N&#39;</code>, và <code>s[4]</code> từ <code>&#39;E&#39;</code> thành <code>&#39;W&#39;</code>. Chuỗi <code>s</code> trở thành <code>&quot;NNWWWW&quot;</code>.</p>

<p>Khoảng cách Manhattan lớn nhất từ gốc tọa độ có thể đạt được là 6. Do đó, đáp án là 6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= s.length</code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;N&#39;</code>, <code>&#39;S&#39;</code>, <code>&#39;E&#39;</code> và <code>&#39;W&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể thay đổi nhiều nhất $k$ bước và muốn tìm khoảng cách Manhattan lớn nhất của một prefix bất kỳ. Việc thử xem nên thay đổi những bước nào có độ phức tạp cấp số mũ.
>
> Khoảng cách được quyết định bởi một góc phần tư chi phối. Sau khi cố định một trong bốn đích chéo, các bước vốn đã hướng về đích đó đóng góp một đơn vị; các bước khác sẽ được đổi hướng khi vẫn còn $k$ lượt thay đổi, rồi bị trừ đi.
>
> Ta chạy greedy cho $\textit{SE}/\textit{SW}/\textit{NE}/\textit{NW}$ và giữ lại giá trị $\textit{mx}$ lớn nhất đã gặp trong quá trình đó.

<!-- thinking:end -->

Ta có thể liệt kê bốn trường hợp: $\textit{SE}$, $\textit{SW}$, $\textit{NE}$ và $\textit{NW}$, sau đó tính khoảng cách Manhattan lớn nhất cho mỗi trường hợp.

Ta định nghĩa hàm $\text{calc}(a, b)$ để tính khoảng cách Manhattan lớn nhất khi các hướng hiệu dụng là $\textit{a}$ và $\textit{b}$.

Ta định nghĩa biến $\textit{mx}$ để ghi nhận khoảng cách Manhattan hiện tại, biến $\textit{cnt}$ để ghi nhận số lần thay đổi đã thực hiện, và khởi tạo đáp án $\textit{ans}$ bằng $0$.

Duyệt chuỗi $\textit{s}$. Nếu ký tự hiện tại là $\textit{a}$ hoặc $\textit{b}$, ta tăng $\textit{mx}$ thêm $1$. Ngược lại, nếu $\textit{cnt} < k$, ta tăng $\textit{mx}$ thêm $1$ và tăng $\textit{cnt}$ thêm $1$. Nếu không, ta giảm $\textit{mx}$ đi $1$. Sau đó cập nhật $\textit{ans} = \max(\textit{ans}, \textit{mx})$.

Cuối cùng, trả về giá trị lớn nhất trong bốn trường hợp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, s: str, k: int) -> int:
        def calc(a: str, b: str) -> int:
            ans = mx = cnt = 0
            for c in s:
                if c == a or c == b:
                    mx += 1
                elif cnt < k:
                    cnt += 1
                    mx += 1
                else:
                    mx -= 1
                ans = max(ans, mx)
            return ans

        a = calc("S", "E")
        b = calc("S", "W")
        c = calc("N", "E")
        d = calc("N", "W")
        return max(a, b, c, d)
```

#### Java

```java
class Solution {
    private char[] s;
    private int k;

    public int maxDistance(String s, int k) {
        this.s = s.toCharArray();
        this.k = k;
        int a = calc('S', 'E');
        int b = calc('S', 'W');
        int c = calc('N', 'E');
        int d = calc('N', 'W');
        return Math.max(Math.max(a, b), Math.max(c, d));
    }

    private int calc(char a, char b) {
        int ans = 0, mx = 0, cnt = 0;
        for (char c : s) {
            if (c == a || c == b) {
                ++mx;
            } else if (cnt < k) {
                ++mx;
                ++cnt;
            } else {
                --mx;
            }
            ans = Math.max(ans, mx);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(string s, int k) {
        auto calc = [&](char a, char b) {
            int ans = 0, mx = 0, cnt = 0;
            for (char c : s) {
                if (c == a || c == b) {
                    ++mx;
                } else if (cnt < k) {
                    ++mx;
                    ++cnt;
                } else {
                    --mx;
                }
                ans = max(ans, mx);
            }
            return ans;
        };
        int a = calc('S', 'E');
        int b = calc('S', 'W');
        int c = calc('N', 'E');
        int d = calc('N', 'W');
        return max({a, b, c, d});
    }
};
```

#### Go

```go
func maxDistance(s string, k int) int {
	calc := func(a rune, b rune) int {
		var ans, mx, cnt int
		for _, c := range s {
			if c == a || c == b {
				mx++
			} else if cnt < k {
				mx++
				cnt++
			} else {
				mx--
			}
			ans = max(ans, mx)
		}
		return ans
	}
	a := calc('S', 'E')
	b := calc('S', 'W')
	c := calc('N', 'E')
	d := calc('N', 'W')
	return max(a, b, c, d)
}
```

#### TypeScript

```ts
function maxDistance(s: string, k: number): number {
    const calc = (a: string, b: string): number => {
        let [ans, mx, cnt] = [0, 0, 0];
        for (const c of s) {
            if (c === a || c === b) {
                ++mx;
            } else if (cnt < k) {
                ++mx;
                ++cnt;
            } else {
                --mx;
            }
            ans = Math.max(ans, mx);
        }
        return ans;
    };
    const a = calc('S', 'E');
    const b = calc('S', 'W');
    const c = calc('N', 'E');
    const d = calc('N', 'W');
    return Math.max(a, b, c, d);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distance(s: String, k: i32) -> i32 {
        fn calc(s: &str, a: char, b: char, k: i32) -> i32 {
            let mut ans = 0;
            let mut mx = 0;
            let mut cnt = 0;
            for c in s.chars() {
                if c == a || c == b {
                    mx += 1;
                } else if cnt < k {
                    mx += 1;
                    cnt += 1;
                } else {
                    mx -= 1;
                }
                ans = ans.max(mx);
            }
            ans
        }

        let a = calc(&s, 'S', 'E', k);
        let b = calc(&s, 'S', 'W', k);
        let c = calc(&s, 'N', 'E', k);
        let d = calc(&s, 'N', 'W', k);
        a.max(b).max(c).max(d)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
