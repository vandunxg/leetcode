---
comments: true
difficulty: Hard
tags:
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [828. Count Unique Characters of All Substrings of a Given String](https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string)

[中文文档](/solution/0800-0899/0828.Count%20Unique%20Characters%20of%20All%20Substrings%20of%20a%20Given%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy định nghĩa hàm <code>countUniqueChars(s)</code> trả về số ký tự chỉ xuất hiện một lần trong <code>s</code>.</p>

<ul>
	<li>Ví dụ, khi gọi <code>countUniqueChars(s)</code> với <code>s = &quot;LEETCODE&quot;</code>, các ký tự <code>&quot;L&quot;</code>, <code>&quot;T&quot;</code>, <code>&quot;C&quot;</code>, <code>&quot;O&quot;</code>, <code>&quot;D&quot;</code> được xem là ký tự duy nhất vì mỗi ký tự chỉ xuất hiện một lần trong <code>s</code>; do đó <code>countUniqueChars(s) = 5</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code>, hãy trả về tổng <code>countUniqueChars(t)</code> trên mọi chuỗi con <code>t</code> của <code>s</code>. Các test được tạo sao cho đáp án nằm trong phạm vi số nguyên 32-bit.</p>

<p>Lưu ý, một số chuỗi con có thể trùng nhau; trong trường hợp đó, vẫn phải tính từng lần xuất hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ABC&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích: </strong>Tất cả chuỗi con có thể có là: &quot;A&quot;,&quot;B&quot;,&quot;C&quot;,&quot;AB&quot;,&quot;BC&quot; và &quot;ABC&quot;.
Mỗi chuỗi con đều chỉ gồm các chữ cái không trùng nhau.
Tổng độ dài của tất cả chuỗi con là 1 + 1 + 1 + 2 + 2 + 3 = 10
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ABA&quot;
<strong>Đầu ra:</strong> 8
<strong>Giải thích: </strong>Tương tự ví dụ 1, ngoại trừ <code>countUniqueChars</code>(&quot;ABA&quot;) = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;LEETCODE&quot;
<strong>Đầu ra:</strong> 92
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính đóng góp của từng ký tự

<!-- thinking:start -->

> **Tư duy**
>
> Có $O(n^2)$ chuỗi con và $n\le 10^5$, nên không thể liệt kê hết. Một ký tự là duy nhất trong chuỗi con khi và chỉ khi chuỗi con chứa lần xuất hiện này nhưng không chứa lần xuất hiện liền trước hoặc liền sau của cùng ký tự.
>
> Lưu các chỉ số xuất hiện của từng chữ cái, thêm sentinel ở hai đầu. Với lần xuất hiện thứ $i$, ta có thể chọn độc lập điểm bắt đầu trong khoảng trống bên trái và điểm kết thúc trong khoảng trống bên phải; đóng góp của nó là tích độ dài hai khoảng trống. Cộng đóng góp của tất cả chữ cái.

<!-- thinking:end -->

Với mỗi ký tự $c_i$ trong chuỗi $s$, nếu nó chỉ xuất hiện một lần trong một chuỗi con thì nó đóng góp 1 vào số ký tự duy nhất của chuỗi con đó.

Vì vậy, ta chỉ cần tính số chuỗi con mà mỗi ký tự $c_i$ xuất hiện đúng một lần.

Ta dùng hash table hoặc mảng $d$ có độ dài $26$ để lưu các chỉ số xuất hiện của từng ký tự trong $s$ theo thứ tự tăng dần.

Với mỗi ký tự $c_i$, duyệt từng vị trí $p$ trong $d[c_i]$ và tìm vị trí gần nhất $l$ ở bên trái cùng vị trí gần nhất $r$ ở bên phải. Số chuỗi con thỏa mãn, được tạo bằng cách mở rộng từ vị trí $p$ sang hai phía, là $(p - l) \times (r - p)$. Thực hiện phép tính này cho từng ký tự rồi cộng các đóng góp để thu được đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueLetterString(self, s: str) -> int:
        d = defaultdict(list)
        for i, c in enumerate(s):
            d[c].append(i)
        ans = 0
        for v in d.values():
            v = [-1] + v + [len(s)]
            for i in range(1, len(v) - 1):
                ans += (v[i] - v[i - 1]) * (v[i + 1] - v[i])
        return ans
```

#### Java

```java
class Solution {
    public int uniqueLetterString(String s) {
        List<Integer>[] d = new List[26];
        Arrays.setAll(d, k -> new ArrayList<>());
        for (int i = 0; i < 26; ++i) {
            d[i].add(-1);
        }
        for (int i = 0; i < s.length(); ++i) {
            d[s.charAt(i) - 'A'].add(i);
        }
        int ans = 0;
        for (var v : d) {
            v.add(s.length());
            for (int i = 1; i < v.size() - 1; ++i) {
                ans += (v.get(i) - v.get(i - 1)) * (v.get(i + 1) - v.get(i));
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
    int uniqueLetterString(string s) {
        vector<vector<int>> d(26, {-1});
        for (int i = 0; i < s.size(); ++i) {
            d[s[i] - 'A'].push_back(i);
        }
        int ans = 0;
        for (auto& v : d) {
            v.push_back(s.size());
            for (int i = 1; i < v.size() - 1; ++i) {
                ans += (v[i] - v[i - 1]) * (v[i + 1] - v[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func uniqueLetterString(s string) (ans int) {
	d := make([][]int, 26)
	for i := range d {
		d[i] = []int{-1}
	}
	for i, c := range s {
		d[c-'A'] = append(d[c-'A'], i)
	}
	for _, v := range d {
		v = append(v, len(s))
		for i := 1; i < len(v)-1; i++ {
			ans += (v[i] - v[i-1]) * (v[i+1] - v[i])
		}
	}
	return
}
```

#### TypeScript

```ts
function uniqueLetterString(s: string): number {
    const d: number[][] = Array.from({ length: 26 }, () => [-1]);
    for (let i = 0; i < s.length; ++i) {
        d[s.charCodeAt(i) - 'A'.charCodeAt(0)].push(i);
    }
    let ans = 0;
    for (const v of d) {
        v.push(s.length);

        for (let i = 1; i < v.length - 1; ++i) {
            ans += (v[i] - v[i - 1]) * (v[i + 1] - v[i]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn unique_letter_string(s: String) -> i32 {
        let mut d: Vec<Vec<i32>> = vec![vec![-1; 1]; 26];
        for (i, c) in s.chars().enumerate() {
            d[(c as usize) - ('A' as usize)].push(i as i32);
        }
        let mut ans = 0;
        for v in d.iter_mut() {
            v.push(s.len() as i32);
            for i in 1..v.len() - 1 {
                ans += (v[i] - v[i - 1]) * (v[i + 1] - v[i]);
            }
        }
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
