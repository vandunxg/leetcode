---
comments: true
difficulty: Easy
rating: 1205
source: Weekly Contest 394 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3120. Count the Number of Special Characters I](https://leetcode.com/problems/count-the-number-of-special-characters-i)

[中文文档](/solution/3100-3199/3120.Count%20the%20Number%20of%20Special%20Characters%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>. Một chữ cái được gọi là <strong>đặc biệt</strong> nếu nó xuất hiện <strong>cả</strong> ở dạng viết thường và viết hoa trong <code>word</code>.</p>

<p>Hãy trả về số lượng chữ cái <em> </em><strong>đặc biệt</strong> trong<em> </em><code>word</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aaAbcBC&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chữ cái đặc biệt trong <code>word</code> là <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ký tự nào trong <code>word</code> xuất hiện ở dạng viết hoa.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abBCab&quot;</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Chữ cái đặc biệt duy nhất trong <code>word</code> là <code>&#39;b&#39;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 50</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Một chữ cái đặc biệt khi cả dạng viết thường và viết hoa đều xuất hiện. Nếu kiểm tra lại chuỗi cho từng chữ cái, ta sẽ lặp lại công việc $O(n|\Sigma|)$.
>
> Chỉ cần biết mỗi ký tự có xuất hiện hay không; một set được xây dựng trong một lượt duyệt có thể trả lời cho cả $26$ cặp.
>
> Đưa $word$ vào một set, sau đó đếm các chữ cái mà cả dạng viết thường và viết hoa đều xuất hiện.

<!-- thinking:end -->

Ta dùng một hash table hoặc mảng $s$ để ghi nhận các ký tự xuất hiện trong chuỗi $word$. Sau đó, ta duyệt qua 26 chữ cái. Nếu cả chữ cái viết thường và viết hoa đều xuất hiện trong $s$, ta tăng số lượng chữ cái đặc biệt lên một.

Cuối cùng, trả về số lượng chữ cái đặc biệt.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $word$, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| \leq 128$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSpecialChars(self, word: str) -> int:
        s = set(word)
        return sum(a in s and b in s for a, b in zip(ascii_lowercase, ascii_uppercase))
```

#### Java

```java
class Solution {
    public int numberOfSpecialChars(String word) {
        boolean[] s = new boolean['z' + 1];
        for (int i = 0; i < word.length(); ++i) {
            s[word.charAt(i)] = true;
        }
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            if (s['a' + i] && s['A' + i]) {
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
    int numberOfSpecialChars(string word) {
        vector<bool> s('z' + 1);
        for (char& c : word) {
            s[c] = true;
        }
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            ans += s['a' + i] && s['A' + i];
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSpecialChars(word string) (ans int) {
	s := make([]bool, 'z'+1)
	for _, c := range word {
		s[c] = true
	}
	for i := 0; i < 26; i++ {
		if s['a'+i] && s['A'+i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfSpecialChars(word: string): number {
    const s: boolean[] = Array.from({ length: 'z'.charCodeAt(0) + 1 }, () => false);
    for (let i = 0; i < word.length; ++i) {
        s[word.charCodeAt(i)] = true;
    }
    let ans: number = 0;
    for (let i = 0; i < 26; ++i) {
        if (s['a'.charCodeAt(0) + i] && s['A'.charCodeAt(0) + i]) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_special_chars(word: String) -> i32 {
        let mut s = [false; 128];
        for ch in word.chars() {
            s[ch as u8 as usize] = true;
        }
        let mut ans = 0;
        for i in 0..26 {
            if s[(b'a' + i) as usize] && s[(b'A' + i) as usize] {
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
