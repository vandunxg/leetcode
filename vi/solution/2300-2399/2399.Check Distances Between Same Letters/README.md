---
comments: true
difficulty: Easy
rating: 1243
source: Weekly Contest 309 Q1
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2399. Check Distances Between Same Letters](https://leetcode.com/problems/check-distances-between-same-letters)

[中文文档](/solution/2300-2399/2399.Check%20Distances%20Between%20Same%20Letters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code> chỉ gồm các chữ cái tiếng Anh thường, trong đó mỗi chữ cái trong <code>s</code> xuất hiện <strong>chính xác</strong> <strong>hai lần</strong>. Bạn cũng được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>distance</code> có độ dài <code>26</code>.</p>

<p>Mỗi chữ cái trong bảng chữ cái được đánh số từ <code>0</code> đến <code>25</code> (tức là <code>&#39;a&#39; -&gt; 0</code>, <code>&#39;b&#39; -&gt; 1</code>, <code>&#39;c&#39; -&gt; 2</code>, ... , <code>&#39;z&#39; -&gt; 25</code>).</p>

<p>Trong một chuỗi <strong>cách đều</strong>, số chữ cái nằm giữa hai lần xuất hiện của chữ cái <code>i<sup>th</sup></code> là <code>distance[i]</code>. Nếu chữ cái <code>i<sup>th</sup></code> không xuất hiện trong <code>s</code>, có thể <strong>bỏ qua</strong> <code>distance[i]</code>.</p>

<p>Trả về <code>true</code><em> nếu </em><code>s</code><em> là một chuỗi <strong>cách đều</strong>, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abaccb&quot;, distance = [1,3,0,5,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
- &#39;a&#39; xuất hiện tại các chỉ số 0 và 2 nên thỏa mãn distance[0] = 1.
- &#39;b&#39; xuất hiện tại các chỉ số 1 và 5 nên thỏa mãn distance[1] = 3.
- &#39;c&#39; xuất hiện tại các chỉ số 3 và 4 nên thỏa mãn distance[2] = 0.
Lưu ý rằng distance[3] = 5, nhưng vì &#39;d&#39; không xuất hiện trong s nên có thể bỏ qua.
Trả về true vì s là một chuỗi cách đều.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aa&quot;, distance = [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
- &#39;a&#39; xuất hiện tại các chỉ số 0 và 1 nên không có chữ cái nào nằm giữa chúng.
Vì distance[0] = 1, s không phải là một chuỗi cách đều.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 52</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
	<li>Mỗi chữ cái xuất hiện trong <code>s</code> đúng hai lần.</li>
	<li><code>distance.length == 26</code></li>
	<li><code>0 &lt;= distance[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái xuất hiện hai lần; khoảng cách giữa chúng phải khớp với $distance$. Vì $|s| \le 52$, chỉ cần lưu chỉ số xuất hiện trước đó.
>
> Ghi nhận chỉ số xuất hiện đầu tiên; khi gặp lần thứ hai, so sánh khoảng cách và trả về kết quả sai nếu không khớp. Không cần kiểm tra các chữ cái không xuất hiện.

<!-- thinking:end -->

Ta có thể dùng một hash table $d$ để lưu các chỉ số xuất hiện của từng chữ cái. Sau đó, duyệt qua hash table và kiểm tra xem hiệu giữa các chỉ số của mỗi chữ cái có bằng giá trị tương ứng trong mảng `distance` hay không. Nếu phát hiện bất kỳ điểm không khớp nào, trả về `false`. Nếu duyệt xong mà không có điểm không khớp, trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự, ở đây là tập các chữ cái thường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkDistances(self, s: str, distance: List[int]) -> bool:
        d = defaultdict(int)
        for i, c in enumerate(map(ord, s), 1):
            j = c - ord("a")
            if d[j] and i - d[j] - 1 != distance[j]:
                return False
            d[j] = i
        return True
```

#### Java

```java
class Solution {
    public boolean checkDistances(String s, int[] distance) {
        int[] d = new int[26];
        for (int i = 1; i <= s.length(); ++i) {
            int j = s.charAt(i - 1) - 'a';
            if (d[j] > 0 && i - d[j] - 1 != distance[j]) {
                return false;
            }
            d[j] = i;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkDistances(string s, vector<int>& distance) {
        int d[26]{};
        for (int i = 1; i <= s.size(); ++i) {
            int j = s[i - 1] - 'a';
            if (d[j] && i - d[j] - 1 != distance[j]) {
                return false;
            }
            d[j] = i;
        }
        return true;
    }
};
```

#### Go

```go
func checkDistances(s string, distance []int) bool {
	d := [26]int{}
	for i, c := range s {
		c -= 'a'
		if d[c] > 0 && i-d[c] != distance[c] {
			return false
		}
		d[c] = i + 1
	}
	return true
}
```

#### TypeScript

```ts
function checkDistances(s: string, distance: number[]): boolean {
    const d: number[] = Array(26).fill(0);
    const n = s.length;
    for (let i = 1; i <= n; ++i) {
        const j = s.charCodeAt(i - 1) - 97;
        if (d[j] && i - d[j] - 1 !== distance[j]) {
            return false;
        }
        d[j] = i;
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_distances(s: String, distance: Vec<i32>) -> bool {
        let n = s.len();
        let s = s.as_bytes();
        let mut d = [0; 26];
        for i in 0..n {
            let j = (s[i] - b'a') as usize;
            let i = i as i32;
            if d[j] > 0 && i - d[j] != distance[j] {
                return false;
            }
            d[j] = i + 1;
        }
        true
    }
}
```

#### C

```c
bool checkDistances(char* s, int* distance, int distanceSize) {
    int n = strlen(s);
    int d[26] = {0};
    for (int i = 0; i < n; i++) {
        int j = s[i] - 'a';
        if (d[j] > 0 && i - d[j] != distance[j]) {
            return false;
        }
        d[j] = i + 1;
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
