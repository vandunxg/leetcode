---
comments: true
difficulty: Easy
rating: 1248
source: Biweekly Contest 53 Q1
tags:
    - Hash Table
    - String
    - Counting
    - Sliding Window
---

<!-- problem:start -->

# [1876. Substrings of Size Three with Distinct Characters](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters)

[中文文档](/solution/1800-1899/1876.Substrings%20of%20Size%20Three%20with%20Distinct%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi được gọi là <strong>tốt</strong> nếu không có ký tự lặp lại.</p>

<p>Cho chuỗi <code>s</code>​​​​​, hãy trả về <em>số lượng <strong>chuỗi con tốt</strong> có độ dài <strong>ba </strong>trong </em><code>s</code>​​​​​​.</p>

<p>Lưu ý rằng nếu cùng một chuỗi con xuất hiện nhiều lần, mỗi lần xuất hiện đều phải được đếm.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;xyzzaz&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 4 chuỗi con độ dài 3: &quot;xyz&quot;, &quot;yzz&quot;, &quot;zza&quot; và &quot;zaz&quot;.
Chuỗi con tốt duy nhất có độ dài 3 là &quot;xyz&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aababcabc&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 7 chuỗi con độ dài 3: &quot;aab&quot;, &quot;aba&quot;, &quot;bab&quot;, &quot;abc&quot;, &quot;bca&quot;, &quot;cab&quot; và &quot;abc&quot;.
Các chuỗi con tốt là &quot;abc&quot;, &quot;bca&quot;, &quot;cab&quot; và &quot;abc&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code>​​​​​​ chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con độ dài-$3$ có các ký tự phân biệt. Kiểm tra từng bộ ba là đủ; cửa sổ trượt có thể mở rộng cho mọi $k$.
>
> Duy trì một cửa sổ không có ký tự trùng: $mask$ đánh dấu các chữ cái bên trong cửa sổ, và đầu trái tiến lên khi gặp ký tự lặp. Khi cửa sổ có độ dài ít nhất $3$, bộ ba kết thúc ở đầu phải có các ký tự phân biệt nên ta tăng kết quả lên một.

<!-- thinking:end -->

Ta có thể duy trì một cửa sổ trượt sao cho các ký tự bên trong không bị lặp. Ban đầu, ta dùng một số nguyên nhị phân $\textit{mask}$ có độ dài $26$ để biểu diễn các ký tự trong cửa sổ; bit thứ $i$ bằng $1$ cho biết ký tự $i$ đã xuất hiện trong cửa sổ, ngược lại cho biết ký tự $i$ chưa xuất hiện.

Sau đó, ta duyệt chuỗi $s$. Với mỗi vị trí $r$, nếu $\textit{s}[r]$ đã xuất hiện trong cửa sổ, ta cần dịch biên trái $l$ của cửa sổ sang phải cho đến khi cửa sổ không còn ký tự lặp. Tiếp đó, ta thêm $\textit{s}[r]$ vào cửa sổ. Lúc này, nếu độ dài cửa sổ lớn hơn hoặc bằng $3$, ta đã tìm thấy một chuỗi con tốt độ dài $3$ kết thúc tại $\textit{s}[r]$.

Sau khi duyệt xong, ta thu được số lượng tất cả chuỗi con tốt.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

> Lời giải này có thể mở rộng để tìm số lượng chuỗi con tốt có độ dài $k$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodSubstrings(self, s: str) -> int:
        ans = mask = l = 0
        for r, x in enumerate(map(lambda c: ord(c) - 97, s)):
            while mask >> x & 1:
                y = ord(s[l]) - 97
                mask ^= 1 << y
                l += 1
            mask |= 1 << x
            ans += int(r - l + 1 >= 3)
        return ans
```

#### Java

```java
class Solution {
    public int countGoodSubstrings(String s) {
        int ans = 0;
        int n = s.length();
        for (int l = 0, r = 0, mask = 0; r < n; ++r) {
            int x = s.charAt(r) - 'a';
            while ((mask >> x & 1) == 1) {
                int y = s.charAt(l++) - 'a';
                mask ^= 1 << y;
            }
            mask |= 1 << x;
            ans += r - l + 1 >= 3 ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countGoodSubstrings(string s) {
        int ans = 0;
        int n = s.length();
        for (int l = 0, r = 0, mask = 0; r < n; ++r) {
            int x = s[r] - 'a';
            while ((mask >> x & 1) == 1) {
                int y = s[l++] - 'a';
                mask ^= 1 << y;
            }
            mask |= 1 << x;
            ans += r - l + 1 >= 3 ? 1 : 0;
        }
        return ans;
    }
};
```

#### Go

```go
func countGoodSubstrings(s string) (ans int) {
	mask, l := 0, 0
	for r, c := range s {
		x := int(c - 'a')
		for (mask>>x)&1 == 1 {
			y := int(s[l] - 'a')
			l++
			mask ^= 1 << y
		}
		mask |= 1 << x
		if r-l+1 >= 3 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countGoodSubstrings(s: string): number {
    let ans = 0;
    const n = s.length;
    for (let l = 0, r = 0, mask = 0; r < n; ++r) {
        const x = s.charCodeAt(r) - 'a'.charCodeAt(0);
        while ((mask >> x) & 1) {
            const y = s.charCodeAt(l++) - 'a'.charCodeAt(0);
            mask ^= 1 << y;
        }
        mask |= 1 << x;
        ans += r - l + 1 >= 3 ? 1 : 0;
    }
    return ans;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Integer
     */
    function countGoodSubstrings($s) {
        $ans = 0;
        $n = strlen($s);
        $l = 0;
        $r = 0;
        $mask = 0;

        while ($r < $n) {
            $x = ord($s[$r]) - ord('a');
            while (($mask >> $x) & 1) {
                $y = ord($s[$l++]) - ord('a');
                $mask ^= 1 << $y;
            }
            $mask |= 1 << $x;
            if ($r - $l + 1 >= 3) {
                $ans++;
            }
            $r++;
        }

        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
