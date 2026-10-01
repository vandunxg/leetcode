---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - String
---

<!-- problem:start -->

# [318. Maximum Product of Word Lengths](https://leetcode.com/problems/maximum-product-of-word-lengths)

[中文文档](/solution/0300-0399/0318.Maximum%20Product%20of%20Word%20Lengths/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code>. Hãy trả về <em>giá trị lớn nhất của</em> <code>length(word[i]) * length(word[j])</code> <em>với điều kiện hai từ không có chữ cái chung</em>. Nếu không tồn tại cặp từ nào như vậy, hãy trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abcw&quot;,&quot;baz&quot;,&quot;foo&quot;,&quot;bar&quot;,&quot;xtfn&quot;,&quot;abcdef&quot;]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Có thể chọn hai từ &quot;abcw&quot; và &quot;xtfn&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;ab&quot;,&quot;abc&quot;,&quot;d&quot;,&quot;cd&quot;,&quot;bcd&quot;,&quot;abcd&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có thể chọn hai từ &quot;ab&quot; và &quot;cd&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;aa&quot;,&quot;aaa&quot;,&quot;aaaa&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cặp từ nào thỏa mãn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Tích chỉ được tính khi hai từ không có chữ cái chung. Kiểm tra giao của hai tập chữ cái mất $O(\sigma)$ cho mỗi cặp, nên ta cần cách kiểm tra rời nhau hiệu quả hơn.
>
> Mã hóa mỗi từ thành một mask 26 bit; nếu phép AND bitwise cho kết quả 0 thì hai từ không có chữ cái chung. Kiểm tra từng cặp $i>j$ và giữ lại tích lớn nhất. Với $n\le 1000$, độ phức tạp $O(n^2)$ là chấp nhận được.

<!-- thinking:end -->

Bài toán yêu cầu tìm hai chuỗi không có chữ cái chung sao cho tích độ dài của chúng lớn nhất. Ta có thể biểu diễn mỗi chuỗi bằng số nhị phân $mask[i]$, trong đó mỗi bit cho biết chuỗi có chứa một chữ cái nhất định hay không. Nếu hai chuỗi không có chữ cái chung, phép AND bitwise trên hai số nhị phân tương ứng sẽ cho kết quả $0$, tức là $mask[i] \& mask[j] = 0$.

Ta duyệt từng chuỗi. Với chuỗi hiện tại $words[i]$, trước tiên ta tính số nhị phân tương ứng $mask[i]$, rồi duyệt mọi chuỗi $words[j]$ với $j \in [0, i)$. Ta kiểm tra xem $mask[i] \& mask[j] = 0$ có đúng không. Nếu đúng, cập nhật đáp án thành $\max(ans, |words[i]| \times |words[j]|)$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2 + L)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số chuỗi trong mảng $words$, còn $L$ là tổng độ dài của tất cả các chuỗi trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, words: List[str]) -> int:
        mask = [0] * len(words)
        ans = 0
        for i, s in enumerate(words):
            for c in s:
                mask[i] |= 1 << (ord(c) - ord("a"))
            for j, t in enumerate(words[:i]):
                if (mask[i] & mask[j]) == 0:
                    ans = max(ans, len(s) * len(t))
        return ans
```

#### Java

```java
class Solution {
    public int maxProduct(String[] words) {
        int n = words.length;
        int[] mask = new int[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (char c : words[i].toCharArray()) {
                mask[i] |= 1 << (c - 'a');
            }
            for (int j = 0; j < i; ++j) {
                if ((mask[i] & mask[j]) == 0) {
                    ans = Math.max(ans, words[i].length() * words[j].length());
                }
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
    int maxProduct(vector<string>& words) {
        int n = words.size();
        int mask[n];
        memset(mask, 0, sizeof(mask));
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (char& c : words[i]) {
                mask[i] |= 1 << (c - 'a');
            }
            for (int j = 0; j < i; ++j) {
                if ((mask[i] & mask[j]) == 0) {
                    ans = max(ans, (int) (words[i].size() * words[j].size()));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxProduct(words []string) (ans int) {
	n := len(words)
	mask := make([]int, n)
	for i, s := range words {
		for _, c := range s {
			mask[i] |= 1 << (c - 'a')
		}
		for j, t := range words[:i] {
			if mask[i]&mask[j] == 0 {
				ans = max(ans, len(s)*len(t))
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function maxProduct(words: string[]): number {
    const n = words.length;
    const mask: number[] = Array(n).fill(0);
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (const c of words[i]) {
            mask[i] |= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        }
        for (let j = 0; j < i; ++j) {
            if ((mask[i] & mask[j]) === 0) {
                ans = Math.max(ans, words[i].length * words[j].length);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 có thể so sánh nhiều lần các từ dùng chung một mask. Ta lưu từ dài nhất cho mỗi mask và chỉ so sánh với các mask đã gặp. Các từ có cùng tập chữ cái được gom về một độ dài duy nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, words: List[str]) -> int:
        mask = defaultdict(int)
        ans = 0
        for s in words:
            a = len(s)
            x = 0
            for c in s:
                x |= 1 << (ord(c) - ord("a"))
            for y, b in mask.items():
                if (x & y) == 0:
                    ans = max(ans, a * b)
            mask[x] = max(mask[x], a)
        return ans
```

#### Java

```java
class Solution {
    public int maxProduct(String[] words) {
        Map<Integer, Integer> mask = new HashMap<>();
        int ans = 0;
        for (var s : words) {
            int a = s.length();
            int x = 0;
            for (char c : s.toCharArray()) {
                x |= 1 << (c - 'a');
            }
            for (var e : mask.entrySet()) {
                int y = e.getKey(), b = e.getValue();
                if ((x & y) == 0) {
                    ans = Math.max(ans, a * b);
                }
            }
            mask.merge(x, a, Math::max);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        unordered_map<int, int> mask;
        int ans = 0;
        for (auto& s : words) {
            int a = s.size();
            int x = 0;
            for (char& c : s) {
                x |= 1 << (c - 'a');
            }
            for (auto& [y, b] : mask) {
                if ((x & y) == 0) {
                    ans = max(ans, a * b);
                }
            }
            mask[x] = max(mask[x], a);
        }
        return ans;
    }
};
```

#### Go

```go
func maxProduct(words []string) (ans int) {
	mask := map[int]int{}
	for _, s := range words {
		a := len(s)
		x := 0
		for _, c := range s {
			x |= 1 << (c - 'a')
		}
		for y, b := range mask {
			if x&y == 0 {
				ans = max(ans, a*b)
			}
		}
		mask[x] = max(mask[x], a)
	}
	return
}
```

#### TypeScript

```ts
function maxProduct(words: string[]): number {
    const mask: Map<number, number> = new Map();
    let ans = 0;
    for (const s of words) {
        const a = s.length;
        let x = 0;
        for (const c of s) {
            x |= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        }
        for (const [y, b] of mask.entries()) {
            if ((x & y) === 0) {
                ans = Math.max(ans, a * b);
            }
        }
        mask.set(x, Math.max(mask.get(x) || 0, a));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
