---
comments: true
difficulty: Easy
rating: 1468
source: Biweekly Contest 115 Q2
tags:
    - Greedy
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2900. Longest Unequal Adjacent Groups Subsequence I](https://leetcode.com/problems/longest-unequal-adjacent-groups-subsequence-i)

[中文文档](/solution/2900-2999/2900.Longest%20Unequal%20Adjacent%20Groups%20Subsequence%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một mảng <strong>nhị phân</strong> <code>groups</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Một <span data-keyword="subsequence-array">dãy con</span> của <code>words</code> được gọi là <strong>luân phiên</strong> nếu với mọi hai chuỗi <em>liên tiếp</em> trong dãy, các phần tử tương ứng tại <em>cùng</em> chỉ số trong <code>groups</code> là <strong>khác nhau</strong> (nghĩa là <em>không thể</em> có hai số 0 hoặc hai số 1 liên tiếp).</p>

<p>Nhiệm vụ của bạn là chọn một dãy con <strong>luân phiên dài nhất</strong> từ <code>words</code>.</p>

<p>Trả về <em>dãy con được chọn. Nếu có nhiều đáp án, trả về <strong>bất kỳ</strong> đáp án nào trong số đó.</em></p>

<p><strong>Lưu ý:</strong> Các phần tử trong <code>words</code> là đôi một khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">words = [&quot;e&quot;,&quot;a&quot;,&quot;b&quot;], groups = [0,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">[&quot;e&quot;,&quot;b&quot;]</span></p>

<p><strong>Giải thích:</strong> Một dãy con có thể được chọn là <code>[&quot;e&quot;,&quot;b&quot;]</code> vì <code>groups[0] != groups[2]</code>. Một dãy con khác có thể được chọn là <code>[&quot;a&quot;,&quot;b&quot;]</code> vì <code>groups[1] != groups[2]</code>. Có thể chứng minh rằng độ dài lớn nhất của dãy chỉ số thỏa mãn điều kiện là <code>2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;], groups = [1,0,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]</span></p>

<p><strong>Giải thích:</strong> Một dãy con có thể được chọn là <code>[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]</code> vì <code>groups[0] != groups[1]</code> và <code>groups[1] != groups[2]</code>. Một dãy con khác có thể được chọn là <code>[&quot;a&quot;,&quot;b&quot;,&quot;d&quot;]</code> vì <code>groups[0] != groups[1]</code> và <code>groups[1] != groups[3]</code>. Có thể chứng minh rằng độ dài lớn nhất của dãy chỉ số thỏa mãn điều kiện là <code>3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == words.length == groups.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>groups[i]</code> là <code>0</code> hoặc <code>1.</code></li>
	<li><code>words</code> gồm các chuỗi <strong>đôi một khác nhau</strong>.</li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 100$, ta có thể liệt kê các dãy con hoặc dùng quy hoạch động $O(n^2)$ để tìm độ dài lớn nhất. $groups$ là mảng nhị phân, nên hai phần tử được chọn liên tiếp chỉ hợp lệ khi nhóm thay đổi; do đó, mỗi đoạn liên tiếp gồm các nhóm giống nhau chỉ cần giữ lại nhiều nhất một chỉ số.
>
> Giữ lại chỉ số đầu tiên của mỗi đoạn vừa nối được với đoạn trước, vừa không làm giảm các lựa chọn về sau. Vì mọi dãy con dài nhất đều được chấp nhận nên không cần so sánh các phần tử trong $words$ ở cùng một đoạn. Chỉ cần duyệt từ trái sang phải một lần để xây dựng đáp án.

<!-- thinking:end -->

Ta có thể duyệt mảng $groups$. Với chỉ số hiện tại $i$, nếu $i=0$ hoặc $groups[i] \neq groups[i - 1]$, ta thêm $words[i]$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $groups$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLongestSubsequence(self, words: List[str], groups: List[int]) -> List[str]:
        return [words[i] for i, x in enumerate(groups) if i == 0 or x != groups[i - 1]]
```

#### Java

```java
class Solution {
    public List<String> getLongestSubsequence(String[] words, int[] groups) {
        int n = groups.length;
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (i == 0 || groups[i] != groups[i - 1]) {
                ans.add(words[i]);
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
    vector<string> getLongestSubsequence(vector<string>& words, vector<int>& groups) {
        int n = groups.size();
        vector<string> ans;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || groups[i] != groups[i - 1]) {
                ans.emplace_back(words[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getLongestSubsequence(words []string, groups []int) (ans []string) {
	for i, x := range groups {
		if i == 0 || x != groups[i-1] {
			ans = append(ans, words[i])
		}
	}
	return
}
```

#### TypeScript

```ts
function getLongestSubsequence(words: string[], groups: number[]): string[] {
    const ans: string[] = [];
    for (let i = 0; i < groups.length; ++i) {
        if (i === 0 || groups[i] !== groups[i - 1]) {
            ans.push(words[i]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_longest_subsequence(words: Vec<String>, groups: Vec<i32>) -> Vec<String> {
        let mut ans = Vec::new();
        for (i, &g) in groups.iter().enumerate() {
            if i == 0 || g != groups[i - 1] {
                ans.push(words[i].clone());
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
