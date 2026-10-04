---
comments: true
difficulty: Medium
rating: 1486
source: Biweekly Contest 112 Q2
tags:
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2840. Check if Strings Can be Made Equal With Operations II](https://leetcode.com/problems/check-if-strings-can-be-made-equal-with-operations-ii)

[中文文档](/solution/2800-2899/2840.Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, cả hai đều có độ dài <code>n</code> và chỉ gồm các chữ cái tiếng Anh <strong>viết thường</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau trên <strong>bất kỳ</strong> chuỗi nào trong hai chuỗi <strong>bao nhiêu lần tùy ý</strong>:</p>

<ul>
	<li>Chọn hai chỉ số <code>i</code> và <code>j</code> sao cho <code>i &lt; j</code> và hiệu <code>j - i</code> là <strong>số chẵn</strong>, sau đó <strong>hoán đổi</strong> hai ký tự tại các chỉ số đó trong chuỗi.</li>
</ul>

<p>Trả về <code>true</code><em> nếu có thể biến hai chuỗi </em><code>s1</code><em> và </em><code>s2</code><em> thành hai chuỗi bằng nhau, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abcdba&quot;, s2 = &quot;cabdab&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau trên s1:
- Chọn các chỉ số i = 0, j = 2. Chuỗi thu được là s1 = &quot;cbadba&quot;.
- Chọn các chỉ số i = 2, j = 4. Chuỗi thu được là s1 = &quot;cbbdaa&quot;.
- Chọn các chỉ số i = 1, j = 5. Chuỗi thu được là s1 = &quot;cabdab&quot; = s2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abe&quot;, s2 = &quot;bea&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể biến hai chuỗi thành hai chuỗi bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s1.length == s2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các phép hoán đổi vẫn chỉ trộn các chỉ số cùng tính chẵn lẻ, chỉ khác là chuỗi dài hơn. Các vị trí chẵn và lẻ vẫn là hai multiset độc lập; chỉ cần số lần xuất hiện tương ứng khớp nhau.

<!-- thinking:end -->

Ta nhận thấy trong phép toán của đề bài, nếu hai chỉ số $i$ và $j$ của chuỗi có cùng tính chẵn lẻ thì có thể thay đổi thứ tự của chúng bằng cách hoán đổi.

Do đó, ta có thể đếm số lần xuất hiện của các ký tự tại các chỉ số lẻ và chẵn trong hai chuỗi. Nếu kết quả đếm của hai chuỗi giống nhau, ta có thể biến chúng thành hai chuỗi bằng nhau bằng các thao tác trên.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi và $\Sigma$ là tập ký tự.

Bài tương tự:

- [2839. Check if Strings Can be Made Equal With Operations I](https://github.com/doocs/leetcode/blob/main/solution/2800-2899/2839.Check%20if%20Strings%20Can%20be%20Made%20Equal%20With%20Operations%20I/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkStrings(self, s1: str, s2: str) -> bool:
        return Counter(s1[::2]) == Counter(s2[::2]) and Counter(s1[1::2]) == Counter(
            s2[1::2]
        )
```

#### Java

```java
class Solution {
    public boolean checkStrings(String s1, String s2) {
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
    bool checkStrings(string s1, string s2) {
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
func checkStrings(s1 string, s2 string) bool {
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
function checkStrings(s1: string, s2: string): boolean {
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
    pub fn check_strings(s1: String, s2: String) -> bool {
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
