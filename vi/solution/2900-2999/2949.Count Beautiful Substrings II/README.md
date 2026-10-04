---
comments: true
difficulty: Hard
rating: 2444
source: Weekly Contest 373 Q4
tags:
    - Hash Table
    - Math
    - String
    - Number Theory
    - Prefix Sum
---

<!-- problem:start -->

# [2949. Count Beautiful Substrings II](https://leetcode.com/problems/count-beautiful-substrings-ii)

[中文文档](/solution/2900-2999/2949.Count%20Beautiful%20Substrings%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> và một số nguyên dương <code>k</code>.</p>

<p>Gọi <code>vowels</code> và <code>consonants</code> lần lượt là số lượng nguyên âm và phụ âm trong một chuỗi.</p>

<p>Một chuỗi được gọi là <strong>đẹp</strong> nếu:</p>

<ul>
	<li><code>vowels == consonants</code>.</li>
	<li><code>(vowels * consonants) % k == 0</code>, nói cách khác, tích của <code>vowels</code> và <code>consonants</code> chia hết cho <code>k</code>.</li>
</ul>

<p>Trả về <em>số lượng <strong>chuỗi con đẹp không rỗng</strong> trong chuỗi đã cho</em> <code>s</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p><strong>Nguyên âm</strong> trong tiếng Anh là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p><strong>Phụ âm</strong> trong tiếng Anh là mọi chữ cái ngoại trừ nguyên âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;baeyh&quot;, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 chuỗi con đẹp trong chuỗi đã cho.
- Chuỗi con &quot;b<u>aeyh</u>&quot;, vowels = 2 ([&quot;a&quot;,e&quot;]), consonants = 2 ([&quot;y&quot;,&quot;h&quot;]).
Có thể thấy chuỗi &quot;aeyh&quot; là chuỗi đẹp vì vowels == consonants và vowels * consonants % k == 0.
- Chuỗi con &quot;<u>baey</u>h&quot;, vowels = 2 ([&quot;a&quot;,e&quot;]), consonants = 2 ([&quot;b&quot;,&quot;y&quot;]).
Có thể thấy chuỗi &quot;baey&quot; là chuỗi đẹp vì vowels == consonants và vowels * consonants % k == 0.
Có thể chứng minh rằng chỉ có 2 chuỗi con đẹp trong chuỗi đã cho.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abba&quot;, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 chuỗi con đẹp trong chuỗi đã cho.
- Chuỗi con &quot;<u>ab</u>ba&quot;, vowels = 1 ([&quot;a&quot;]), consonants = 1 ([&quot;b&quot;]).
- Chuỗi con &quot;ab<u>ba</u>&quot;, vowels = 1 ([&quot;a&quot;]), consonants = 1 ([&quot;b&quot;]).
- Chuỗi con &quot;<u>abba</u>&quot;, vowels = 2 ([&quot;a&quot;,&quot;a&quot;]), consonants = 2 ([&quot;b&quot;,&quot;b&quot;]).
Có thể chứng minh rằng chỉ có 3 chuỗi con đẹp trong chuỗi đã cho.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcdf&quot;, k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi con đẹp nào trong chuỗi đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện này giống phần I, nhưng $n \le 5 \times 10^4$ khiến việc liệt kê các điểm kết thúc là không thể. Số lượng nguyên âm và phụ âm bằng nhau có nghĩa là một prefix với giá trị $+1$ cho nguyên âm và $-1$ cho phụ âm sẽ lặp lại; $v^2 \equiv 0 \pmod k$ với $v=len/2$ buộc độ dài phải là bội của một $l$ được suy ra từ square-free kernel của $4k$.
>
> Lưu $(i \bmod l,\ prefix)$ vào hash map; các vị trí trước đó có cùng key sẽ tạo thành một chuỗi con đẹp với $i$. Dịch prefix thêm $n$ giúp các key không âm.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function beautifulSubstrings(s: string, k: number): number {
    const l = pSqrt(k * 4);
    const n = s.length;
    let sum = n;
    let ans = 0;
    const counter = new Map();
    counter.set(((l - 1) << 17) | sum, 1);
    for (let i = 0; i < n; i++) {
        const char = s[i];
        const bit = (AEIOU_MASK >> (char.charCodeAt(0) - 'a'.charCodeAt(0))) & 1;
        sum += bit * 2 - 1; // 1 -> 1    0 -> -1
        const key = ((i % l) << 17) | sum;
        ans += counter.get(key) || 0; // ans += cnt[(i%k,sum)]++
        counter.set(key, (counter.get(key) ?? 0) + 1);
    }
    return ans;
}
const AEIOU_MASK = 1065233;

function pSqrt(n: number) {
    let res = 1;
    for (let i = 2; i * i <= n; i++) {
        let i2 = i * i;
        while (n % i2 == 0) {
            res *= i;
            n /= i2;
        }
        if (n % i == 0) {
            res *= i;
            n /= i;
        }
    }
    if (n > 1) {
        res *= n;
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
