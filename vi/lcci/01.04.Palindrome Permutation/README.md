---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [01.04. Palindrome Permutation](https://leetcode.cn/problems/palindrome-permutation-lcci)

[中文文档](/lcci/01.04.Palindrome%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi, hãy viết hàm kiểm tra xem chuỗi đó có phải là một hoán vị của palindrome hay không. Palindrome là một từ hoặc cụm từ đọc xuôi và đọc ngược giống nhau. Hoán vị là cách sắp xếp lại các ký tự. Palindrome không nhất thiết phải chỉ gồm các từ có trong từ điển.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: &quot;</strong>tactcoa&quot;

<strong>Đầu ra: </strong>true（permutations: &quot;tacocat&quot;、&quot;atcocta&quot;, etc.）

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị palindrome dựa trên tính đối xứng sau khi sắp xếp lại, chứ không cần dựng palindrome đó. Không cần liệt kê các hoán vị.
>
> Có nhiều nhất một ký tự có số lần xuất hiện lẻ. Chỉ cần đếm tần suất và xem có bao nhiêu tần suất lẻ.
>
> Một hash table (`Counter`) cho biết toàn bộ tần suất chỉ sau một lượt quét; sau đó kiểm tra số tần suất lẻ có nhỏ hơn $2$ hay không, tương ứng với `sum(v & 1 ...) < 2`.

<!-- thinking:end -->

Dùng hash table $cnt$ để lưu số lần xuất hiện của mỗi ký tự. Nếu có nhiều hơn $1$ ký tự có số lần xuất hiện lẻ thì không thể tạo thành một hoán vị palindrome.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        cnt = Counter(s)
        return sum(v & 1 for v in cnt.values()) < 2
```

#### Java

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Map<Character, Integer> cnt = new HashMap<>();
        for (int i = 0; i < s.length(); ++i) {
            cnt.merge(s.charAt(i), 1, Integer::sum);
        }
        int sum = 0;
        for (int v : cnt.values()) {
            sum += v & 1;
        }
        return sum < 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPermutePalindrome(string s) {
        unordered_map<char, int> cnt;
        for (auto& c : s) {
            ++cnt[c];
        }
        int sum = 0;
        for (auto& [_, v] : cnt) {
            sum += v & 1;
        }
        return sum < 2;
    }
};
```

#### Go

```go
func canPermutePalindrome(s string) bool {
	cnt := map[rune]int{}
	for _, c := range s {
		cnt[c]++
	}
	sum := 0
	for _, v := range cnt {
		sum += v & 1
	}
	return sum < 2
}
```

#### TypeScript

```ts
function canPermutePalindrome(s: string): boolean {
    const cnt: Record<string, number> = {};
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    return Object.values(cnt).filter(v => v % 2 === 1).length < 2;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn can_permute_palindrome(s: String) -> bool {
        let mut cnt = HashMap::new();
        for c in s.chars() {
            *cnt.entry(c).or_insert(0) += 1;
        }
        cnt.values().filter(|&&v| v % 2 == 1).count() < 2
    }
}
```

#### Swift

```swift
class Solution {
    func canPermutePalindrome(_ s: String) -> Bool {
        var cnt = [Character: Int]()
        for char in s {
            cnt[char, default: 0] += 1
        }

        var sum = 0
        for count in cnt.values {
            sum += count % 2
        }

        return sum < 2
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Cài đặt khác của bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi kiểm tra, không cần giữ lại số lần xuất hiện đầy đủ; chỉ cần quan tâm đến tính chẵn lẻ.
>
> Chỉ cần một set chứa các ký tự hiện có số lần xuất hiện lẻ: xóa khi gặp ký tự lần thứ hai, nếu không thì thêm vào. Kích thước set cuối cùng chính là số tần suất lẻ, tương đương với điều kiện “có nhiều nhất một tần suất lẻ”, đồng thời có constant nhỏ hơn.

<!-- thinking:end -->

Dùng hash table $vis$ để lưu việc mỗi ký tự đã xuất hiện hay chưa. Nếu ký tự đã xuất hiện, ta xóa ký tự đó khỏi hash table; nếu chưa, ta thêm ký tự vào hash table.

Cuối cùng, kiểm tra xem số ký tự trong hash table có nhỏ hơn $2$ hay không. Nếu có, chuỗi là một hoán vị palindrome.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        vis = set()
        for c in s:
            if c in vis:
                vis.remove(c)
            else:
                vis.add(c)
        return len(vis) < 2
```

#### Java

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Set<Character> vis = new HashSet<>();
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (!vis.add(c)) {
                vis.remove(c);
            }
        }
        return vis.size() < 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPermutePalindrome(string s) {
        unordered_set<char> vis;
        for (auto& c : s) {
            if (vis.count(c)) {
                vis.erase(c);
            } else {
                vis.insert(c);
            }
        }
        return vis.size() < 2;
    }
};
```

#### Go

```go
func canPermutePalindrome(s string) bool {
	vis := map[rune]bool{}
	for _, c := range s {
		if vis[c] {
			delete(vis, c)
		} else {
			vis[c] = true
		}
	}
	return len(vis) < 2
}
```

#### TypeScript

```ts
function canPermutePalindrome(s: string): boolean {
    const vis = new Set<string>();
    for (const c of s) {
        if (vis.has(c)) {
            vis.delete(c);
        } else {
            vis.add(c);
        }
    }
    return vis.size < 2;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn can_permute_palindrome(s: String) -> bool {
        let mut vis = HashSet::new();
        for c in s.chars() {
            if vis.contains(&c) {
                vis.remove(&c);
            } else {
                vis.insert(c);
            }
        }
        vis.len() < 2
    }
}
```

#### Swift

```swift
class Solution {
    func canPermutePalindrome(_ s: String) -> Bool {
        var vis = Set<Character>()
        for c in s {
            if vis.contains(c) {
                vis.remove(c)
            } else {
                vis.insert(c)
            }
        }
        return vis.count < 2
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
