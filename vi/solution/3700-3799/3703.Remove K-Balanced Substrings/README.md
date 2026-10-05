---
comments: true
difficulty: Medium
rating: 1802
source: Weekly Contest 470 Q3
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [3703. Remove K-Balanced Substrings](https://leetcode.com/problems/remove-k-balanced-substrings)

[中文文档](/solution/3700-3799/3703.Remove%20K-Balanced%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>, cùng một số nguyên <code>k</code>.</p>

<p>Một <strong>chuỗi</strong> được gọi là <strong>k-cân bằng</strong> nếu nó gồm <strong>chính xác</strong> <code>k</code> ký tự <code>&#39;(&#39;</code> <strong>liên tiếp</strong>, theo sau bởi <code>k</code> ký tự <code>&#39;)&#39;</code> <strong>liên tiếp</strong>, tức là <code>&#39;(&#39; * k + &#39;)&#39; * k</code>.</p>

<p>Ví dụ, nếu <code>k = 3</code>, chuỗi k-cân bằng là <code>&quot;((()))&quot;</code>.</p>

<p>Bạn phải <strong>liên tục</strong> xóa tất cả các <strong><span data-keyword="substring-nonempty">chuỗi con</span> k-cân bằng không chồng lấn</strong> khỏi <code>s</code>, sau đó nối các phần còn lại. Tiếp tục quá trình này cho đến khi không còn <strong>chuỗi con</strong> k-cân bằng nào tồn tại.</p>

<p>Trả về chuỗi cuối cùng sau khi thực hiện mọi lần xóa có thể.</p>

<p>&nbsp;</p>
<p>​​​​​​​<strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;(())&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con k-cân bằng là <code>&quot;()&quot;</code></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Bước</th>
			<th style="border: 1px solid black;">Chữ <code>s</code> hiện tại</th>
			<th style="border: 1px solid black;"><code>k-balanced</code></th>
			<th style="border: 1px solid black;">Chữ <code>s</code> sau đó</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>(())</code></td>
			<td style="border: 1px solid black;"><code>(<s><strong>()</strong></s>)</code></td>
			<td style="border: 1px solid black;"><code>()</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>()</code></td>
			<td style="border: 1px solid black;"><s><strong><code>()</code></strong></s></td>
			<td style="border: 1px solid black;">Rỗng</td>
		</tr>
	</tbody>
</table>

<p>Vậy chuỗi cuối cùng là <code>&quot;&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;(()(&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;((&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con k-cân bằng là <code>&quot;()&quot;</code></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Bước</th>
			<th style="border: 1px solid black;">Chữ <code>s</code> hiện tại</th>
			<th style="border: 1px solid black;"><code>k-balanced</code></th>
			<th style="border: 1px solid black;">Chữ <code>s</code> sau đó</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>(()(</code></td>
			<td style="border: 1px solid black;"><code>(<s><strong>()</strong></s>(</code></td>
			<td style="border: 1px solid black;"><code>((</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>((</code></td>
			<td style="border: 1px solid black;">-</td>
			<td style="border: 1px solid black;"><code>((</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy chuỗi cuối cùng là <code>&quot;((&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;((()))()()()&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;()()()&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con k-cân bằng là <code>&quot;((()))&quot;</code></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Bước</th>
			<th style="border: 1px solid black;">Chữ <code>s</code> hiện tại</th>
			<th style="border: 1px solid black;"><code>k-balanced</code></th>
			<th style="border: 1px solid black;">Chữ <code>s</code> sau đó</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;"><code>((()))()()()</code></td>
			<td style="border: 1px solid black;"><code><s><strong>((()))</strong></s>()()()</code></td>
			<td style="border: 1px solid black;"><code>()()()</code></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><code>()()()</code></td>
			<td style="border: 1px solid black;">-</td>
			<td style="border: 1px solid black;"><code>()()()</code></td>
		</tr>
	</tbody>
</table>

<p>Vậy chuỗi cuối cùng là <code>&quot;()()()&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm <code>&#39;(&#39;</code> và <code>&#39;)&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= s.length / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Việc liên tục tìm và xóa các đoạn $k$-cân bằng sẽ khiến ta phải quét lại cùng một vùng nhiều lần. Các ký tự liên tiếp giống nhau có thể được nén lại, vì vậy stack chỉ cần lưu một ký tự và số lần xuất hiện của nó. Khi phần tử trên cùng chứa chính xác $k$ dấu ngoặc đóng và đoạn bên dưới chứa ít nhất $k$ dấu ngoặc mở, ta xóa chúng ngay lập tức, nhờ đó các lần xóa liên tiếp được hoàn tất trong một lượt duyệt.

<!-- thinking:end -->

Ta sử dụng một stack để duy trì trạng thái hiện tại của chuỗi. Mỗi phần tử trong stack là một cặp biểu diễn một ký tự và số lần xuất hiện liên tiếp của ký tự đó.

Duyệt qua từng ký tự trong chuỗi:

- Nếu stack không rỗng và ký tự của phần tử trên cùng trùng với ký tự hiện tại, tăng số đếm của phần tử trên cùng.
- Nếu không, thêm ký tự hiện tại cùng số đếm 1 vào stack như một phần tử mới.
- Nếu ký tự hiện tại là `')'`, stack có ít nhất hai phần tử, số đếm của phần tử trên cùng bằng $k$, và số đếm của phần tử trước đó lớn hơn hoặc bằng $k$, thì lấy phần tử trên cùng ra và trừ $k$ khỏi số đếm của phần tử trước đó. Nếu số đếm của phần tử trước đó trở thành 0, lấy phần tử đó ra khỏi stack.

Sau khi duyệt xong, các phần tử còn lại trong stack biểu diễn trạng thái cuối cùng của chuỗi. Ta nối các phần tử này theo thứ tự để thu được chuỗi kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeSubstring(self, s: str, k: int) -> str:
        stk = []
        for c in s:
            if stk and stk[-1][0] == c:
                stk[-1][1] += 1
            else:
                stk.append([c, 1])
            if c == ")" and len(stk) > 1 and stk[-1][1] == k and stk[-2][1] >= k:
                stk.pop()
                stk[-1][1] -= k
                if stk[-1][1] == 0:
                    stk.pop()
        return "".join(c * v for c, v in stk)
```

#### Java

```java
class Solution {
    public String removeSubstring(String s, int k) {
        List<int[]> stk = new ArrayList<>();
        for (char c : s.toCharArray()) {
            if (!stk.isEmpty() && stk.get(stk.size() - 1)[0] == c) {
                stk.get(stk.size() - 1)[1] += 1;
            } else {
                stk.add(new int[] {c, 1});
            }
            if (c == ')' && stk.size() > 1) {
                int[] top = stk.get(stk.size() - 1);
                int[] prev = stk.get(stk.size() - 2);
                if (top[1] == k && prev[1] >= k) {
                    stk.remove(stk.size() - 1);
                    prev[1] -= k;
                    if (prev[1] == 0) {
                        stk.remove(stk.size() - 1);
                    }
                }
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int[] pair : stk) {
            for (int i = 0; i < pair[1]; i++) {
                sb.append((char) pair[0]);
            }
        }
        return sb.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeSubstring(string s, int k) {
        vector<pair<char, int>> stk;
        for (char c : s) {
            if (!stk.empty() && stk.back().first == c) {
                stk.back().second += 1;
            } else {
                stk.emplace_back(c, 1);
            }
            if (c == ')' && stk.size() > 1) {
                auto& top = stk.back();
                auto& prev = stk[stk.size() - 2];
                if (top.second == k && prev.second >= k) {
                    stk.pop_back();
                    prev.second -= k;
                    if (prev.second == 0) {
                        stk.pop_back();
                    }
                }
            }
        }
        string res;
        for (auto& p : stk) {
            res.append(p.second, p.first);
        }
        return res;
    }
};
```

#### Go

```go
func removeSubstring(s string, k int) string {
	type pair struct {
		ch    byte
		count int
	}
	stk := make([]pair, 0)
	for i := 0; i < len(s); i++ {
		c := s[i]
		if len(stk) > 0 && stk[len(stk)-1].ch == c {
			stk[len(stk)-1].count++
		} else {
			stk = append(stk, pair{c, 1})
		}
		if c == ')' && len(stk) > 1 {
			top := &stk[len(stk)-1]
			prev := &stk[len(stk)-2]
			if top.count == k && prev.count >= k {
				stk = stk[:len(stk)-1]
				prev.count -= k
				if prev.count == 0 {
					stk = stk[:len(stk)-1]
				}
			}
		}
	}
	res := make([]byte, 0)
	for _, p := range stk {
		for i := 0; i < p.count; i++ {
			res = append(res, p.ch)
		}
	}
	return string(res)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
