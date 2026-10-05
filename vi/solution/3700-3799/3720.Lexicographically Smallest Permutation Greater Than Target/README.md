---
comments: true
difficulty: Medium
rating: 1958
source: Weekly Contest 472 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [3720. Lexicographically Smallest Permutation Greater Than Target](https://leetcode.com/problems/lexicographically-smallest-permutation-greater-than-target)

[中文文档](/solution/3700-3799/3720.Lexicographically%20Smallest%20Permutation%20Greater%20Than%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>target</code>, cả hai đều có độ dài <code>n</code> và chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Hãy trả về <strong><span data-keyword="permutation-string">hoán vị</span> nhỏ nhất theo thứ tự từ điển</strong> của <code>s</code> lớn hơn <strong>nghiêm ngặt</strong> <code>target</code>. Nếu không có hoán vị nào của <code>s</code> lớn hơn <code>target</code> theo thứ tự từ điển một cách nghiêm ngặt, hãy trả về chuỗi rỗng.</p>

<p>Một chuỗi <code>a</code> <strong>lớn hơn nghiêm ngặt theo thứ tự từ điển</strong> một chuỗi <code>b</code> (có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, chuỗi <code>a</code> có chữ cái đứng sau chữ cái tương ứng trong <code>b</code> trong bảng chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, target = &quot;bba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;bca&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các hoán vị của <code>s</code> (theo thứ tự từ điển) là <code>&quot;abc&quot;</code>, <code>&quot;acb&quot;</code>, <code>&quot;bac&quot;</code>, <code>&quot;bca&quot;</code>, <code>&quot;cab&quot;</code> và <code>&quot;cba&quot;</code>.</li>
	<li>Hoán vị nhỏ nhất lớn hơn nghiêm ngặt <code>target</code> theo thứ tự từ điển là <code>&quot;bca&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leet&quot;, target = &quot;code&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;eelt&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các hoán vị của <code>s</code> (theo thứ tự từ điển) là <code>&quot;eelt&quot;</code>, <code>&quot;eetl&quot;</code>, <code>&quot;elet&quot;</code>, <code>&quot;elte&quot;</code>, <code>&quot;etel&quot;</code>, <code>&quot;etle&quot;</code>, <code>&quot;leet&quot;</code>, <code>&quot;lete&quot;</code>, <code>&quot;ltee&quot;</code>, <code>&quot;teel&quot;</code>, <code>&quot;tele&quot;</code> và <code>&quot;tlee&quot;</code>.</li>
	<li>Hoán vị nhỏ nhất lớn hơn nghiêm ngặt <code>target</code> theo thứ tự từ điển là <code>&quot;eelt&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;baba&quot;, target = &quot;bbaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các hoán vị của <code>s</code> (theo thứ tự từ điển) là <code>&quot;aabb&quot;</code>, <code>&quot;abab&quot;</code>, <code>&quot;abba&quot;</code>, <code>&quot;baab&quot;</code>, <code>&quot;baba&quot;</code> và <code>&quot;bbaa&quot;</code>.</li>
	<li>Không có hoán vị nào lớn hơn nghiêm ngặt <code>target</code> theo thứ tự từ điển. Vì vậy, đáp án là <code>&quot;&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length == target.length &lt;= 300</code></li>
	<li><code>s</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị lớn hơn nghiêm ngặt $\textit{target}$ có dạng “tiền tố chung + một chữ cái lớn hơn + phần còn lại theo thứ tự tăng dần”, và tiền tố càng dài thì kết quả càng nhỏ theo thứ tự từ điển. Ta khớp với $\textit{target}$ nhiều nhất có thể theo số lượng chữ cái, sau đó lùi lại và tại vị trí khả thi đầu tiên đặt chữ cái nhỏ nhất còn lại lớn hơn $\textit{target}[i]$.

<!-- thinking:end -->

Để lớn hơn nghiêm ngặt $\textit{target}$, đáp án phải có dạng: khớp chính xác với một tiền tố của $\textit{target}$, đặt một ký tự lớn hơn ký tự tương ứng của $\textit{target}$ ở vị trí tiếp theo, rồi sắp xếp các ký tự còn lại theo thứ tự tăng dần. Tiền tố chung càng dài thì hoán vị tạo được càng nhỏ, vì vậy ta muốn tiền tố chung dài nhất có thể.

Trước tiên, ta đếm số lần xuất hiện của mỗi ký tự trong $s$ vào $\textit{cnt}$, sau đó khớp $\textit{target}$ từ trái sang phải nhiều nhất có thể: chừng nào ký tự hiện tại còn khả dụng, ta lấy ký tự đó và thêm vào đáp án, dừng lại khi một ký tự nào đó hết. Kết quả là tiền tố chung dài nhất.

Tiếp theo, ta lùi từ cuối tiền tố này, thử từng vị trí $i$ làm nơi đáp án rẽ hướng: đặt ký tự nhỏ nhất còn khả dụng lớn hơn $\textit{target}[i]$ tại vị trí $i$, nếu thành công thì thêm các ký tự còn lại theo thứ tự tăng dần và trả về. Nếu không, ta trả $\textit{target}[i - 1]$ về $\textit{cnt}$ rồi thử một vị trí sớm hơn. Lưu ý rằng khi bản thân $\textit{target}$ là một hoán vị của $s$, không thể đặt ký tự nào ở vị trí $n$, vì vậy ta phải bắt đầu quay lui từ vị trí cuối cùng do đáp án phải lớn hơn nghiêm ngặt. Nếu mọi vị trí đều thất bại, không tồn tại hoán vị phù hợp và ta trả về chuỗi rỗng.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, còn độ phức tạp không gian là $O(n + |\Sigma|)$. Ở đây, $n$ là độ dài của chuỗi $s$, và $|\Sigma| = 26$ là kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lexGreaterPermutation(self, s: str, target: str) -> str:
        cnt = Counter(s)
        n = len(target)
        ans = []
        for c in target:
            if cnt[c] == 0:
                break
            cnt[c] -= 1
            ans.append(c)
        for i in range(len(ans), -1, -1):
            if i < n:
                for c in ascii_lowercase:
                    if c > target[i] and cnt[c] > 0:
                        cnt[c] -= 1
                        rest = ''.join(x * cnt[x] for x in ascii_lowercase)
                        return ''.join(ans[:i]) + c + rest
            if i > 0:
                cnt[ans[i - 1]] += 1
        return ''
```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

#### Rust

```rust
impl Solution {
    pub fn lex_greater_permutation(s: String, target: String) -> String {
        let mut permutation = s.into_bytes();
        let target_bytes = target.as_bytes();
        let mut letter_counts = [0usize; 26];
        for &byte in &permutation {
            letter_counts[(byte - b'a') as usize] += 1;
        }
        let mut prefix_length = 0;
        while prefix_length < target_bytes.len() {
            let target_letter = (target_bytes[prefix_length] - b'a') as usize;
            if letter_counts[target_letter] == 0 {
                break;
            }
            permutation[prefix_length] = target_bytes[prefix_length];
            letter_counts[target_letter] -= 1;
            prefix_length += 1;
        }
        loop {
            if prefix_length < target_bytes.len() {
                let next_letter = (target_bytes[prefix_length] - b'a') as usize + 1;
                if let Some(replacement_letter) =
                    (next_letter..26).find(|&letter| letter_counts[letter] > 0)
                {
                    permutation[prefix_length] = b'a' + replacement_letter as u8;
                    letter_counts[replacement_letter] -= 1;
                    let mut write_index = prefix_length + 1;
                    for (letter, &count) in letter_counts.iter().enumerate() {
                        for _ in 0..count {
                            permutation[write_index] = b'a' + letter as u8;
                            write_index += 1;
                        }
                    }
                    return String::from_utf8(permutation).unwrap();
                }
            }
            if prefix_length == 0 {
                return String::new();
            }
            prefix_length -= 1;
            letter_counts[(target_bytes[prefix_length] - b'a') as usize] += 1;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
