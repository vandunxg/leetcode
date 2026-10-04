---
comments: true
difficulty: Easy
rating: 1285
source: Biweekly Contest 112 Q1
tags:
    - String
---

<!-- problem:start -->

# [2839. Check if Strings Can be Made Equal With Operations I](https://leetcode.com/problems/check-if-strings-can-be-made-equal-with-operations-i)

[中文文档](/solution/2800-2899/2839.Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, cả hai đều có độ dài <code>4</code> và chỉ gồm các chữ cái tiếng Anh <strong>viết thường</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau trên một trong hai chuỗi <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn hai chỉ số <code>i</code> và <code>j</code> sao cho <code>j - i = 2</code>, sau đó <strong>hoán đổi</strong> hai ký tự tại các chỉ số đó trong chuỗi.</li>
</ul>

<p>Trả về <code>true</code><em> nếu có thể biến hai chuỗi </em><code>s1</code><em> và </em><code>s2</code><em> thành hai chuỗi bằng nhau, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abcd&quot;, s2 = &quot;cdab&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau trên s1:
- Chọn các chỉ số i = 0, j = 2. Chuỗi thu được là s1 = &quot;cbad&quot;.
- Chọn các chỉ số i = 1, j = 3. Chuỗi thu được là s1 = &quot;cdab&quot; = s2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abcd&quot;, s2 = &quot;dacb&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể biến hai chuỗi thành hai chuỗi bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s1.length == s2.length == 4</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Với chuỗi có độ dài $4$, các phép hoán đổi chỉ trộn các chỉ số cùng tính chẵn lẻ, nên các vị trí chẵn và lẻ là hai nhóm độc lập. Số lần xuất hiện của các ký tự trong cả hai nhóm phải bằng nhau, và điều kiện này cũng đủ.

<!-- thinking:end -->

Ta nhận thấy trong thao tác của đề bài, vì hai chỉ số $i$ và $j$ của chuỗi có cùng tính chẵn lẻ nên có thể thay đổi thứ tự của chúng bằng cách hoán đổi.

Do đó, ta có thể đếm số lần xuất hiện của các ký tự tại các chỉ số lẻ và chẵn trong hai chuỗi. Nếu kết quả đếm của hai chuỗi giống nhau, ta có thể biến chúng thành hai chuỗi bằng nhau bằng các thao tác trên.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi và $\Sigma$ là tập ký tự.

Bài tương tự:

- [2840. Check if Strings Can be Made Equal With Operations II](https://github.com/doocs/leetcode/blob/main/solution/2800-2899/2840.Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeEqual(self, s1: str, s2: str) -> bool:
        return Counter(s1[::2]) == Counter(s2[::2]) and Counter(s1[1::2]) == Counter(
            s2[1::2]
        )
```

#### Java

```java
class Solution {
    public boolean canBeEqual(String s1, String s2) {
        int[][] cnt = new int[2][26];
        for (int i = 0; i < s1.length(); ++i) {
            ++cnt[i & 1][s1.charAt(i) - 'a'];
            --cnt[i & 1][s2.charAt(i) - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[0][i] != 0 || cnt[1][i] != 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canBeEqual(string s1, string s2) {
        vector<vector<int>> cnt(2, vector<int>(26, 0));
        for (int i = 0; i < s1.size(); ++i) {
            ++cnt[i & 1][s1[i] - 'a'];
            --cnt[i & 1][s2[i] - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[0][i] || cnt[1][i]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canBeEqual(s1 string, s2 string) bool {
	cnt := [2][26]int{}
	for i := 0; i < len(s1); i++ {
		cnt[i&1][s1[i]-'a']++
		cnt[i&1][s2[i]-'a']--
	}
	for i := 0; i < 26; i++ {
		if cnt[0][i] != 0 || cnt[1][i] != 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canBeEqual(s1: string, s2: string): boolean {
    const cnt: number[][] = Array.from({ length: 2 }, () => Array.from({ length: 26 }, () => 0));
    for (let i = 0; i < s1.length; ++i) {
        ++cnt[i & 1][s1.charCodeAt(i) - 97];
        --cnt[i & 1][s2.charCodeAt(i) - 97];
    }
    return cnt.every(arr => arr.every(x => x === 0));
}
```

#### Rust

```rust
impl Solution {
    pub fn can_be_equal(s1: String, s2: String) -> bool {
        let mut cnt: [[i32; 26]; 2] = [[0; 26]; 2];
        let n = s1.len();
        let s1 = s1.as_bytes();
        let s2 = s2.as_bytes();

        for i in 0..n {
            let idx = (i & 1) as usize;
            cnt[idx][(s1[i] - b'a') as usize] += 1;
            cnt[idx][(s2[i] - b'a') as usize] -= 1;
        }

        cnt.iter().all(|row| row.iter().all(|&x| x == 0))
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
