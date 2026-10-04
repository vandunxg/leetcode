---
comments: true
difficulty: Medium
rating: 2473
source: Biweekly Contest 140 Q3
tags:
    - Greedy
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3302. Find the Lexicographically Smallest Valid Sequence](https://leetcode.com/problems/find-the-lexicographically-smallest-valid-sequence)

[中文文档](/solution/3300-3399/3302.Find%20the%20Lexicographically%20Smallest%20Valid%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>.</p>

<p>Một chuỗi <code>x</code> được gọi là <strong>gần bằng</strong> <code>y</code> nếu có thể thay đổi <strong>nhiều nhất</strong> một ký tự trong <code>x</code> để biến nó thành <em>giống hệt</em> <code>y</code>.</p>

<p>Một dãy chỉ số <code>seq</code> được gọi là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Các chỉ số được sắp xếp theo thứ tự <strong>tăng dần</strong>.</li>
	<li><em>Nối</em> các ký tự tại những chỉ số này trong <code>word1</code> theo <strong>đúng</strong> thứ tự đó sẽ tạo thành một chuỗi <strong>gần bằng</strong> <code>word2</code>.</li>
</ul>

<p>Trả về một mảng có kích thước <code>word2.length</code>, biểu diễn dãy chỉ số <strong>hợp lệ</strong> có thứ tự từ điển <span data-keyword="lexicographically-smaller-array">nhỏ nhất</span>. Nếu không tồn tại dãy chỉ số nào như vậy, trả về một mảng <strong>rỗng</strong>.</p>

<p><strong>Lưu ý</strong> rằng đáp án phải biểu diễn <em>mảng có thứ tự từ điển nhỏ nhất</em>, <strong>không phải</strong> chuỗi tương ứng được tạo bởi các chỉ số đó.<!-- notionvc: 2ff8e782-bd6f-4813-a421-ec25f7e84c1e --></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;vbcca&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy chỉ số hợp lệ có thứ tự từ điển nhỏ nhất là <code>[0, 1, 2]</code>:</p>

<ul>
	<li>Thay đổi <code>word1[0]</code> thành <code>&#39;a&#39;</code>.</li>
	<li><code>word1[1]</code> đã là <code>&#39;b&#39;</code>.</li>
	<li><code>word1[2]</code> đã là <code>&#39;c&#39;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;bacdc&quot;, word2 = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy chỉ số hợp lệ có thứ tự từ điển nhỏ nhất là <code>[1, 2, 4]</code>:</p>

<ul>
	<li><code>word1[1]</code> đã là <code>&#39;a&#39;</code>.</li>
	<li>Thay đổi <code>word1[2]</code> thành <code>&#39;b&#39;</code>.</li>
	<li><code>word1[4]</code> đã là <code>&#39;c&#39;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;aaaaaa&quot;, word2 = &quot;aaabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không tồn tại dãy chỉ số hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abc&quot;, word2 = &quot;ab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word2.length &lt; word1.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần một dãy con của $\textit{word1}$ khớp với $\textit{word2}$ sau nhiều nhất một lần thay đổi, đồng thời dãy chỉ số phải có thứ tự từ điển nhỏ nhất. Với $|\textit{word1}| \le 3 \times 10^5$, thử mọi vị trí thay đổi sẽ quá chậm.
>
> Khi có thể khớp, ta nên chọn các chỉ số nhỏ hơn. Câu hỏi còn lại là liệu có nên dùng lần thay đổi duy nhất cho một vị trí không khớp hay không.
>
> Quét từ phải sang trái để xây dựng $\textit{suf}[i]$, là vị trí nhỏ nhất của $\textit{word2}$ vẫn có thể khớp bắt đầu từ $i$. Khi quét từ trái sang phải, ta chọn một chỉ số nếu khớp chính xác; ngược lại, chỉ thay đổi khi chưa dùng lần thay đổi nào và $\textit{suf}[i+1] \le j+1$, để phần hậu tố vẫn có thể hoàn tất mẫu.

<!-- thinking:end -->

Trước tiên, chúng ta dùng two pointers để tiền xử lý một mảng hậu tố $\textit{suf}$ từ phải sang trái, trong đó $\textit{suf}[i]$ biểu diễn chỉ số bắt đầu nhỏ nhất trong $\textit{word2}$ sao cho $\textit{word2}[\textit{suf}[i]:]$ là một dãy con của $\textit{word1}[i:]$. Cụ thể, chúng ta dùng một con trỏ $j$ trỏ đến ký tự chưa khớp ở phía cuối của $\textit{word2}$, ban đầu $j = n - 1$, và đặt $\textit{suf}[m] = n$. Bắt đầu từ $i = m - 1$, chúng ta duyệt $\textit{word1}$ từ phải sang trái. Nếu $j \ge 0$ và $\textit{word1}[i] = \textit{word2}[j]$, điều đó có nghĩa là $\textit{word2}[j]$ có thể được khớp, nên chúng ta giảm $j$ đi một, sau đó đặt $\textit{suf}[i] = j + 1$.

Tiếp theo, chúng ta duyệt $\textit{word1}$ từ trái sang phải, dùng một con trỏ $j$ biểu diễn chỉ số của ký tự trong $\textit{word2}$ mà chúng ta cần khớp hiện tại (ban đầu $j = 0$), và một biến $\textit{changed}$ để ghi nhận liệu chúng ta đã thay đổi một ký tự hay chưa. Với mỗi ký tự $c$ tại chỉ số $i$:

- Nếu $c = \textit{word2}[j]$, việc chọn chỉ số $i$ luôn không tệ hơn (chỉ số càng nhỏ thì thứ tự từ điển của dãy càng nhỏ), nên chúng ta trực tiếp thêm $i$ vào đáp án và tăng $j$ lên một;
- Ngược lại, nếu chưa thay đổi ký tự nào và $\textit{suf}[i+1] \le j + 1$, điều đó có nghĩa là chúng ta có thể thay đổi $\textit{word1}[i]$ thành $\textit{word2}[j]$, đồng thời phần còn lại $\textit{word2}[j+1:]$ vẫn có thể được khớp trong $\textit{word1}[i+1:]$. Khi đó, chúng ta chọn chỉ số $i$ và đặt $\textit{changed}$ thành true.

Khi $j = n$, chúng ta đã khớp toàn bộ $\textit{word2}$ và có thể trả về đáp án. Nếu kết thúc quá trình duyệt mà vẫn chưa hoàn tất việc khớp, chúng ta trả về một mảng rỗng.

Độ phức tạp thời gian là $O(m + n)$, và độ phức tạp không gian là $O(m)$, trong đó $m$ và $n$ lần lượt là độ dài của các chuỗi $\textit{word1}$ và $\textit{word2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSequence(self, word1: str, word2: str) -> List[int]:
        m, n = len(word1), len(word2)
        suf = [0] * (m + 1)
        suf[m] = n
        j = n - 1
        for i in range(m - 1, -1, -1):
            if j >= 0 and word1[i] == word2[j]:
                j -= 1
            suf[i] = j + 1

        ans = []
        changed = False
        j = 0
        for i, c in enumerate(word1):
            if c == word2[j] or (not changed and suf[i + 1] <= j + 1):
                if c != word2[j]:
                    changed = True
                ans.append(i)
                j += 1
                if j == n:
                    return ans
        return []
```

#### Java

```java
class Solution {
    public int[] validSequence(String word1, String word2) {
        int m = word1.length(), n = word2.length();

        int[] suf = new int[m + 1];
        suf[m] = n;

        int j = n - 1;
        for (int i = m - 1; i >= 0; i--) {
            if (j >= 0 && word1.charAt(i) == word2.charAt(j)) {
                j--;
            }
            suf[i] = j + 1;
        }

        int[] ans = new int[n];
        int size = 0;
        boolean changed = false;
        j = 0;

        for (int i = 0; i < m; i++) {
            char c = word1.charAt(i);
            if (c == word2.charAt(j) || (!changed && suf[i + 1] <= j + 1)) {
                if (c != word2.charAt(j)) {
                    changed = true;
                }
                ans[size++] = i;
                j++;
                if (j == n) {
                    return ans;
                }
            }
        }

        return new int[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> validSequence(string word1, string word2) {
        int m = word1.size(), n = word2.size();

        vector<int> suf(m + 1);
        suf[m] = n;

        int j = n - 1;
        for (int i = m - 1; i >= 0; i--) {
            if (j >= 0 && word1[i] == word2[j]) {
                j--;
            }
            suf[i] = j + 1;
        }

        vector<int> ans;
        bool changed = false;
        j = 0;

        for (int i = 0; i < m; i++) {
            char c = word1[i];
            if (c == word2[j] || (!changed && suf[i + 1] <= j + 1)) {
                if (c != word2[j]) {
                    changed = true;
                }
                ans.push_back(i);
                j++;

                if (j == n) {
                    return ans;
                }
            }
        }

        return {};
    }
};
```

#### Go

```go
func validSequence(word1 string, word2 string) []int {
	m, n := len(word1), len(word2)

	suf := make([]int, m+1)
	suf[m] = n

	j := n - 1
	for i := m - 1; i >= 0; i-- {
		if j >= 0 && word1[i] == word2[j] {
			j--
		}
		suf[i] = j + 1
	}

	ans := make([]int, 0, n)
	changed := false
	j = 0

	for i := 0; i < m; i++ {
		c := word1[i]
		if c == word2[j] || (!changed && suf[i+1] <= j+1) {
			if c != word2[j] {
				changed = true
			}
			ans = append(ans, i)
			j++

			if j == n {
				return ans
			}
		}
	}

	return []int{}
}
```

#### TypeScript

```ts
function validSequence(word1: string, word2: string): number[] {
    const m = word1.length;
    const n = word2.length;

    const suf = new Array<number>(m + 1).fill(0);
    suf[m] = n;

    let j = n - 1;
    for (let i = m - 1; i >= 0; i--) {
        if (j >= 0 && word1[i] === word2[j]) {
            j--;
        }
        suf[i] = j + 1;
    }

    const ans: number[] = [];
    let changed = false;
    j = 0;

    for (let i = 0; i < m; i++) {
        const c = word1[i];

        if (c === word2[j] || (!changed && suf[i + 1] <= j + 1)) {
            if (c !== word2[j]) {
                changed = true;
            }

            ans.push(i);
            j++;

            if (j === n) {
                return ans;
            }
        }
    }

    return [];
}
```

#### Rust

```rust
impl Solution {
    pub fn valid_sequence(word1: String, word2: String) -> Vec<i32> {
        let word1_bytes = word1.as_bytes();
        let word2_bytes = word2.as_bytes();
        let mut positions = vec![-1i32; word2_bytes.len()];
        let mut word2_index = word2_bytes.len() as isize - 1;
        let mut word1_index = word1_bytes.len() as isize - 1;
        while word1_index >= 0 && word2_index >= 0 {
            if word1_bytes[word1_index as usize] == word2_bytes[word2_index as usize] {
                positions[word2_index as usize] = word1_index as i32;
                word2_index -= 1;
            }
            word1_index -= 1;
        }
        let mut mismatch_available = true;
        let mut matched_count = 0usize;
        for (index, &byte) in word1_bytes.iter().enumerate() {
            if matched_count == word2_bytes.len() {
                break;
            }
            if byte == word2_bytes[matched_count] {
                positions[matched_count] = index as i32;
                matched_count += 1;
            } else if mismatch_available
                && (matched_count + 1 == word2_bytes.len()
                    || (index as i32) < positions[matched_count + 1])
            {
                mismatch_available = false;
                positions[matched_count] = index as i32;
                matched_count += 1;
            }
        }
        if matched_count == word2_bytes.len() {
            positions
        } else {
            Vec::new()
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
