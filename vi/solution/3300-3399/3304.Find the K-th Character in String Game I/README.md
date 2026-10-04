---
comments: true
difficulty: Easy
rating: 1288
source: Weekly Contest 417 Q1
tags:
    - Bit Manipulation
    - Recursion
    - Math
    - Simulation
---

<!-- problem:start -->

# [3304. Find the K-th Character in String Game I](https://leetcode.com/problems/find-the-k-th-character-in-string-game-i)

[Tài liệu tiếng Trung](/solution/3300-3399/3304.Find%20the%20K-th%20Character%20in%20String%20Game%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi. Ban đầu, Alice có chuỗi <code>word = &quot;a&quot;</code>.</p>

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Bob sẽ yêu cầu Alice thực hiện thao tác sau <strong>vô hạn lần</strong>:</p>

<ul>
	<li>Tạo một chuỗi mới bằng cách <strong>thay đổi</strong> mỗi ký tự trong <code>word</code> thành ký tự <strong>tiếp theo</strong> trong bảng chữ cái tiếng Anh, rồi <strong>nối</strong> chuỗi đó vào <em>chuỗi ban đầu</em> <code>word</code>.</li>
</ul>

<p>Ví dụ, thực hiện thao tác với <code>&quot;c&quot;</code> sẽ tạo ra <code>&quot;cd&quot;</code>, còn thực hiện thao tác với <code>&quot;zb&quot;</code> sẽ tạo ra <code>&quot;zbac&quot;</code>.</p>

<p>Trả về giá trị của ký tự thứ <code>k<sup>th</sup></code> trong <code>word</code>, sau khi thực hiện đủ số thao tác để <code>word</code> có <strong>ít nhất</strong> <code>k</code> ký tự.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;b&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>word = &quot;a&quot;</code>. Ta cần thực hiện thao tác ba lần:</p>

<ul>
	<li>Chuỗi được tạo ra là <code>&quot;b&quot;</code>, <code>word</code> trở thành <code>&quot;ab&quot;</code>.</li>
	<li>Chuỗi được tạo ra là <code>&quot;bc&quot;</code>, <code>word</code> trở thành <code>&quot;abbc&quot;</code>.</li>
	<li>Chuỗi được tạo ra là <code>&quot;bccd&quot;</code>, <code>word</code> trở thành <code>&quot;abbcbccd&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;c&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác nối thêm một bản sao của word hiện tại với mọi chữ cái được dịch sang một vị trí. Vì $k \le 500$, ta có thể mô phỏng cho đến khi độ dài đạt ít nhất $k$.
>
> Lưu các độ lệch trong $0..25$ giúp tránh phải xây dựng chuỗi: nửa được nối thêm là nửa cũ cộng 1, lấy modulo $26$.
>
> Đáp án là chữ cái tại chỉ số $k-1$ sau khi mảng đủ dài.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{word}$ để lưu chuỗi sau mỗi thao tác. Khi độ dài của $\textit{word}$ nhỏ hơn $k$, ta liên tục thực hiện các thao tác trên $\textit{word}$.

Cuối cùng, trả về $\textit{word}[k - 1]$.

Độ phức tạp thời gian là $O(k)$ và độ phức tạp không gian là $O(k)$. Ở đây, $k$ là tham số đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthCharacter(self, k: int) -> str:
        word = [0]
        while len(word) < k:
            word.extend([(x + 1) % 26 for x in word])
        return chr(ord("a") + word[k - 1])
```

#### Java

```java
class Solution {
    public char kthCharacter(int k) {
        List<Integer> word = new ArrayList<>();
        word.add(0);
        while (word.size() < k) {
            for (int i = 0, m = word.size(); i < m; ++i) {
                word.add((word.get(i) + 1) % 26);
            }
        }
        return (char) ('a' + word.get(k - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    char kthCharacter(int k) {
        vector<int> word;
        word.push_back(0);
        while (word.size() < k) {
            int m = word.size();
            for (int i = 0; i < m; ++i) {
                word.push_back((word[i] + 1) % 26);
            }
        }
        return 'a' + word[k - 1];
    }
};
```

#### Go

```go
func kthCharacter(k int) byte {
	word := []int{0}
	for len(word) < k {
		m := len(word)
		for i := 0; i < m; i++ {
			word = append(word, (word[i]+1)%26)
		}
	}
	return 'a' + byte(word[k-1])
}
```

#### TypeScript

```ts
function kthCharacter(k: number): string {
    const word: number[] = [0];
    while (word.length < k) {
        word.push(...word.map(x => (x + 1) % 26));
    }
    return String.fromCharCode(97 + word[k - 1]);
}
```

#### Rust

```rust
impl Solution {
    pub fn kth_character(k: i32) -> char {
        let mut word = vec![0];
        while word.len() < k as usize {
            let m = word.len();
            for i in 0..m {
                word.push((word[i] + 1) % 26);
            }
        }
        (b'a' + word[(k - 1) as usize] as u8) as char
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
