---
comments: true
difficulty: Hard
rating: 2803
source: Weekly Contest 265 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2060. Check if an Original String Exists Given Two Encoded Strings](https://leetcode.com/problems/check-if-an-original-string-exists-given-two-encoded-strings)

[中文文档](/solution/2000-2099/2060.Check%20if%20an%20Original%20String%20Exists%20Given%20Two%20Encoded%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi gốc, chỉ gồm các chữ cái tiếng Anh viết thường, có thể được mã hóa theo các bước sau:</p>

<ul>
	<li>Tùy ý <strong>chia</strong> chuỗi đó thành một <strong>chuỗi</strong> gồm một số lượng tùy ý các chuỗi con <strong>không rỗng</strong>.</li>
	<li>Tùy ý chọn một số phần tử (có thể không chọn phần tử nào) trong chuỗi trên, rồi <strong>thay thế</strong> mỗi phần tử bằng <strong>độ dài của nó</strong> (dưới dạng chuỗi số).</li>
	<li><strong>Nối</strong> các phần tử trong chuỗi để tạo thành chuỗi đã mã hóa.</li>
</ul>

<p>Ví dụ, một <strong>cách</strong> mã hóa chuỗi gốc <code>&quot;abcdefghijklmnop&quot;</code> là:</p>

<ul>
	<li>Chia thành chuỗi: <code>[&quot;ab&quot;, &quot;cdefghijklmn&quot;, &quot;o&quot;, &quot;p&quot;]</code>.</li>
	<li>Chọn phần tử thứ hai và thứ ba để thay thế lần lượt bằng độ dài của chúng. Chuỗi trở thành <code>[&quot;ab&quot;, &quot;12&quot;, &quot;1&quot;, &quot;p&quot;]</code>.</li>
	<li>Nối các phần tử trong chuỗi để nhận được chuỗi đã mã hóa: <code>&quot;ab121p&quot;</code>.</li>
</ul>

<p>Cho hai chuỗi đã mã hóa <code>s1</code> và <code>s2</code>, chỉ gồm các chữ cái tiếng Anh viết thường và các chữ số <code>1-9</code> (bao gồm cả hai đầu), hãy trả về <code>true</code><em> nếu tồn tại một chuỗi gốc có thể được mã hóa thành <strong>cả hai</strong> </em><code>s1</code><em> và </em><code>s2</code><em>. Nếu không, trả về </em><code>false</code>.</p>

<p><strong>Lưu ý</strong>: Các test được tạo sao cho số chữ số liên tiếp trong <code>s1</code> và <code>s2</code> không vượt quá <code>3</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;internationalization&quot;, s2 = &quot;i18n&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chuỗi gốc là &quot;internationalization&quot;.
- &quot;internationalization&quot;
  -&gt; Chia:       [&quot;internationalization&quot;]
  -&gt; Không thay thế phần tử nào
  -&gt; Nối:         &quot;internationalization&quot;, chính là s1.
- &quot;internationalization&quot;
  -&gt; Chia:       [&quot;i&quot;, &quot;nternationalizatio&quot;, &quot;n&quot;]
  -&gt; Thay thế:   [&quot;i&quot;, &quot;18&quot;,                 &quot;n&quot;]
  -&gt; Nối:         &quot;i18n&quot;, chính là s2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;l123e&quot;, s2 = &quot;44&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chuỗi gốc là &quot;leetcode&quot;.
- &quot;leetcode&quot;
  -&gt; Chia:       [&quot;l&quot;, &quot;e&quot;, &quot;et&quot;, &quot;cod&quot;, &quot;e&quot;]
  -&gt; Thay thế:   [&quot;l&quot;, &quot;1&quot;, &quot;2&quot;,  &quot;3&quot;,   &quot;e&quot;]
  -&gt; Nối:         &quot;l123e&quot;, chính là s1.
- &quot;leetcode&quot;
  -&gt; Chia:       [&quot;leet&quot;, &quot;code&quot;]
  -&gt; Thay thế:   [&quot;4&quot;,    &quot;4&quot;]
  -&gt; Nối:         &quot;44&quot;, chính là s2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;a5b&quot;, s2 = &quot;c5b&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể thực hiện được.
- Chuỗi gốc được mã hóa thành s1 phải bắt đầu bằng chữ cái &#39;a&#39;.
- Chuỗi gốc được mã hóa thành s2 phải bắt đầu bằng chữ cái &#39;c&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 40</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ số <code>1-9</code> (bao gồm cả hai đầu) và các chữ cái tiếng Anh viết thường.</li>
	<li>Số chữ số liên tiếp trong <code>s1</code> và <code>s2</code> không vượt quá <code>3</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi đã mã hóa có độ dài $\le 40$ và các đoạn gồm nhiều nhất ba chữ số, mỗi đoạn đại diện cho một khối ký tự đại diện. Việc giải mã mọi chuỗi gốc là bất khả thi. Trạng thái căn chỉnh gồm một cặp chỉ số và một độ lệch độ dài: các chữ cái làm thay đổi độ lệch, còn các chữ số cộng hoặc trừ độ dài của một khối.
>
> Một đoạn chữ số có thể được tách thành nhiều độ dài thực tế, nên DFS phải thử các khả năng. Độ lệch có giới hạn, vì vậy ta ghi nhớ các trạng thái.
>
> Các tab code đang trống; phần lập luận dựa trên phép tìm kiếm được dẫn dắt bởi độ lệch này.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function possiblyEquals(s1: string, s2: string): boolean {
    const n = s1.length,
        m = s2.length;
    let dp: Array<Array<Set<number>>> = Array.from({ length: n + 1 }, v =>
        Array.from({ length: m + 1 }, w => new Set()),
    );
    dp[0][0].add(0);

    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= m; j++) {
            for (let delta of dp[i][j]) {
                // s1为数字
                let num = 0;
                if (delta <= 0) {
                    for (let p = i; i < Math.min(i + 3, n); p++) {
                        if (isDigit(s1[p])) {
                            num = num * 10 + Number(s1[p]);
                            dp[p + 1][j].add(delta + num);
                        } else {
                            break;
                        }
                    }
                }

                // s2为数字
                num = 0;
                if (delta >= 0) {
                    for (let q = j; q < Math.min(j + 3, m); q++) {
                        if (isDigit(s2[q])) {
                            num = num * 10 + Number(s2[q]);
                            dp[i][q + 1].add(delta - num);
                        } else {
                            break;
                        }
                    }
                }

                // 数字匹配s1为字母
                if (i < n && delta < 0 && !isDigit(s1[i])) {
                    dp[i + 1][j].add(delta + 1);
                }

                // 数字匹配s2为字母
                if (j < m && delta > 0 && !isDigit(s2[j])) {
                    dp[i][j + 1].add(delta - 1);
                }

                // 两个字母匹配
                if (i < n && j < m && delta == 0 && s1[i] == s2[j]) {
                    dp[i + 1][j + 1].add(0);
                }
            }
        }
    }
    return dp[n][m].has(0);
}

function isDigit(char: string): boolean {
    return /^\d{1}$/g.test(char);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
