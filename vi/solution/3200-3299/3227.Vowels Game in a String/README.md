---
comments: true
difficulty: Medium
rating: 1451
source: Weekly Contest 407 Q2
tags:
    - Brainteaser
    - Math
    - String
    - Game Theory
---

<!-- problem:start -->

# [3227. Vowels Game in a String](https://leetcode.com/problems/vowels-game-in-a-string)

[中文文档](/solution/3200-3299/3227.Vowels%20Game%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi trên một chuỗi.</p>

<p>Cho một chuỗi <code>s</code>, Alice và Bob lần lượt thực hiện trò chơi sau, trong đó Alice đi <strong>trước</strong>:</p>

<ul>
    <li>Trong lượt của Alice, cô ấy phải xóa một <strong><span data-keyword="substring">chuỗi con</span> không rỗng</strong> bất kỳ khỏi <code>s</code>, chứa số lượng nguyên âm <strong>lẻ</strong>.</li>
    <li>Trong lượt của Bob, anh ấy phải xóa một <strong><span data-keyword="substring">chuỗi con</span> không rỗng</strong> bất kỳ khỏi <code>s</code>, chứa số lượng nguyên âm <strong>chẵn</strong>.</li>
</ul>

<p>Người chơi đầu tiên không thể thực hiện nước đi trong lượt của mình sẽ thua. Giả sử cả Alice và Bob đều chơi <strong>tối ưu</strong>.</p>

<p>Trả về <code>true</code> nếu Alice thắng trò chơi, ngược lại trả về <code>false</code>.</p>

<p>Các nguyên âm trong tiếng Anh là: <code>a</code>, <code>e</code>, <code>i</code>, <code>o</code> và <code>u</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcoder&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong><br />
Alice có thể thắng trò chơi như sau:</p>

<ul>
    <li>Alice đi trước, cô ấy có thể xóa chuỗi con được gạch chân trong <code>s = &quot;<u><strong>leetco</strong></u>der&quot;</code>, chứa 3 nguyên âm. Chuỗi kết quả là <code>s = &quot;der&quot;</code>.</li>
    <li>Bob đi thứ hai, anh ấy có thể xóa chuỗi con được gạch chân trong <code>s = &quot;<u><strong>d</strong></u>er&quot;</code>, chứa 0 nguyên âm. Chuỗi kết quả là <code>s = &quot;er&quot;</code>.</li>
    <li>Alice đi thứ ba, cô ấy có thể xóa toàn bộ chuỗi <code>s = &quot;<strong><u>er</u></strong>&quot;</code>, chứa 1 nguyên âm.</li>
    <li>Bob đi thứ tư. Vì chuỗi đã rỗng nên Bob không có nước đi hợp lệ nào. Do đó Alice thắng trò chơi.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;bbcd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong><br />
Alice không có nước đi hợp lệ nào trong lượt đầu tiên, nên Alice thua trò chơi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mẹo tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi xóa một chuỗi con có số lượng nguyên âm lẻ; người không thể đi sẽ thua. Vì $n\le 10^5$, không thể duyệt cây trò chơi, nhưng chỉ cần biết số lượng nguyên âm $k$ là đã quyết định được người thắng.
>
> Nếu $k=0$, người chơi đầu tiên không thể đi; nếu $k$ lẻ, cô ấy xóa toàn bộ chuỗi; nếu $k$ chẵn, cô ấy xóa $k-1$ nguyên âm và để lại một nguyên âm, khiến người chơi thứ hai không thể đi. Vì vậy, chỉ cần chuỗi có nguyên âm thì Alice thắng; chỉ cần duyệt chuỗi một lần.

<!-- thinking:end -->

Gọi số lượng nguyên âm trong chuỗi là $k$.

Nếu $k = 0$, nghĩa là chuỗi không có nguyên âm, Little Red không thể xóa chuỗi con nào và Little Ming trực tiếp thắng.

Nếu $k$ là số lẻ, Little Red có thể xóa toàn bộ chuỗi, qua đó trực tiếp thắng.

Nếu $k$ là số chẵn, Little Red có thể xóa $k - 1$ nguyên âm, để lại một nguyên âm trong chuỗi. Khi đó, Little Ming không thể xóa chuỗi con nào, nên Little Red trực tiếp thắng.

Tóm lại, nếu chuỗi chứa nguyên âm thì Little Red thắng; nếu không thì Little Ming thắng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def doesAliceWin(self, s: str) -> bool:
        vowels = set("aeiou")
        return any(c in vowels for c in s)
```

#### Java

```java
class Solution {
    public boolean doesAliceWin(String s) {
        for (int i = 0; i < s.length(); ++i) {
            if ("aeiou".indexOf(s.charAt(i)) != -1) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool doesAliceWin(string s) {
        string vowels = "aeiou";
        for (char c : s) {
            if (vowels.find(c) != string::npos) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func doesAliceWin(s string) bool {
    vowels := "aeiou"
    for _, c := range s {
        if strings.ContainsRune(vowels, c) {
            return true
        }
    }
    return false
}
```

#### TypeScript

```ts
function doesAliceWin(s: string): boolean {
    const vowels = 'aeiou';
    for (const c of s) {
        if (vowels.includes(c)) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn does_alice_win(s: String) -> bool {
        let vowels = "aeiou";
        for c in s.chars() {
            if vowels.contains(c) {
                return true;
            }
        }
        false
    }
}
```

#### C#

```cs
public class Solution {
    public bool DoesAliceWin(string s) {
        string vowels = "aeiou";
        foreach (char c in s) {
            if (vowels.Contains(c)) {
                return true;
            }
        }
        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
