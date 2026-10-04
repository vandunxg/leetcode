---
comments: true
difficulty: Medium
rating: 1411
source: Weekly Contest 394 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3121. Count the Number of Special Characters II](https://leetcode.com/problems/count-the-number-of-special-characters-ii)

[中文文档](/solution/3100-3199/3121.Count%20the%20Number%20of%20Special%20Characters%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>. Một chữ cái&nbsp;<code>c</code> được gọi là <strong>đặc biệt</strong> nếu nó xuất hiện <strong>cả</strong> ở dạng chữ thường và chữ hoa trong <code>word</code>, đồng thời <strong>mọi</strong> lần xuất hiện của <code>c</code> ở dạng chữ thường đều nằm trước <strong>lần xuất hiện đầu tiên</strong> của <code>c</code> ở dạng chữ hoa.</p>

<p>Trả về số lượng<em> </em><strong>chữ cái đặc biệt</strong><em> </em>trong<em> </em><code>word</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aaAbcBC&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các ký tự đặc biệt là <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ký tự đặc biệt nào trong <code>word</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;AbBCab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ký tự đặc biệt nào trong <code>word</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường và viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Ở đây, mọi lần xuất hiện của chữ cái viết thường cũng phải đứng trước lần xuất hiện đầu tiên của chữ cái viết hoa tương ứng. Nếu chỉ kiểm tra sự tồn tại, ta có thể chấp nhận một lần xuất hiện chữ thường ở phía sau.
>
> Chỉ cần so sánh chỉ số cuối cùng của chữ thường với chỉ số đầu tiên của chữ hoa, cả hai đều có thể lấy được trong một lần duyệt.
>
> Ghi nhận $first$ và $last$ trong khi duyệt qua $word$, sau đó đếm những chữ cái có chỉ số cuối cùng của chữ thường nhỏ hơn chỉ số đầu tiên của chữ hoa.

<!-- thinking:end -->

Ta định nghĩa hai hash table hoặc mảng `first` và `last` để lưu vị trí xuất hiện đầu tiên và cuối cùng của mỗi chữ cái tương ứng.

Sau đó, ta duyệt qua chuỗi `word`, cập nhật `first` và `last`.

Cuối cùng, ta duyệt qua tất cả các chữ cái viết thường và viết hoa. Nếu `last[a]` tồn tại, `first[b]` tồn tại và `last[a] < first[b]`, điều đó có nghĩa chữ cái `a` là chữ cái đặc biệt, khi đó ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi `word`, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| \leq 128$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSpecialChars(self, word: str) -> int:
        first, last = {}, {}
        for i, c in enumerate(word):
            if c not in first:
                first[c] = i
            last[c] = i
        return sum(
            a in last and b in first and last[a] < first[b]
            for a, b in zip(ascii_lowercase, ascii_uppercase)
        )
```

#### Java

```java
class Solution {
    public int numberOfSpecialChars(String word) {
        int[] first = new int['z' + 1];
        int[] last = new int['z' + 1];
        for (int i = 1; i <= word.length(); ++i) {
            int j = word.charAt(i - 1);
            if (first[j] == 0) {
                first[j] = i;
            }
            last[j] = i;
        }
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            int a = 'a' + i;
            int b = 'A' + i;
            if (last[a] > 0 && last[a] < first[b]) {
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
        vector<int> first('z' + 1);
        vector<int> last('z' + 1);
        for (int i = 1; i <= word.size(); ++i) {
            int j = word[i - 1];
            if (first[j] == 0) {
                first[j] = i;
            }
            last[j] = i;
        }
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            if (last['a' + i] && last['a' + i] < first['A' + i]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSpecialChars(word string) (ans int) {
	first := make([]int, 'z'+1)
	last := make([]int, 'z'+1)
	for i, c := range word {
		if first[c] == 0 {
			first[c] = i + 1
		}
		last[c] = i + 1
	}
	for i := 0; i < 26; i++ {
		if last['a'+i] > 0 && last['a'+i] < first['A'+i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfSpecialChars(word: string): number {
    const first: number[] = Array.from({ length: 'z'.charCodeAt(0) + 1 }, () => 0);
    const last: number[] = Array.from({ length: 'z'.charCodeAt(0) + 1 }, () => 0);
    for (let i = 0; i < word.length; ++i) {
        const j = word.charCodeAt(i);
        if (first[j] === 0) {
            first[j] = i + 1;
        }
        last[j] = i + 1;
    }
    let ans: number = 0;
    for (let i = 0; i < 26; ++i) {
        const a = 'a'.charCodeAt(0) + i;
        const b = 'A'.charCodeAt(0) + i;
        if (last[a] && last[a] < first[b]) {
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
        let mut first = [0; 128];
        let mut last = [0; 128];
        for (i, ch) in word.chars().enumerate() {
            let j = ch as u8 as usize;
            let pos = (i + 1) as i32;
            if first[j] == 0 {
                first[j] = pos;
            }
            last[j] = pos;
        }
        let mut ans = 0;
        for i in 0..26 {
            let a = (b'a' + i) as usize;
            let b = (b'A' + i) as usize;
            if last[a] > 0 && last[a] < first[b] {
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
