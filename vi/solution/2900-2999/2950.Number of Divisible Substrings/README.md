---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [2950. Number of Divisible Substrings 🔒](https://leetcode.com/problems/number-of-divisible-substrings)

[中文文档](/solution/2900-2999/2950.Number%20of%20Divisible%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Mỗi ký tự trong bảng chữ cái tiếng Anh được ánh xạ tới một chữ số như hình dưới đây.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2950.Number%20of%20Divisible%20Substrings/images/old_phone_digits.png" style="padding: 10px; width: 200px; height: 200px;" /></p>

<p>Một chuỗi được gọi là <strong>chia hết</strong> nếu tổng các giá trị ánh xạ của các ký tự trong chuỗi chia hết cho độ dài của chuỗi.</p>

<p>Cho một chuỗi <code>s</code>, hãy trả về <em>số lượng <strong>chuỗi con chia hết</strong></em> của <code>s</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp, không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<table border="1" cellspacing="3" style="border-collapse: separate; text-align: center;">
	<tbody>
		<tr>
			<th style="padding: 5px; border: 1px solid black;">Chuỗi con</th>
			<th style="padding: 5px; border: 1px solid black;">Giá trị ánh xạ</th>
			<th style="padding: 5px; border: 1px solid black;">Tổng</th>
			<th style="padding: 5px; border: 1px solid black;">Độ dài</th>
			<th style="padding: 5px; border: 1px solid black;">Chia hết?</th>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">a</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">s</td>
			<td style="padding: 5px; border: 1px solid black;">7</td>
			<td style="padding: 5px; border: 1px solid black;">7</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">d</td>
			<td style="padding: 5px; border: 1px solid black;">2</td>
			<td style="padding: 5px; border: 1px solid black;">2</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">f</td>
			<td style="padding: 5px; border: 1px solid black;">3</td>
			<td style="padding: 5px; border: 1px solid black;">3</td>
			<td style="padding: 5px; border: 1px solid black;">1</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">as</td>
			<td style="padding: 5px; border: 1px solid black;">1, 7</td>
			<td style="padding: 5px; border: 1px solid black;">8</td>
			<td style="padding: 5px; border: 1px solid black;">2</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">sd</td>
			<td style="padding: 5px; border: 1px solid black;">7, 2</td>
			<td style="padding: 5px; border: 1px solid black;">9</td>
			<td style="padding: 5px; border: 1px solid black;">2</td>
			<td style="padding: 5px; border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">df</td>
			<td style="padding: 5px; border: 1px solid black;">2, 3</td>
			<td style="padding: 5px; border: 1px solid black;">5</td>
			<td style="padding: 5px; border: 1px solid black;">2</td>
			<td style="padding: 5px; border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">asd</td>
			<td style="padding: 5px; border: 1px solid black;">1, 7, 2</td>
			<td style="padding: 5px; border: 1px solid black;">10</td>
			<td style="padding: 5px; border: 1px solid black;">3</td>
			<td style="padding: 5px; border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">sdf</td>
			<td style="padding: 5px; border: 1px solid black;">7, 2, 3</td>
			<td style="padding: 5px; border: 1px solid black;">12</td>
			<td style="padding: 5px; border: 1px solid black;">3</td>
			<td style="padding: 5px; border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="padding: 5px; border: 1px solid black;">asdf</td>
			<td style="padding: 5px; border: 1px solid black;">1, 7, 2, 3</td>
			<td style="padding: 5px; border: 1px solid black;">13</td>
			<td style="padding: 5px; border: 1px solid black;">4</td>
			<td style="padding: 5px; border: 1px solid black;">Không</td>
		</tr>
	</tbody>
</table>

<pre>
<strong>Đầu vào:</strong> word = &quot;asdf&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Bảng trên liệt kê thông tin chi tiết của mọi chuỗi con của word, và ta thấy có 6 chuỗi con chia hết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;bdh&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 4 chuỗi con chia hết là: &quot;b&quot;, &quot;d&quot;, &quot;h&quot;, &quot;bdh&quot;.
Có thể chứng minh rằng không còn chuỗi con nào khác của word chia hết.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abcd&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 6 chuỗi con chia hết là: &quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;d&quot;, &quot;ab&quot;, &quot;cd&quot;.
Có thể chứng minh rằng không còn chuỗi con nào khác của word chia hết.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 2000</code></li>
	<li><code>word</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái được ánh xạ tới các giá trị từ $1 \ldots 9$; ta đếm những chuỗi con có tổng giá trị ánh xạ chia hết cho độ dài. Với $n$ vừa phải, ta duyệt các vị trí kết thúc, cộng dồn giá trị ánh xạ và kiểm tra điều kiện $s \bmod (j-i+1)=0$.
>
> Xây dựng bảng ánh xạ một lần để vòng lặp bên trong không phải tính lại.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table hoặc mảng $mp$ để lưu giá trị tương ứng với mỗi chữ cái.

Sau đó, ta duyệt vị trí bắt đầu $i$ của chuỗi con, rồi duyệt vị trí kết thúc $j$ của chuỗi con và tính tổng số $s$ của chuỗi con $s[i..j]$. Nếu $s$ chia hết cho $j-i+1$, ta tìm được một chuỗi con chia hết và tăng đáp án lên một.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(C)$. Trong đó $n$ là độ dài của chuỗi $word$, còn $C$ là kích thước của tập ký tự; trong bài này $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDivisibleSubstrings(self, word: str) -> int:
        d = ["ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"]
        mp = {}
        for i, s in enumerate(d, 1):
            for c in s:
                mp[c] = i
        ans = 0
        n = len(word)
        for i in range(n):
            s = 0
            for j in range(i, n):
                s += mp[word[j]]
                ans += s % (j - i + 1) == 0
        return ans
```

#### Java

```java
class Solution {
    public int countDivisibleSubstrings(String word) {
        String[] d = {"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"};
        int[] mp = new int[26];
        for (int i = 0; i < d.length; ++i) {
            for (char c : d[i].toCharArray()) {
                mp[c - 'a'] = i + 1;
            }
        }
        int ans = 0;
        int n = word.length();
        for (int i = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += mp[word.charAt(j) - 'a'];
                ans += s % (j - i + 1) == 0 ? 1 : 0;
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
    int countDivisibleSubstrings(string word) {
        string d[9] = {"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"};
        int mp[26]{};
        for (int i = 0; i < 9; ++i) {
            for (char& c : d[i]) {
                mp[c - 'a'] = i + 1;
            }
        }
        int ans = 0;
        int n = word.size();
        for (int i = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += mp[word[j] - 'a'];
                ans += s % (j - i + 1) == 0 ? 1 : 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countDivisibleSubstrings(word string) (ans int) {
	d := []string{"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"}
	mp := [26]int{}
	for i, s := range d {
		for _, c := range s {
			mp[c-'a'] = i + 1
		}
	}
	n := len(word)
	for i := 0; i < n; i++ {
		s := 0
		for j := i; j < n; j++ {
			s += mp[word[j]-'a']
			if s%(j-i+1) == 0 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countDivisibleSubstrings(word: string): number {
    const d: string[] = ['ab', 'cde', 'fgh', 'ijk', 'lmn', 'opq', 'rst', 'uvw', 'xyz'];
    const mp: number[] = Array(26).fill(0);
    for (let i = 0; i < d.length; ++i) {
        for (const c of d[i]) {
            mp[c.charCodeAt(0) - 'a'.charCodeAt(0)] = i + 1;
        }
    }
    const n = word.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let s = 0;
        for (let j = i; j < n; ++j) {
            s += mp[word.charCodeAt(j) - 'a'.charCodeAt(0)];
            if (s % (j - i + 1) === 0) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_divisible_substrings(word: String) -> i32 {
        let d = vec!["ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"];
        let mut mp = vec![0; 26];

        for (i, s) in d.iter().enumerate() {
            s.chars().for_each(|c| {
                mp[(c as usize) - ('a' as usize)] = (i + 1) as i32;
            });
        }

        let mut ans = 0;
        let n = word.len();

        for i in 0..n {
            let mut s = 0;

            for j in i..n {
                s += mp[(word.as_bytes()[j] as usize) - ('a' as usize)];
                ans += (s % ((j - i + 1) as i32) == 0) as i32;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm + Tổng tiền tố + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp $O(n^2)$ với các chuỗi dài. Trung bình của một đoạn hợp lệ là một số nguyên trong khoảng $1 \ldots 9$, vì vậy ta duyệt giá trị trung bình $t$, trừ $t$ khỏi mọi giá trị ánh xạ và đếm các mảng con có tổng bằng $0$.
>
> Tổng tiền tố kết hợp với hash map lưu tần suất thực hiện việc này trong một lượt duyệt cho mỗi $t$, tức là $O(9n)$.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta dùng một hash table hoặc mảng $mp$ để lưu giá trị tương ứng với mỗi chữ cái.

Nếu tổng các số trong một mảng con số nguyên chia hết cho độ dài của nó, thì giá trị trung bình của mảng con này phải là một số nguyên. Vì giá trị của mỗi phần tử trong mảng con nằm trong khoảng $[1, 9]$, giá trị trung bình của mảng con chỉ có thể là một trong các số $1, 2, \cdots, 9$.

Ta có thể duyệt giá trị trung bình $i$ của mảng con. Nếu tổng các phần tử trong một mảng con chia hết cho $i$, giả sử mảng con là $a_1, a_2, \cdots, a_k$, thì $a_1 + a_2 + \cdots + a_k = i \times k$, hay $(a_1 - i) + (a_2 - i) + \cdots + (a_k - i) = 0$. Nếu xem $a_k - i$ là một phần tử mới $b_k$, mảng con ban đầu trở thành $b_1, b_2, \cdots, b_k$, trong đó $b_1 + b_2 + \cdots + b_k = 0$. Ta chỉ cần tìm số lượng mảng con trong mảng mới có tổng các phần tử bằng $0$, có thể thực hiện bằng cách kết hợp "hash table" với "tổng tiền tố".

Độ phức tạp thời gian là $O(10 \times n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $word$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDivisibleSubstrings(self, word: str) -> int:
        d = ["ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"]
        mp = {}
        for i, s in enumerate(d, 1):
            for c in s:
                mp[c] = i
        ans = 0
        for i in range(1, 10):
            cnt = defaultdict(int)
            cnt[0] = 1
            s = 0
            for c in word:
                s += mp[c] - i
                ans += cnt[s]
                cnt[s] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countDivisibleSubstrings(String word) {
        String[] d = {"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"};
        int[] mp = new int[26];
        for (int i = 0; i < d.length; ++i) {
            for (char c : d[i].toCharArray()) {
                mp[c - 'a'] = i + 1;
            }
        }
        int ans = 0;
        char[] cs = word.toCharArray();
        for (int i = 1; i < 10; ++i) {
            Map<Integer, Integer> cnt = new HashMap<>();
            cnt.put(0, 1);
            int s = 0;
            for (char c : cs) {
                s += mp[c - 'a'] - i;
                ans += cnt.getOrDefault(s, 0);
                cnt.merge(s, 1, Integer::sum);
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
    int countDivisibleSubstrings(string word) {
        string d[9] = {"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"};
        int mp[26]{};
        for (int i = 0; i < 9; ++i) {
            for (char& c : d[i]) {
                mp[c - 'a'] = i + 1;
            }
        }
        int ans = 0;
        for (int i = 1; i < 10; ++i) {
            unordered_map<int, int> cnt{{0, 1}};
            int s = 0;
            for (char& c : word) {
                s += mp[c - 'a'] - i;
                ans += cnt[s]++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countDivisibleSubstrings(word string) (ans int) {
	d := []string{"ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"}
	mp := [26]int{}
	for i, s := range d {
		for _, c := range s {
			mp[c-'a'] = i + 1
		}
	}
	for i := 0; i < 10; i++ {
		cnt := map[int]int{0: 1}
		s := 0
		for _, c := range word {
			s += mp[c-'a'] - i
			ans += cnt[s]
			cnt[s]++
		}
	}
	return
}
```

#### TypeScript

```ts
function countDivisibleSubstrings(word: string): number {
    const d = ['ab', 'cde', 'fgh', 'ijk', 'lmn', 'opq', 'rst', 'uvw', 'xyz'];
    const mp: number[] = Array(26).fill(0);

    d.forEach((s, i) => {
        for (const c of s) {
            mp[c.charCodeAt(0) - 'a'.charCodeAt(0)] = i + 1;
        }
    });

    let ans = 0;
    for (let i = 0; i < 10; i++) {
        const cnt: { [key: number]: number } = { 0: 1 };
        let s = 0;
        for (const c of word) {
            s += mp[c.charCodeAt(0) - 'a'.charCodeAt(0)] - i;
            ans += cnt[s] || 0;
            cnt[s] = (cnt[s] || 0) + 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_divisible_substrings(word: String) -> i32 {
        let d = vec!["ab", "cde", "fgh", "ijk", "lmn", "opq", "rst", "uvw", "xyz"];
        let mut mp: Vec<usize> = vec![0; 26];
        for (i, s) in d.iter().enumerate() {
            for c in s.chars() {
                mp[(c as usize) - ('a' as usize)] = i + 1;
            }
        }
        let mut ans = 0;
        for i in 0..10 {
            let mut cnt: HashMap<i32, i32> = HashMap::new();
            cnt.insert(0, 1);
            let mut s = 0;
            for c in word.chars() {
                s += (mp[(c as usize) - ('a' as usize)] - i) as i32;
                ans += cnt.get(&s).cloned().unwrap_or(0);
                *cnt.entry(s).or_insert(0) += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
