---
comments: true
difficulty: Medium
rating: 1541
source: Weekly Contest 334 Q2
tags:
    - Array
    - Math
    - String
---

<!-- problem:start -->

# [2575. Find the Divisibility Array of a String](https://leetcode.com/problems/find-the-divisibility-array-of-a-string)

[中文文档](/solution/2500-2599/2575.Find%20the%20Divisibility%20Array%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>, chỉ gồm các chữ số, và một số nguyên dương <code>m</code>.</p>

<p><strong>Mảng chia hết</strong> <code>div</code> của <code>word</code> là một mảng số nguyên có độ dài <code>n</code> sao cho:</p>

<ul>
	<li><code>div[i] = 1</code> nếu <strong>giá trị số</strong> của <code>word[0,...,i]</code> chia hết cho <code>m</code>, hoặc</li>
	<li><code>div[i] = 0</code> trong trường hợp ngược lại.</li>
</ul>

<p>Trả về <em>mảng chia hết của</em><em> </em><code>word</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;998244353&quot;, m = 3
<strong>Đầu ra:</strong> [1,1,0,0,0,1,1,0,0]
<strong>Giải thích:</strong> Chỉ có 4 tiền tố chia hết cho 3: &quot;9&quot;, &quot;99&quot;, &quot;998244&quot; và &quot;9982443&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;1010&quot;, m = 10
<strong>Đầu ra:</strong> [0,1,0,1]
<strong>Giải thích:</strong> Chỉ có 2 tiền tố chia hết cho 10: &quot;10&quot; và &quot;1010&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">word.length == n</font></code></li>
	<li><code><font face="monospace">word</font></code><font face="monospace"> chỉ gồm các chữ số từ <code>0</code>&nbsp;đến <code>9</code></font></li>
	<li><code><font face="monospace">1 &lt;= m &lt;= 10<sup>9</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Modulo

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem giá trị số của từng tiền tố có chia hết cho $m$ hay không. Các tiền tố có thể dài tới $10^5$ chữ số, nên không thể tạo trực tiếp các số đó.
>
> Phần dư được cập nhật theo công thức $x\leftarrow (10x+d)\bmod m$. Khi phần dư bằng 0, ghi $1$; ngược lại ghi $0$.

<!-- thinking:end -->

Ta duyệt chuỗi `word`, sử dụng biến $x$ để lưu kết quả phép modulo của tiền tố hiện tại với $m$. Nếu $x$ bằng $0$, giá trị tại vị trí hiện tại của mảng chia hết là $1$, ngược lại là $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `word`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divisibilityArray(self, word: str, m: int) -> List[int]:
        ans = []
        x = 0
        for c in word:
            x = (x * 10 + int(c)) % m
            ans.append(1 if x == 0 else 0)
        return ans
```

#### Java

```java
class Solution {
    public int[] divisibilityArray(String word, int m) {
        int n = word.length();
        int[] ans = new int[n];
        long x = 0;
        for (int i = 0; i < n; ++i) {
            x = (x * 10 + word.charAt(i) - '0') % m;
            if (x == 0) {
                ans[i] = 1;
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
    vector<int> divisibilityArray(string word, int m) {
        vector<int> ans;
        long long x = 0;
        for (char& c : word) {
            x = (x * 10 + c - '0') % m;
            ans.push_back(x == 0 ? 1 : 0);
        }
        return ans;
    }
};
```

#### Go

```go
func divisibilityArray(word string, m int) (ans []int) {
	x := 0
	for _, c := range word {
		x = (x*10 + int(c-'0')) % m
		if x == 0 {
			ans = append(ans, 1)
		} else {
			ans = append(ans, 0)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function divisibilityArray(word: string, m: number): number[] {
    const ans: number[] = [];
    let x = 0;
    for (const c of word) {
        x = (x * 10 + Number(c)) % m;
        ans.push(x === 0 ? 1 : 0);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn divisibility_array(word: String, m: i32) -> Vec<i32> {
        let m = m as i64;
        let mut x = 0i64;
        word.as_bytes()
            .iter()
            .map(|&c| {
                x = (x * 10 + i64::from(c - b'0')) % m;
                if x == 0 {
                    1
                } else {
                    0
                }
            })
            .collect()
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* divisibilityArray(char* word, int m, int* returnSize) {
    int n = strlen(word);
    int* ans = malloc(sizeof(int) * n);
    long long x = 0;
    for (int i = 0; i < n; i++) {
        x = (x * 10 + word[i] - '0') % m;
        ans[i] = x == 0 ? 1 : 0;
    }
    *returnSize = n;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
