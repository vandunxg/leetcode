---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [01.02. Check Permutation](https://leetcode.cn/problems/check-permutation-lcci)

[中文文档](/lcci/01.02.Check%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi, hãy viết một method để xác định xem một chuỗi có phải là permutation của chuỗi còn lại hay không.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào: </strong>s1 = &quot;abc&quot;, s2 = &quot;bca&quot;

<strong>Đầu ra: </strong>true

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào: </strong>s1 = &quot;abc&quot;, s2 = &quot;bad&quot;

<strong>Đầu ra: </strong>false

</pre>

<p><strong>Lưu ý:</strong></p>
<ol>
	<li><code>0 &lt;= len(s1) &lt;= 100 </code></li>
	<li><code>0 &lt;= len(s2) &lt;= 100</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Array hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hai chuỗi là permutation của nhau khi và chỉ khi chúng có cùng multiset ký tự. Có thể loại ngay các chuỗi có độ dài khác nhau; việc liệt kê các permutation của $s1$ tốn kém hơn rất nhiều so với yêu cầu của kích thước input.
>
> Nút thắt nằm ở việc kiểm tra tần suất trong thời gian tuyến tính. Ta đếm các ký tự trong $s1$, sau đó giảm dần khi duyệt $s2$: nếu một giá trị đếm âm, tần suất của hai chuỗi đã khác nhau.
>
> Các test chỉ sử dụng chữ cái thường, nên một array có độ dài $26$ là đủ để làm bảng đếm. So sánh độ dài trước giúp tránh việc đếm trên những input chắc chắn không thể khớp.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem độ dài của hai chuỗi có bằng nhau hay không. Nếu không bằng nhau, ta trực tiếp trả về `false`.

Sau đó, ta sử dụng một array hoặc hash table để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s1$.

Tiếp theo, ta duyệt chuỗi $s2$ còn lại. Với mỗi ký tự gặp được, ta giảm giá trị đếm tương ứng đi một đơn vị. Nếu giá trị đếm sau khi giảm nhỏ hơn $0$, điều đó có nghĩa là số lần xuất hiện của các ký tự trong hai chuỗi khác nhau, nên ta trực tiếp trả về `false`.

Cuối cùng, sau khi duyệt chuỗi $s2$, ta trả về `true`.

Lưu ý: Trong bài toán này, các chuỗi của mọi test case chỉ chứa chữ cái thường, nên ta có thể trực tiếp tạo một array có độ dài $26$ để đếm.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi và $C$ là kích thước character set. Trong bài toán này, $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def CheckPermutation(self, s1: str, s2: str) -> bool:
        return Counter(s1) == Counter(s2)
```

#### Java

```java
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (char c : s1.toCharArray()) {
            ++cnt[c - 'a'];
        }
        for (char c : s2.toCharArray()) {
            if (--cnt[c - 'a'] < 0) {
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
    bool CheckPermutation(string s1, string s2) {
        if (s1.size() != s2.size()) {
            return false;
        }
        int cnt[26]{};
        for (char c : s1) {
            ++cnt[c - 'a'];
        }
        for (char c : s2) {
            if (--cnt[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func CheckPermutation(s1 string, s2 string) bool {
	if len(s1) != len(s2) {
		return false
	}
	cnt := make([]int, 26)
	for _, c := range s1 {
		cnt[c-'a']++
	}
	for _, c := range s2 {
		if cnt[c-'a']--; cnt[c-'a'] < 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function CheckPermutation(s1: string, s2: string): boolean {
    if (s1.length !== s2.length) {
        return false;
    }
    const cnt: Record<string, number> = {};
    for (const c of s1) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    for (const c of s2) {
        if (!cnt[c]) {
            return false;
        }
        cnt[c]--;
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_permutation(s1: String, s2: String) -> bool {
        if s1.len() != s2.len() {
            return false;
        }

        let mut cnt = vec![0; 26];
        for c in s1.chars() {
            cnt[(c as usize - 'a' as usize)] += 1;
        }

        for c in s2.chars() {
            let index = c as usize - 'a' as usize;
            if cnt[index] == 0 {
                return false;
            }
            cnt[index] -= 1;
        }

        true
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {boolean}
 */
var CheckPermutation = function (s1, s2) {
    if (s1.length !== s2.length) {
        return false;
    }
    const cnt = {};
    for (const c of s1) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    for (const c of s2) {
        if (!cnt[c]) {
            return false;
        }
        cnt[c]--;
    }
    return true;
};
```

#### Swift

```swift
class Solution {
    func CheckPermutation(_ s1: String, _ s2: String) -> Bool {
        if s1.count != s2.count {
            return false
        }

        var cnt = [Int](repeating: 0, count: 26)

        for char in s1 {
            cnt[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
        }

        for char in s2 {
            let index = Int(char.asciiValue! - Character("a").asciiValue!)
            if cnt[index] == 0 {
                return false
            }
            cnt[index] -= 1
        }

        return true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Việc đếm tần suất đã giải quyết bài toán trong thời gian $O(n)$, nhưng sử dụng không gian phụ tỷ lệ với alphabet.
>
> Sắp xếp hai chuỗi rồi so sánh chúng cũng cho kết quả tương đương, không giả định alphabet có kích thước nhỏ, với chi phí $O(n \log n)$ và cách triển khai ngắn hơn.

<!-- thinking:end -->

Ta cũng có thể sắp xếp hai chuỗi theo thứ tự lexicographical, sau đó so sánh xem hai chuỗi có bằng nhau hay không.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def CheckPermutation(self, s1: str, s2: str) -> bool:
        return sorted(s1) == sorted(s2)
```

#### Java

```java
class Solution {
    public boolean CheckPermutation(String s1, String s2) {
        char[] cs1 = s1.toCharArray();
        char[] cs2 = s2.toCharArray();
        Arrays.sort(cs1);
        Arrays.sort(cs2);
        return Arrays.equals(cs1, cs2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool CheckPermutation(string s1, string s2) {
        ranges::sort(s1);
        ranges::sort(s2);
        return s1 == s2;
    }
};
```

#### Go

```go
func CheckPermutation(s1 string, s2 string) bool {
	cs1, cs2 := []byte(s1), []byte(s2)
	sort.Slice(cs1, func(i, j int) bool { return cs1[i] < cs1[j] })
	sort.Slice(cs2, func(i, j int) bool { return cs2[i] < cs2[j] })
	return string(cs1) == string(cs2)
}
```

#### TypeScript

```ts
function CheckPermutation(s1: string, s2: string): boolean {
    return [...s1].sort().join('') === [...s2].sort().join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn check_permutation(s1: String, s2: String) -> bool {
        let mut s1: Vec<char> = s1.chars().collect();
        let mut s2: Vec<char> = s2.chars().collect();
        s1.sort();
        s2.sort();
        s1 == s2
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s1
 * @param {string} s2
 * @return {boolean}
 */
var CheckPermutation = function (s1, s2) {
    return [...s1].sort().join('') === [...s2].sort().join('');
};
```

#### Swift

```swift
class Solution {
    func CheckPermutation(_ s1: String, _ s2: String) -> Bool {
        let s1 = s1.sorted()
        let s2 = s2.sorted()
        return s1 == s2
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
