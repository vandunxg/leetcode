---
comments: true
difficulty: Easy
tags:
    - Queue
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string)

[中文文档](/solution/0300-0399/0387.First%20Unique%20Character%20in%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy tìm ký tự <strong>đầu tiên</strong> chỉ xuất hiện một lần và trả về chỉ số của nó. Nếu <strong>không tồn tại</strong> ký tự như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ký tự <code>&#39;l&#39;</code> ở chỉ số 0 là ký tự đầu tiên không xuất hiện ở bất kỳ chỉ số nào khác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;loveleetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
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

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm chỉ số của ký tự đầu tiên chỉ xuất hiện một lần. Chỉ duyệt từ trái sang phải một lượt thì chưa thể biết ký tự đó có lặp lại về sau hay không.
>
> Đếm số lần xuất hiện của từng ký tự, sau đó duyệt để tìm ký tự có tần suất $1$; nếu không có thì trả về $-1$. Cần hai lượt duyệt tuyến tính và bộ nhớ theo kích thước bảng chữ cái.

<!-- thinking:end -->

Ta dùng hash table hoặc mảng độ dài $26$ $\text{cnt}$ để lưu tần suất của từng ký tự. Sau đó, duyệt các ký tự $\text{s[i]}$ từ đầu chuỗi. Nếu $\text{cnt[s[i]]}$ bằng $1$, ta trả về $i$.

Nếu duyệt hết chuỗi mà không tìm thấy ký tự nào như vậy, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, với $\Sigma$ là tập ký tự. Trong bài này, tập ký tự gồm các chữ cái viết thường nên $|\Sigma|=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        cnt = Counter(s)
        for i, c in enumerate(s):
            if cnt[c] == 1:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] cnt = new int[26];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        for (int i = 0; i < n; ++i) {
            if (cnt[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (cnt[s[i] - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func firstUniqChar(s string) int {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	for i, c := range s {
		if cnt[c-'a'] == 1 {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function firstUniqChar(s: string): number {
    const cnt = new Map<string, number>();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    for (let i = 0; i < s.length; ++i) {
        if (cnt.get(s[i]) === 1) {
            return i;
        }
    }
    return -1;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function (s) {
    const cnt = new Map();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    for (let i = 0; i < s.length; ++i) {
        if (cnt.get(s[i]) === 1) {
            return i;
        }
    }
    return -1;
};
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Integer
     */
    function firstUniqChar($s) {
        for ($i = 0; $i < strlen($s); $i++) {
            $hashtable[$s[$i]]++;
        }
        for ($i = 0; $i < strlen($s); $i++) {
            if ($hashtable[$s[$i]] == 1) {
                return $i;
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
