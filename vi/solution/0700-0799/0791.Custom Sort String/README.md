---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [791. Custom Sort String](https://leetcode.com/problems/custom-sort-string)

[中文文档](/solution/0700-0799/0791.Custom%20Sort%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>order</code> và <code>s</code>. Các ký tự trong <code>order</code> đều <strong>khác nhau</strong> và đã được sắp xếp theo một thứ tự tùy chỉnh.</p>

<p>Hãy hoán vị các ký tự của <code>s</code> sao cho thứ tự của chúng tuân theo <code>order</code>. Cụ thể, nếu ký tự <code>x</code> đứng trước ký tự <code>y</code> trong <code>order</code>, thì <code>x</code> cũng phải đứng trước <code>y</code> trong chuỗi sau khi hoán vị.</p>

<p>Trả về <em>một hoán vị bất kỳ của </em><code>s</code><em> thỏa mãn điều kiện này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> order = &quot;cba&quot;, s = &quot;abcd&quot; </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;cbad&quot; </span></p>

<p><strong>Giải thích: </strong> <code>&quot;a&quot;</code>, <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code> xuất hiện trong <code>order</code>, nên thứ tự của <code>&quot;a&quot;</code>, <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code> phải là <code>&quot;c&quot;</code>, <code>&quot;b&quot;</code>, rồi đến <code>&quot;a&quot;</code>.</p>

<p>Vì <code>&quot;d&quot;</code> không xuất hiện trong <code>order</code>, nó có thể nằm ở bất kỳ vị trí nào trong chuỗi trả về. <code>&quot;dcba&quot;</code>, <code>&quot;cdba&quot;</code>, <code>&quot;cbda&quot;</code> cũng là các kết quả hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> order = &quot;bcafg&quot;, s = &quot;abcd&quot; </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> &quot;bcad&quot; </span></p>

<p><strong>Giải thích: </strong> Các ký tự <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code> và <code>&quot;a&quot;</code> trong <code>order</code> xác định thứ tự tương ứng trong <code>s</code>. Ký tự <code>&quot;d&quot;</code> trong <code>s</code> không có trong <code>order</code>, nên có thể đặt ở vị trí bất kỳ.</p>

<p>Theo thứ tự xuất hiện trong <code>order</code>, các ký tự <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code> và <code>&quot;a&quot;</code> trong <code>s</code> cần được sắp xếp thành <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code>, <code>&quot;a&quot;</code>. Có thể đặt <code>&quot;d&quot;</code> ở bất kỳ vị trí nào vì nó không có trong <code>order</code>. Kết quả <code>&quot;bcad&quot;</code> tuân theo quy tắc này. Các cách sắp xếp khác như <code>&quot;dbca&quot;</code> hoặc <code>&quot;bcda&quot;</code> cũng hợp lệ, miễn là <code>&quot;b&quot;</code>, <code>&quot;c&quot;</code>, <code>&quot;a&quot;</code> vẫn giữ nguyên thứ tự.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= order.length &lt;= 26</code></li>
	<li><code>1 &lt;= s.length &lt;= 200</code></li>
	<li><code>order</code> và <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Các ký tự trong <code>order</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại $s$ theo thứ tự tương đối trong $order$. Chỉ cần sort theo chỉ số của ký tự trong $order$.
>
> Các ký tự không có trong $order$ được gán khóa $0$ và dồn về đầu chuỗi; cách này vẫn hợp lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def customSortString(self, order: str, s: str) -> str:
        d = {c: i for i, c in enumerate(order)}
        return ''.join(sorted(s, key=lambda x: d.get(x, 0)))
```

#### Java

```java
class Solution {
    public String customSortString(String order, String s) {
        int[] d = new int[26];
        for (int i = 0; i < order.length(); ++i) {
            d[order.charAt(i) - 'a'] = i;
        }
        List<Character> cs = new ArrayList<>();
        for (int i = 0; i < s.length(); ++i) {
            cs.add(s.charAt(i));
        }
        cs.sort((a, b) -> d[a - 'a'] - d[b - 'a']);
        return cs.stream().map(String::valueOf).collect(Collectors.joining());
    }
}
```

#### C++

```cpp
class Solution {
public:
    string customSortString(string order, string s) {
        int d[26] = {0};
        for (int i = 0; i < order.size(); ++i) d[order[i] - 'a'] = i;
        sort(s.begin(), s.end(), [&](auto a, auto b) { return d[a - 'a'] < d[b - 'a']; });
        return s;
    }
};
```

#### Go

```go
func customSortString(order string, s string) string {
	d := [26]int{}
	for i := range order {
		d[order[i]-'a'] = i
	}
	cs := []byte(s)
	sort.Slice(cs, func(i, j int) bool { return d[cs[i]-'a'] < d[cs[j]-'a'] })
	return string(cs)
}
```

#### TypeScript

```ts
function customSortString(order: string, s: string): string {
    const toIndex = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const n = order.length;
    const d = new Array(26).fill(n);
    for (let i = 0; i < n; i++) {
        d[toIndex(order[i])] = i;
    }
    return [...s].sort((a, b) => d[toIndex(a)] - d[toIndex(b)]).join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn custom_sort_string(order: String, s: String) -> String {
        let n = order.len();
        let mut d = [n; 26];
        for (i, c) in order.as_bytes().iter().enumerate() {
            d[(c - b'a') as usize] = i;
        }
        let mut ans = s.chars().collect::<Vec<_>>();
        ans.sort_by(|&a, &b| {
            d[((a as u8) - ('a' as u8)) as usize].cmp(&d[((b as u8) - ('a' as u8)) as usize])
        });
        ans.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Sort có độ phức tạp $n\log n$. Thay vào đó, đếm ký tự trong $s$, lần lượt thêm mỗi ký tự theo thứ tự trong $order$ với số lần tương ứng, rồi nối các ký tự còn lại. Cách này có độ phức tạp tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def customSortString(self, order: str, s: str) -> str:
        cnt = Counter(s)
        ans = []
        for c in order:
            ans.append(c * cnt[c])
            cnt[c] = 0
        for c, v in cnt.items():
            ans.append(c * v)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String customSortString(String order, String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < order.length(); ++i) {
            char c = order.charAt(i);
            while (cnt[c - 'a']-- > 0) {
                ans.append(c);
            }
        }
        for (int i = 0; i < 26; ++i) {
            while (cnt[i]-- > 0) {
                ans.append((char) ('a' + i));
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string customSortString(string order, string s) {
        int cnt[26] = {0};
        for (char& c : s) ++cnt[c - 'a'];
        string ans;
        for (char& c : order)
            while (cnt[c - 'a']-- > 0) ans += c;
        for (int i = 0; i < 26; ++i)
            if (cnt[i] > 0) ans += string(cnt[i], i + 'a');
        return ans;
    }
};
```

#### Go

```go
func customSortString(order string, s string) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	ans := []rune{}
	for _, c := range order {
		for cnt[c-'a'] > 0 {
			ans = append(ans, c)
			cnt[c-'a']--
		}
	}
	for i, v := range cnt {
		for j := 0; j < v; j++ {
			ans = append(ans, rune('a'+i))
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function customSortString(order: string, s: string): string {
    const toIndex = (c: string) => c.charCodeAt(0) - 'a'.charCodeAt(0);
    const count = new Array(26).fill(0);
    for (const c of s) {
        count[toIndex(c)]++;
    }
    const ans: string[] = [];
    for (const c of order) {
        const i = toIndex(c);
        ans.push(c.repeat(count[i]));
        count[i] = 0;
    }
    for (let i = 0; i < 26; i++) {
        if (!count[i]) continue;
        ans.push(String.fromCharCode('a'.charCodeAt(0) + i).repeat(count[i]));
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn custom_sort_string(order: String, s: String) -> String {
        let mut count = [0; 26];
        for c in s.as_bytes() {
            count[(c - b'a') as usize] += 1;
        }
        let mut ans = String::new();
        for c in order.as_bytes() {
            for _ in 0..count[(c - b'a') as usize] {
                ans.push(char::from(*c));
            }
            count[(c - b'a') as usize] = 0;
        }
        for i in 0..count.len() {
            for _ in 0..count[i] {
                ans.push(char::from(b'a' + (i as u8)));
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
