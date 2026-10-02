---
comments: true
difficulty: Easy
tags:
    - Math
    - String
    - Enumeration
---

<!-- problem:start -->

# [800. Similar RGB Color 🔒](https://leetcode.com/problems/similar-rgb-color)

[中文文档](/solution/0800-0899/0800.Similar%20RGB%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Màu red-green-blue <code>&quot;#AABBCC&quot;</code> có thể được viết ở dạng rút gọn là <code>&quot;#ABC&quot;</code>.</p>

<ul>
	<li>Ví dụ, <code>&quot;#15c&quot;</code> là dạng rút gọn của màu <code>&quot;#1155cc&quot;</code>.</li>
</ul>

<p>Độ tương đồng giữa hai màu <code>&quot;#ABCDEF&quot;</code> và <code>&quot;#UVWXYZ&quot;</code> được tính bằng <code>-(AB - UV)<sup>2</sup> - (CD - WX)<sup>2</sup> - (EF - YZ)<sup>2</sup></code>.</p>

<p>Cho chuỗi <code>color</code> có định dạng <code>&quot;#ABCDEF&quot;</code>, hãy trả về chuỗi biểu diễn màu có độ tương đồng cao nhất với màu đã cho và có dạng rút gọn (tức có thể biểu diễn dưới dạng <code>&quot;#XYZ&quot;</code>).</p>

<p><strong>Bất kỳ đáp án nào</strong> có độ tương đồng cao nhất bằng với đáp án tốt nhất đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> color = &quot;#09f166&quot;
<strong>Đầu ra:</strong> &quot;#11ee66&quot;
<strong>Giải thích:</strong> 
Độ tương đồng là -(0x09 - 0x11)<sup>2</sup> -(0xf1 - 0xee)<sup>2</sup> - (0x66 - 0x66)<sup>2</sup> = -64 -9 -0 = -73.
Đây là giá trị cao nhất trong số các màu dạng rút gọn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> color = &quot;#4e3fe1&quot;
<strong>Đầu ra:</strong> &quot;#5544dd&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>color.length == 7</code></li>
	<li><code>color[0] == &#39;#&#39;</code></li>
	<li>Với <code>i &gt; 0</code>, <code>color[i]</code> là chữ số hoặc ký tự trong phạm vi <code>[&#39;a&#39;, &#39;f&#39;]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi kênh màu trong dạng rút gọn chỉ có thể nhận các giá trị $00,11,\ldots,ff$, tức là bội số của $17$. Độ tương đồng là tổng bình phương độ chênh lệch giữa các kênh; ba kênh độc lập với nhau nên không cần duyệt hết $16^3$ màu dạng rút gọn.
>
> Viết một kênh $q$ dưới dạng $17y+z$. Nếu số dư $z>8$, làm tròn lên bội số tiếp theo; nếu không thì giữ bội số hiện tại. Áp dụng riêng cho cả ba kênh sẽ thu được màu dạng rút gọn gần nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def similarRGB(self, color: str) -> str:
        def f(x):
            y, z = divmod(int(x, 16), 17)
            if z > 8:
                y += 1
            return '{:02x}'.format(17 * y)

        a, b, c = color[1:3], color[3:5], color[5:7]
        return f'#{f(a)}{f(b)}{f(c)}'
```

#### Java

```java
class Solution {
    public String similarRGB(String color) {
        String a = color.substring(1, 3), b = color.substring(3, 5), c = color.substring(5, 7);
        return "#" + f(a) + f(b) + f(c);
    }

    private String f(String x) {
        int q = Integer.parseInt(x, 16);
        q = q / 17 + (q % 17 > 8 ? 1 : 0);
        return String.format("%02x", 17 * q);
    }
}
```

#### Go

```go
func similarRGB(color string) string {
	f := func(x string) string {
		q, _ := strconv.ParseInt(x, 16, 64)
		if q%17 > 8 {
			q = q/17 + 1
		} else {
			q = q / 17
		}
		return fmt.Sprintf("%02x", 17*q)

	}
	a, b, c := color[1:3], color[3:5], color[5:7]
	return "#" + f(a) + f(b) + f(c)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
