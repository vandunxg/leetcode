---
comments: true
difficulty: Hard
rating: 2062
source: Weekly Contest 278 Q3
tags:
    - String
    - Sliding Window
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [2156. Find Substring With Given Hash Value](https://leetcode.com/problems/find-substring-with-given-hash-value)

[Tài liệu tiếng Trung](/solution/2100-2199/2156.Find%20Substring%20With%20Given%20Hash%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Hash của chuỗi <strong>0-indexed</strong> <code>s</code> có độ dài <code>k</code>, với hai số nguyên <code>p</code> và <code>m</code>, được tính bằng hàm sau:</p>

<ul>
	<li><code>hash(s, p, m) = (val(s[0]) * p<sup>0</sup> + val(s[1]) * p<sup>1</sup> + ... + val(s[k-1]) * p<sup>k-1</sup>) mod m</code>.</li>
</ul>

<p>Trong đó, <code>val(s[i])</code> biểu thị vị trí của <code>s[i]</code> trong bảng chữ cái, từ <code>val(&#39;a&#39;) = 1</code> đến <code>val(&#39;z&#39;) = 26</code>.</p>

<p>Cho chuỗi <code>s</code> và các số nguyên <code>power</code>, <code>modulo</code>, <code>k</code> và <code>hashValue.</code> Hãy trả về <code>sub</code>,<em> <strong>chuỗi con</strong> <strong>đầu tiên</strong> của </em><code>s</code><em> có độ dài </em><code>k</code><em> sao cho </em><code>hash(sub, power, modulo) == hashValue</code>.</p>

<p>Các test case được tạo sao cho luôn <strong>tồn tại</strong> đáp án.</p>

<p><b>Chuỗi con</b> là một dãy ký tự liên tiếp, không rỗng, nằm trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;leetcode&quot;, power = 7, modulo = 20, k = 2, hashValue = 0
<strong>Đầu ra:</strong> &quot;ee&quot;
<strong>Giải thích:</strong> Hash của &quot;ee&quot; được tính như sau: hash(&quot;ee&quot;, 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0.
&quot;ee&quot; là chuỗi con đầu tiên có độ dài 2 với hashValue bằng 0. Vì vậy, ta trả về &quot;ee&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;fbxzaad&quot;, power = 31, modulo = 100, k = 3, hashValue = 32
<strong>Đầu ra:</strong> &quot;fbx&quot;
<strong>Giải thích:</strong> Hash của &quot;fbx&quot; được tính như sau: hash(&quot;fbx&quot;, 31, 100) = (6 * 1 + 2 * 31 + 24 * 31<sup>2</sup>) mod 100 = 23132 mod 100 = 32.
Hash của &quot;bxz&quot; được tính như sau: hash(&quot;bxz&quot;, 31, 100) = (2 * 1 + 24 * 31 + 26 * 31<sup>2</sup>) mod 100 = 25732 mod 100 = 32.
&quot;fbx&quot; là chuỗi con đầu tiên có độ dài 3 với hashValue bằng 32. Vì vậy, ta trả về &quot;fbx&quot;.
Lưu ý rằng &quot;bxz&quot; cũng có hash bằng 32, nhưng xuất hiện sau &quot;fbx&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= power, modulo &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= hashValue &lt; modulo</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Các test case được tạo sao cho luôn <strong>tồn tại</strong> đáp án.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chuỗi con độ dài $k$ ở ngoài cùng bên phải có hash bằng giá trị đã cho. Nếu duyệt xuôi, ta phải chia cho $p$ theo modulo $m$, việc này cần phần tử nghịch đảo. Với $n\le 10^5$, ta cần một cửa sổ tuyến tính.
>
> Rabin–Karp ngược loại bỏ chữ số cao cũ, nhân với $p$ rồi thêm chữ số thấp mới, nên chỉ cần các phép nhân và modulo.
>
> Tính hash của $k$ ký tự cuối, lưu $p^{k-1}$, trượt sang trái và cập nhật vị trí bắt đầu mỗi khi tìm thấy kết quả phù hợp.

<!-- thinking:end -->

Ta có thể duy trì một cửa sổ trượt có độ dài $k$ để tính giá trị hash của chuỗi con. Nếu duyệt chuỗi theo thứ tự xuôi, việc tính hash cần các phép chia và modulo, tương đối khó xử lý. Vì vậy, ta có thể duyệt chuỗi theo thứ tự ngược, khi đó việc tính hash chỉ cần các phép nhân và modulo.

Đầu tiên, ta tính giá trị hash của $k$ ký tự cuối chuỗi, sau đó bắt đầu duyệt ngược chuỗi từ cuối. Mỗi lần tính giá trị hash của cửa sổ hiện tại, nếu nó bằng giá trị hash đã cho, ta tìm được một chuỗi con thỏa mãn điều kiện và cập nhật vị trí bắt đầu của chuỗi đáp án.

Cuối cùng, trả về chuỗi đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subStrHash(
        self, s: str, power: int, modulo: int, k: int, hashValue: int
    ) -> str:
        h, n = 0, len(s)
        p = 1
        for i in range(n - 1, n - 1 - k, -1):
            val = ord(s[i]) - ord("a") + 1
            h = ((h * power) + val) % modulo
            if i != n - k:
                p = p * power % modulo
        j = n - k
        for i in range(n - 1 - k, -1, -1):
            pre = ord(s[i + k]) - ord("a") + 1
            cur = ord(s[i]) - ord("a") + 1
            h = ((h - pre * p) * power + cur) % modulo
            if h == hashValue:
                j = i
        return s[j : j + k]
```

#### Java

```java
class Solution {
    public String subStrHash(String s, int power, int modulo, int k, int hashValue) {
        long h = 0, p = 1;
        int n = s.length();
        for (int i = n - 1; i >= n - k; --i) {
            int val = s.charAt(i) - 'a' + 1;
            h = ((h * power % modulo) + val) % modulo;
            if (i != n - k) {
                p = p * power % modulo;
            }
        }
        int j = n - k;
        for (int i = n - k - 1; i >= 0; --i) {
            int pre = s.charAt(i + k) - 'a' + 1;
            int cur = s.charAt(i) - 'a' + 1;
            h = ((h - pre * p % modulo + modulo) * power % modulo + cur) % modulo;
            if (h == hashValue) {
                j = i;
            }
        }
        return s.substring(j, j + k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string subStrHash(string s, int power, int modulo, int k, int hashValue) {
        long long h = 0, p = 1;
        int n = s.size();
        for (int i = n - 1; i >= n - k; --i) {
            int val = s[i] - 'a' + 1;
            h = ((h * power % modulo) + val) % modulo;
            if (i != n - k) {
                p = p * power % modulo;
            }
        }
        int j = n - k;
        for (int i = n - k - 1; i >= 0; --i) {
            int pre = s[i + k] - 'a' + 1;
            int cur = s[i] - 'a' + 1;
            h = ((h - pre * p % modulo + modulo) * power % modulo + cur) % modulo;
            if (h == hashValue) {
                j = i;
            }
        }
        return s.substr(j, k);
    }
};
```

#### Go

```go
func subStrHash(s string, power int, modulo int, k int, hashValue int) string {
	h, p := 0, 1
	n := len(s)
	for i := n - 1; i >= n-k; i-- {
		val := int(s[i] - 'a' + 1)
		h = (h*power%modulo + val) % modulo
		if i != n-k {
			p = p * power % modulo
		}
	}
	j := n - k
	for i := n - k - 1; i >= 0; i-- {
		pre := int(s[i+k] - 'a' + 1)
		cur := int(s[i] - 'a' + 1)
		h = ((h-pre*p%modulo+modulo)*power%modulo + cur) % modulo
		if h == hashValue {
			j = i
		}
	}
	return s[j : j+k]
}
```

#### TypeScript

```ts
function subStrHash(
    s: string,
    power: number,
    modulo: number,
    k: number,
    hashValue: number,
): string {
    let h: bigint = BigInt(0),
        p: bigint = BigInt(1);
    const n: number = s.length;
    const mod = BigInt(modulo);
    for (let i: number = n - 1; i >= n - k; --i) {
        const val: bigint = BigInt(s.charCodeAt(i) - 'a'.charCodeAt(0) + 1);
        h = (((h * BigInt(power)) % mod) + val) % mod;
        if (i !== n - k) {
            p = (p * BigInt(power)) % mod;
        }
    }
    let j: number = n - k;
    for (let i: number = n - k - 1; i >= 0; --i) {
        const pre: bigint = BigInt(s.charCodeAt(i + k) - 'a'.charCodeAt(0) + 1);
        const cur: bigint = BigInt(s.charCodeAt(i) - 'a'.charCodeAt(0) + 1);
        h = ((((h - ((pre * p) % mod) + mod) * BigInt(power)) % mod) + cur) % mod;
        if (Number(h) === hashValue) {
            j = i;
        }
    }
    return s.substring(j, j + k);
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} power
 * @param {number} modulo
 * @param {number} k
 * @param {number} hashValue
 * @return {string}
 */
var subStrHash = function (s, power, modulo, k, hashValue) {
    let h = BigInt(0),
        p = BigInt(1);
    const n = s.length;
    const mod = BigInt(modulo);
    for (let i = n - 1; i >= n - k; --i) {
        const val = BigInt(s.charCodeAt(i) - 'a'.charCodeAt(0) + 1);
        h = (((h * BigInt(power)) % mod) + val) % mod;
        if (i !== n - k) {
            p = (p * BigInt(power)) % mod;
        }
    }
    let j = n - k;
    for (let i = n - k - 1; i >= 0; --i) {
        const pre = BigInt(s.charCodeAt(i + k) - 'a'.charCodeAt(0) + 1);
        const cur = BigInt(s.charCodeAt(i) - 'a'.charCodeAt(0) + 1);
        h = ((((h - ((pre * p) % mod) + mod) * BigInt(power)) % mod) + cur) % mod;
        if (Number(h) === hashValue) {
            j = i;
        }
    }
    return s.substring(j, j + k);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
