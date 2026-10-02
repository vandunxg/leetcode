---
comments: true
difficulty: Medium
rating: 1749
source: Weekly Contest 144 Q4
tags:
    - Stack
    - String
    - Parentheses
---

<!-- problem:start -->

# [1111. Maximum Nesting Depth of Two Valid Parentheses Strings](https://leetcode.com/problems/maximum-nesting-depth-of-two-valid-parentheses-strings)

[中文文档](/solution/1100-1199/1111.Maximum%20Nesting%20Depth%20of%20Two%20Valid%20Parentheses%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi là <em>chuỗi ngoặc hợp lệ</em>&nbsp;(ký hiệu là VPS) khi và chỉ khi chuỗi chỉ gồm các ký tự <code>&quot;(&quot;</code> và <code>&quot;)&quot;</code>, đồng thời thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li>Đó là chuỗi rỗng, hoặc</li>
	<li>Có thể viết thành&nbsp;<code>AB</code>&nbsp;(<code>A</code>&nbsp;được nối với&nbsp;<code>B</code>), trong đó&nbsp;<code>A</code>&nbsp;và&nbsp;<code>B</code>&nbsp;đều là VPS, hoặc</li>
	<li>Có thể viết thành&nbsp;<code>(A)</code>, trong đó&nbsp;<code>A</code>&nbsp;là một VPS.</li>
</ul>

<p>Tương tự, ta định nghĩa <em>độ sâu lồng nhau</em> <code>depth(S)</code> của một VPS bất kỳ <code>S</code> như sau:</p>

<ul>
	<li><code>depth(&quot;&quot;) = 0</code></li>
	<li><code>depth(A + B) = max(depth(A), depth(B))</code>, trong đó <code>A</code> và <code>B</code> là các VPS</li>
	<li><code>depth(&quot;(&quot; + A + &quot;)&quot;) = 1 + depth(A)</code>, trong đó <code>A</code> là một VPS.</li>
</ul>

<p>Ví dụ, <code>&quot;&quot;</code>,&nbsp;<code>&quot;()()&quot;</code> và&nbsp;<code>&quot;()(()())&quot;</code>&nbsp;là các VPS (có độ sâu lồng nhau lần lượt là 0, 1 và 2), còn <code>&quot;)(&quot;</code> và <code>&quot;(()&quot;</code> không phải VPS.</p>

<p>Cho một VPS <font face="monospace">seq</font>, hãy chia nó thành hai dãy con rời nhau <code>A</code> và <code>B</code>, sao cho&nbsp;<code>A</code> và <code>B</code> đều là VPS (và&nbsp;<code>A.length + B.length = seq.length</code>). Các dãy con không nhất thiết phải gồm những ký tự liên tiếp.</p>

<p>Ví dụ, với dãy <code>123456789</code>, một cách chia có thể là:</p>

<ul data-end="822" data-start="776">
	<li data-end="800" data-start="776">
	<p data-end="800" data-start="778"><code data-end="799" data-start="778">A = {1, 3, 5, 7, 9}</code>,</p>
	</li>
	<li data-end="822" data-start="801">
	<p data-end="822" data-start="803"><code data-end="821" data-start="803">B = {2, 4, 6, 8}</code>.</p>
	</li>
</ul>

<p data-end="855" data-start="824">Cách chia này tương ứng với output <code>[0, 1, 0, 1, 0, 1, 0, 1, 0]</code> &nbsp;trong đó 0 cho biết phần tử thuộc&nbsp;<code data-end="929" data-start="926">A</code>&nbsp;và 1 cho biết phần tử thuộc&nbsp;<code data-end="965" data-start="962">B</code>.</p>

<p>Bây giờ, hãy chọn <strong>bất kỳ</strong> cách chia <code>A</code> và <code>B</code> nào sao cho&nbsp;<code>max(depth(A), depth(B))</code> đạt giá trị nhỏ nhất có thể.</p>

<p>Trả về mảng <code>answer</code> (có độ dài <code>seq.length</code>) mã hóa cách chia đó: <code>answer[i] = 0</code> nếu <code>seq[i]</code> thuộc <code>A</code>, ngược lại <code>answer[i] = 1</code>.&nbsp; Lưu ý rằng có thể có nhiều đáp án hợp lệ, bạn có thể trả về bất kỳ đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> seq = &quot;(()())&quot;
<strong>Đầu ra:</strong> [0,1,1,1,1,0]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> seq = &quot;()(())()&quot;
<strong>Đầu ra:</strong> [0,0,0,1,1,0,1,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= seq.size &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dấu ngoặc phải được gán vào một nhóm, đồng thời cả hai nhóm phải luôn là chuỗi hợp lệ. Với $n \le 10^4$, việc thử mọi cách gán cho từng vị trí là không khả thi. Ta cũng có thể duyệt greedy: đưa mỗi `'('` vào nhóm hiện nông hơn, rồi đưa dấu `')'` đóng cặp với nó vào cùng nhóm. Cách này đúng, nhưng khi duyệt cần theo dõi độ sâu của cả hai nhóm.
>
> Độ sâu tăng thêm một lớp lồng nhau mỗi lần. Một chuỗi có độ sâu $d$ gồm $d$ lớp, vì vậy có ít nhất một nhóm phải nhận $\lceil d/2 \rceil$ lớp. Nếu lần lượt phân các lớp liền kề cho hai nhóm, độ sâu của chúng sẽ là $\lceil d/2 \rceil$ và $\lfloor d/2 \rfloor$.
>
> Có thể dùng tính chẵn lẻ của lớp làm id nhóm, nên chỉ cần theo dõi một giá trị cân bằng $x$. Dấu `'('` nằm ở lớp sắp đi vào, vì vậy ta gán nhãn bằng $x$ trước khi tăng độ sâu. Dấu `')'` đóng cặp với nó phải theo cùng quy tắc, nên ta gán nhãn bằng $x$ sau khi giảm độ sâu. Cả hai dấu trong một cặp có cùng tính chẵn lẻ, và hai dãy con đều vẫn hợp lệ.

<!-- thinking:end -->

Ta dùng biến $x$ để ghi nhận balance hiện tại, tức số dấu ngoặc trái chưa được ghép cặp. Đây cũng là độ sâu lồng nhau tại vị trí hiện tại.

Duyệt $seq$ từ trái sang phải. Gặp dấu ngoặc trái, ghi tính chẵn lẻ của $x$ vào answer: ghi $0$ nếu $x$ chẵn, $1$ nếu $x$ lẻ, rồi tăng $x$. Gặp dấu ngoặc phải, giảm $x$ trước rồi ghi tính chẵn lẻ của giá trị mới. Hai dấu trong một cặp có cùng tính chẵn lẻ nên được đưa vào cùng nhóm; do đó cả hai nhóm vẫn là chuỗi ngoặc hợp lệ. Nếu độ sâu lồng nhau ban đầu là $d$, độ sâu lớn hơn trong hai nhóm là $\lceil d/2 \rceil$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của $seq$. Không tính mảng answer, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDepthAfterSplit(self, seq: str) -> List[int]:
        ans = [0] * len(seq)
        x = 0
        for i, c in enumerate(seq):
            if c == "(":
                ans[i] = x & 1
                x += 1
            else:
                x -= 1
                ans[i] = x & 1
        return ans
```

#### Java

```java
class Solution {
    public int[] maxDepthAfterSplit(String seq) {
        int n = seq.length();
        int[] ans = new int[n];
        for (int i = 0, x = 0; i < n; ++i) {
            if (seq.charAt(i) == '(') {
                ans[i] = x++ & 1;
            } else {
                ans[i] = --x & 1;
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
    vector<int> maxDepthAfterSplit(string seq) {
        int n = seq.size();
        vector<int> ans(n);
        for (int i = 0, x = 0; i < n; ++i) {
            if (seq[i] == '(') {
                ans[i] = x++ & 1;
            } else {
                ans[i] = --x & 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDepthAfterSplit(seq string) []int {
	n := len(seq)
	ans := make([]int, n)
	for i, x := 0, 0; i < n; i++ {
		if seq[i] == '(' {
			ans[i] = x & 1
			x++
		} else {
			x--
			ans[i] = x & 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxDepthAfterSplit(seq: string): number[] {
    const n = seq.length;
    const ans: number[] = new Array(n);
    for (let i = 0, x = 0; i < n; ++i) {
        if (seq[i] === '(') {
            ans[i] = x++ & 1;
        } else {
            ans[i] = --x & 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_depth_after_split(seq: String) -> Vec<i32> {
        let n = seq.len();
        let mut ans = vec![0; n];
        let mut x = 0;
        for (i, c) in seq.bytes().enumerate() {
            if c == b'(' {
                ans[i] = x & 1;
                x += 1;
            } else {
                x -= 1;
                ans[i] = x & 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
