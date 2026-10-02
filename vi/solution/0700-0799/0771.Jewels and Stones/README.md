---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [771. Jewels and Stones](https://leetcode.com/problems/jewels-and-stones)

[中文文档](/solution/0700-0799/0771.Jewels%20and%20Stones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>jewels</code> biểu diễn các loại đá quý và <code>stones</code> biểu diễn những viên đá bạn có. Mỗi ký tự trong <code>stones</code> đại diện cho một loại đá. Hãy xác định có bao nhiêu viên đá bạn có cũng là đá quý.</p>

<p>Chữ cái có phân biệt hoa thường, vì vậy <code>&quot;a&quot;</code> được xem là loại đá khác với <code>&quot;A&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> jewels = "aA", stones = "aAAbbbb"
<strong>Đầu ra:</strong> 3
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> jewels = "z", stones = "ZZ"
<strong>Đầu ra:</strong> 0
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;jewels.length, stones.length &lt;= 50</code></li>
	<li><code>jewels</code> và <code>stones</code> chỉ gồm các chữ cái tiếng Anh.</li>
	<li>Tất cả ký tự trong <code>jewels</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số viên đá là đá quý. Độ dài tối đa là $50$: đưa các ký tự trong jewels vào một set rồi kiểm tra từng viên đá. Phân biệt chữ hoa và chữ thường.

<!-- thinking:end -->

Trước tiên, dùng hash table hoặc mảng $s$ để ghi nhận các loại đá quý. Sau đó duyệt tất cả viên đá; nếu viên hiện tại là đá quý thì tăng đáp án lên một.

Độ phức tạp thời gian là $O(m+n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $m$ và $n$ lần lượt là độ dài của hai chuỗi $jewels$ và $stones$; $\Sigma$ là tập ký tự, ở bài này gồm tất cả chữ cái tiếng Anh viết hoa và viết thường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        s = set(jewels)
        return sum(c in s for c in stones)
```

#### Java

```java
class Solution {
    public int numJewelsInStones(String jewels, String stones) {
        int[] s = new int[128];
        for (char c : jewels.toCharArray()) {
            s[c] = 1;
        }
        int ans = 0;
        for (char c : stones.toCharArray()) {
            ans += s[c];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        int s[128] = {0};
        for (char c : jewels) {
            s[c] = 1;
        }
        int ans = 0;
        for (char c : stones) {
            ans += s[c];
        }
        return ans;
    }
};
```

#### Go

```go
func numJewelsInStones(jewels string, stones string) (ans int) {
	s := [128]int{}
	for _, c := range jewels {
		s[c] = 1
	}
	for _, c := range stones {
		ans += s[c]
	}
	return
}
```

#### TypeScript

```ts
function numJewelsInStones(jewels: string, stones: string): number {
    const s = new Set([...jewels]);
    let ans = 0;
    for (const c of stones) {
        s.has(c) && ans++;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn num_jewels_in_stones(jewels: String, stones: String) -> i32 {
        let mut s = jewels.as_bytes().iter().collect::<HashSet<&u8>>();
        let mut ans = 0;
        for c in stones.as_bytes() {
            if s.contains(c) {
                ans += 1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} jewels
 * @param {string} stones
 * @return {number}
 */
var numJewelsInStones = function (jewels, stones) {
    const s = new Set(jewels.split(''));
    return stones.split('').reduce((prev, val) => prev + s.has(val), 0);
};
```

#### C

```c
int numJewelsInStones(char* jewels, char* stones) {
    int set[128] = {0};
    for (int i = 0; jewels[i]; i++) {
        set[jewels[i]] = 1;
    }
    int ans = 0;
    for (int i = 0; stones[i]; i++) {
        set[stones[i]] && ans++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
