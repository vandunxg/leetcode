---
comments: true
difficulty: Easy
rating: 1350
source: Biweekly Contest 64 Q1
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2053. Kth Distinct String in an Array](https://leetcode.com/problems/kth-distinct-string-in-an-array)

[中文文档](/solution/2000-2099/2053.Kth%20Distinct%20String%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Chuỗi phân biệt</strong> là chuỗi chỉ xuất hiện <strong>một lần</strong> trong một mảng.</p>

<p>Cho một mảng các chuỗi <code>arr</code> và một số nguyên <code>k</code>, hãy trả về <em>chuỗi </em><code>k<sup>th</sup></code><em> <strong>phân biệt</strong> xuất hiện trong </em><code>arr</code>. Nếu có <strong>ít hơn</strong> <code>k</code> chuỗi phân biệt, hãy trả về <em>một <strong>chuỗi rỗng </strong></em><code>&quot;&quot;</code>.</p>

<p>Lưu ý rằng các chuỗi được xét theo <strong>thứ tự xuất hiện</strong> trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [&quot;d&quot;,&quot;b&quot;,&quot;c&quot;,&quot;b&quot;,&quot;c&quot;,&quot;a&quot;], k = 2
<strong>Output:</strong> &quot;a&quot;
<strong>Giải thích:</strong>
Các chuỗi phân biệt duy nhất trong arr là &quot;d&quot; và &quot;a&quot;.
&quot;d&quot; xuất hiện ở vị trí 1<sup>st</sup>, nên đây là chuỗi phân biệt thứ 1<sup>st</sup>.
&quot;a&quot; xuất hiện ở vị trí 2<sup>nd</sup>, nên đây là chuỗi phân biệt thứ 2<sup>nd</sup>.
Vì k == 2, ta trả về &quot;a&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [&quot;aaa&quot;,&quot;aa&quot;,&quot;a&quot;], k = 1
<strong>Output:</strong> &quot;aaa&quot;
<strong>Giải thích:</strong>
Tất cả các chuỗi trong arr đều phân biệt, nên ta trả về chuỗi thứ 1<sup>st</sup> là &quot;aaa&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [&quot;a&quot;,&quot;b&quot;,&quot;a&quot;], k = 3
<strong>Output:</strong> &quot;&quot;
<strong>Giải thích:</strong>
Chuỗi phân biệt duy nhất là &quot;b&quot;. Vì có ít hơn 3 chuỗi phân biệt, ta trả về chuỗi rỗng &quot;&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i].length &lt;= 5</code></li>
	<li><code>arr[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Counting

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi là phân biệt khi và chỉ khi nó xuất hiện đúng một lần trong toàn bộ mảng. Với $n \le 1000$, trước tiên ta đếm số lần xuất hiện, sau đó duyệt theo thứ tự và giảm $k$ mỗi khi gặp chuỗi có tần suất bằng $1$ cho đến khi k bằng không.
>
> Hai lượt duyệt tuyến tính, không cần cấu trúc chỉ số bổ sung.

<!-- thinking:end -->

Ta có thể sử dụng một hash table $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi chuỗi. Sau đó, ta duyệt lại mảng một lần nữa. Với mỗi chuỗi, nếu số lần xuất hiện của nó là $1$, ta giảm $k$ đi một. Khi $k$ bằng $0$, ta trả về chuỗi hiện tại.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi trong mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthDistinct(self, arr: List[str], k: int) -> str:
        cnt = Counter(arr)
        for s in arr:
            if cnt[s] == 1:
                k -= 1
                if k == 0:
                    return s
        return ""
```

#### Java

```java
class Solution {
    public String kthDistinct(String[] arr, int k) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String s : arr) {
            cnt.merge(s, 1, Integer::sum);
        }
        for (String s : arr) {
            if (cnt.get(s) == 1 && --k == 0) {
                return s;
            }
        }
        return "";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string kthDistinct(vector<string>& arr, int k) {
        unordered_map<string, int> cnt;
        for (const auto& s : arr) {
            ++cnt[s];
        }
        for (const auto& s : arr) {
            if (cnt[s] == 1 && --k == 0) {
                return s;
            }
        }
        return "";
    }
};
```

#### Go

```go
func kthDistinct(arr []string, k int) string {
	cnt := map[string]int{}
	for _, s := range arr {
		cnt[s]++
	}
	for _, s := range arr {
		if cnt[s] == 1 {
			k--
			if k == 0 {
				return s
			}
		}
	}
	return ""
}
```

#### TypeScript

```ts
function kthDistinct(arr: string[], k: number): string {
    const cnt = new Map<string, number>();
    for (const s of arr) {
        cnt.set(s, (cnt.get(s) || 0) + 1);
    }
    for (const s of arr) {
        if (cnt.get(s) === 1 && --k === 0) {
            return s;
        }
    }
    return '';
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn kth_distinct(arr: Vec<String>, mut k: i32) -> String {
        let mut cnt = HashMap::new();

        for s in &arr {
            *cnt.entry(s).or_insert(0) += 1;
        }

        for s in &arr {
            if *cnt.get(s).unwrap() == 1 {
                k -= 1;
                if k == 0 {
                    return s.clone();
                }
            }
        }

        "".to_string()
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} arr
 * @param {number} k
 * @return {string}
 */
var kthDistinct = function (arr, k) {
    const cnt = new Map();
    for (const s of arr) {
        cnt.set(s, (cnt.get(s) || 0) + 1);
    }
    for (const s of arr) {
        if (cnt.get(s) === 1 && --k === 0) {
            return s;
        }
    }
    return '';
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
