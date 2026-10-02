---
comments: true
difficulty: Hard
rating: 2575
source: Weekly Contest 199 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1531. String Compression II](https://leetcode.com/problems/string-compression-ii)

[中文文档](/solution/1500-1599/1531.String%20Compression%20II/README.md)

## Mô tả

<!-- description:start -->

<p><a href="http://en.wikipedia.org/wiki/Run-length_encoding">Mã hóa run-length</a> là một phương pháp nén chuỗi, thay thế các ký tự giống nhau liên tiếp (lặp lại từ 2 lần trở lên) bằng phép nối ký tự với số lượng ký tự trong đoạn. Ví dụ, để nén chuỗi&nbsp;<code>&quot;aabccc&quot;</code>, ta thay <font face="monospace"><code>&quot;aa&quot;</code></font>&nbsp;bằng&nbsp;<font face="monospace"><code>&quot;a2&quot;</code></font> và thay <font face="monospace"><code>&quot;ccc&quot;</code></font>&nbsp;bằng&nbsp;<font face="monospace"><code>&quot;c3&quot;</code></font>. Khi đó chuỗi nén trở thành <font face="monospace"><code>&quot;a2bc3&quot;</code>.</font></p>

<p>Lưu ý rằng trong bài toán này, ta không thêm&nbsp;<code>&#39;1&#39;</code>&nbsp;sau các ký tự đơn.</p>

<p>Cho một&nbsp;chuỗi <code>s</code>&nbsp;và một số nguyên <code>k</code>. Hãy xóa <strong>tối đa</strong>&nbsp;<code>k</code> ký tự khỏi&nbsp;<code>s</code>&nbsp;sao cho phiên bản mã hóa run-length của <code>s</code>&nbsp;có độ dài nhỏ nhất.</p>

<p>Tìm <em>độ dài nhỏ nhất của phiên bản mã hóa run-length của </em><code>s</code><em> sau khi xóa tối đa </em><code>k</code><em> ký tự</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabcccd&quot;, k = 2
<strong>Đầu ra:</strong> 4
<b>Giải thích: </b>Nén s mà không xóa ký tự nào sẽ cho &quot;a3bc3d&quot; có độ dài 6. Xóa các ký tự &#39;a&#39; hoặc &#39;c&#39; chỉ có thể giảm độ dài chuỗi nén xuống 5, chẳng hạn xóa 2 ký tự &#39;a&#39; thì s = &quot;abcccd&quot; và chuỗi nén là abc3d. Vì vậy, cách tối ưu là xóa &#39;b&#39; và &#39;d&#39;, khi đó chuỗi nén là &quot;a3c3&quot; có độ dài 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabbaa&quot;, k = 2
<strong>Đầu ra:</strong> 2
<b>Giải thích: </b>Nếu xóa cả hai ký tự &#39;b&#39;, chuỗi nén thu được sẽ là &quot;a4&quot; có độ dài 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaaaaaaaaa&quot;, k = 0
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Vì k bằng 0 nên ta không thể xóa gì. Chuỗi nén là &quot;a11&quot; có độ dài 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>0 &lt;= k &lt;= s.length</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sau tối đa $k$ lần xóa, ta muốn mã hóa run-length ngắn nhất có thể. Cả $n$ và $k$ đều không vượt quá $100$, nên trạng thái “bắt đầu tại $i$ với $k$ lần xóa còn lại” có thể ghi nhớ, nhưng ta không thể liệt kê mọi tập con ký tự bị xóa.
>
> Một block được mã hóa là $s[i..j]$ bị buộc thành một ký tự: giữ lại ký tự xuất hiện nhiều nhất và xóa phần còn lại, tức $j-i+1-maxFreq$ ký tự. Độ dài block phụ thuộc vào tần suất đó. Thử mọi điểm kết thúc phải $j$ rồi cộng thêm $compression(j+1, k')$. Nếu hết lượt xóa, hoặc suffix không dài hơn $k$, trả về giá trị sentinel tương ứng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int getLengthOfOptimalCompression(String s, int k) {
        // dp[i][k] := the length of the optimal compression of s[i..n) with at most
        // k deletion
        dp = new int[s.length()][k + 1];
        Arrays.stream(dp).forEach(A -> Arrays.fill(A, K_MAX));
        return compression(s, 0, k);
    }

    private static final int K_MAX = 101;
    private int[][] dp;

    private int compression(final String s, int i, int k) {
        if (k < 0) {
            return K_MAX;
        }
        if (i == s.length() || s.length() - i <= k) {
            return 0;
        }
        if (dp[i][k] != K_MAX) {
            return dp[i][k];
        }
        int maxFreq = 0;
        int[] count = new int[128];
        // Make letters in s[i..j] be the same.
        // Keep the letter that has the maximum frequency in this range and remove
        // the other letters.
        for (int j = i; j < s.length(); ++j) {
            maxFreq = Math.max(maxFreq, ++count[s.charAt(j)]);
            dp[i][k] = Math.min(
                dp[i][k], getLength(maxFreq) + compression(s, j + 1, k - (j - i + 1 - maxFreq)));
        }
        return dp[i][k];
    }

    // Returns the length to compress `maxFreq`.
    private int getLength(int maxFreq) {
        if (maxFreq == 1) {
            return 1; // c
        }
        if (maxFreq < 10) {
            return 2; // [1-9]c
        }
        if (maxFreq < 100) {
            return 3; // [1-9][0-9]c
        }
        return 4; // [1-9][0-9][0-9]c
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
