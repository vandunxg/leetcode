---
comments: true
difficulty: Medium
rating: 2234
source: Weekly Contest 247 Q3
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1915. Number of Wonderful Substrings](https://leetcode.com/problems/number-of-wonderful-substrings)

[中文文档](/solution/1900-1999/1915.Number%20of%20Wonderful%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <strong>wonderful</strong> là chuỗi mà <strong>nhiều nhất một</strong> chữ cái xuất hiện với số lần <strong>lẻ</strong>.</p>

<ul>
	<li>Ví dụ, <code>&quot;ccjjc&quot;</code> và <code>&quot;abab&quot;</code> là các chuỗi wonderful, còn <code>&quot;ab&quot;</code> thì không.</li>
</ul>

<p>Cho một chuỗi <code>word</code> chỉ gồm mười chữ cái tiếng Anh thường đầu tiên (từ <code>&#39;a&#39;</code> đến <code>&#39;j&#39;</code>), hãy trả về <em><strong>số lượng chuỗi con không rỗng wonderful</strong> trong </em><code>word</code><em>. Nếu cùng một chuỗi con xuất hiện nhiều lần trong </em><code>word</code><em>, hãy đếm <strong>từng lần xuất hiện</strong> riêng biệt.</em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aba&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bốn chuỗi con wonderful được gạch chân bên dưới:
- &quot;<u><strong>a</strong></u>ba&quot; -&gt; &quot;a&quot;
- &quot;a<u><strong>b</strong></u>a&quot; -&gt; &quot;b&quot;
- &quot;ab<u><strong>a</strong></u>&quot; -&gt; &quot;a&quot;
- &quot;<u><strong>aba</strong></u>&quot; -&gt; &quot;aba&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aabb&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Chín chuỗi con wonderful được gạch chân bên dưới:
- &quot;<strong><u>a</u></strong>abb&quot; -&gt; &quot;a&quot;
- &quot;<u><strong>aa</strong></u>bb&quot; -&gt; &quot;aa&quot;
- &quot;<u><strong>aab</strong></u>b&quot; -&gt; &quot;aab&quot;
- &quot;<u><strong>aabb</strong></u>&quot; -&gt; &quot;aabb&quot;
- &quot;a<u><strong>a</strong></u>bb&quot; -&gt; &quot;a&quot;
- &quot;a<u><strong>abb</strong></u>&quot; -&gt; &quot;abb&quot;
- &quot;aa<u><strong>b</strong></u>b&quot; -&gt; &quot;b&quot;
- &quot;aa<u><strong>bb</strong></u>&quot; -&gt; &quot;bb&quot;
- &quot;aab<u><strong>b</strong></u>&quot; -&gt; &quot;b&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;he&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai chuỗi con wonderful được gạch chân bên dưới:
- &quot;<b><u>h</u></b>e&quot; -&gt; &quot;h&quot;
- &quot;h<strong><u>e</u></strong>&quot; -&gt; &quot;e&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> gồm các chữ cái tiếng Anh thường từ <code>&#39;a&#39;</code>&nbsp;đến <code>&#39;j&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: XOR tiền tố + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số lần xuất hiện lẻ của các ký tự trong mọi chuỗi con sẽ tốn $O(n^2\cdot\Sigma)$ và không đáp ứng được với $n\le 10^5$.
>
> Mười chữ cái có thể được biểu diễn bằng một mask parity 10 bit. Hai XOR tiền tố bằng nhau tạo ra một đoạn có số lần xuất hiện của mọi ký tự là chẵn; hai XOR khác nhau đúng một bit tạo ra một ký tự xuất hiện lẻ.
>
> Tại mỗi vị trí, ta cộng số lượng các tiền tố trước đó có mask bằng mask hiện tại hoặc khác đúng một bit, sau đó ghi nhận mask hiện tại.

<!-- thinking:end -->

Vì chuỗi chỉ chứa $10$ chữ cái thường, ta có thể dùng một số nguyên $10$-bit để biểu diễn parity của số lần xuất hiện mỗi chữ cái trong tiền tố hiện tại. Bit thứ $i$ bằng $1$ nếu chữ cái thứ $i$ xuất hiện với số lần lẻ, và bằng $0$ nếu xuất hiện với số lần chẵn.

Ta duyệt qua từng ký tự trong chuỗi. Dùng biến $st$ để duy trì trạng thái XOR tiền tố hiện tại và một mảng $cnt$ để ghi nhận số lần mỗi trạng thái tiền tố đã xuất hiện. Ban đầu, $st = 0$ và $cnt[0] = 1$.

Với mỗi ký tự, ta cập nhật trạng thái XOR tiền tố. Nếu trạng thái hiện tại đã xuất hiện $cnt[st]$ lần, điều đó có nghĩa là có $cnt[st]$ chuỗi con trong đó số lần xuất hiện của mọi chữ cái đều là chẵn, nên ta cộng $cnt[st]$ vào đáp án. Ngoài ra, với $0 \le i < 10$, việc đảo bit thứ $i$ của $st$ (tức là $st \oplus (1 << i)$) biểu diễn các chuỗi con trong đó có đúng một chữ cái xuất hiện với số lần lẻ, nên ta cộng $cnt[st \oplus (1 << i)]$ vào đáp án. Cuối cùng, tăng số lần xuất hiện của $st$ lên $1$ rồi tiếp tục.

Độ phức tạp thời gian là $O(n \times \Sigma)$, còn độ phức tạp không gian là $O(2^{\Sigma})$, trong đó $\Sigma = 10$ và $n$ là độ dài chuỗi.

Bài tương tự:

- [1371. Chuỗi con dài nhất chứa mỗi nguyên âm với số lần xuất hiện chẵn](https://github.com/doocs/leetcode/blob/main/solution/1300-1399/1371.Find%20the%20Longest%20Substring%20Containing%20Vowels%20in%20Even%20Counts/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wonderfulSubstrings(self, word: str) -> int:
        cnt = defaultdict(int)
        cnt[0] = 1
        ans = st = 0
        for c in word:
            st ^= 1 << (ord(c) - ord("a"))
            ans += cnt[st]
            ans += sum(cnt[st ^ (1 << i)] for i in range(10))
            cnt[st] += 1
        return ans
```

#### Java

```java
class Solution {
    public long wonderfulSubstrings(String word) {
        int[] cnt = new int[1 << 10];
        cnt[0] = 1;
        long ans = 0;
        int st = 0;
        for (char c : word.toCharArray()) {
            st ^= 1 << (c - 'a');
            ans += cnt[st];
            for (int i = 0; i < 10; ++i) {
                ans += cnt[st ^ (1 << i)];
            }
            ++cnt[st];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long wonderfulSubstrings(string word) {
        int cnt[1024] = {1};
        long long ans = 0;
        int st = 0;
        for (char c : word) {
            st ^= 1 << (c - 'a');
            ans += cnt[st];
            for (int i = 0; i < 10; ++i) {
                ans += cnt[st ^ (1 << i)];
            }
            ++cnt[st];
        }
        return ans;
    }
};
```

#### Go

```go
func wonderfulSubstrings(word string) (ans int64) {
	cnt := [1024]int{1}
	st := 0
	for _, c := range word {
		st ^= 1 << (c - 'a')
		ans += int64(cnt[st])
		for i := 0; i < 10; i++ {
			ans += int64(cnt[st^(1<<i)])
		}
		cnt[st]++
	}
	return
}
```

#### TypeScript

```ts
function wonderfulSubstrings(word: string): number {
    const cnt: number[] = new Array(1 << 10).fill(0);
    cnt[0] = 1;
    let ans = 0;
    let st = 0;
    for (const c of word) {
        st ^= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        ans += cnt[st];
        for (let i = 0; i < 10; ++i) {
            ans += cnt[st ^ (1 << i)];
        }
        cnt[st]++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn wonderful_substrings(word: String) -> i64 {
        let mut cnt = [0i64; 1 << 10];
        cnt[0] = 1;
        let mut ans: i64 = 0;
        let mut st: usize = 0;
        for c in word.chars() {
            st ^= 1 << (c as usize - 'a' as usize);
            ans += cnt[st];
            for i in 0..10 {
                ans += cnt[st ^ (1 << i)];
            }
            cnt[st] += 1;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} word
 * @return {number}
 */
var wonderfulSubstrings = function (word) {
    const cnt = new Array(1024).fill(0);
    cnt[0] = 1;
    let ans = 0;
    let st = 0;
    for (const c of word) {
        st ^= 1 << (c.charCodeAt() - 'a'.charCodeAt());
        ans += cnt[st];
        for (let i = 0; i < 10; ++i) {
            ans += cnt[st ^ (1 << i)];
        }
        cnt[st]++;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
