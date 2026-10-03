---
comments: true
difficulty: Medium
rating: 1604
source: Weekly Contest 326 Q3
tags:
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2522. Partition String Into Substrings With Values at Most K](https://leetcode.com/problems/partition-string-into-substrings-with-values-at-most-k)

[中文文档](/solution/2500-2599/2522.Partition%20String%20Into%20Substrings%20With%20Values%20at%20Most%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số từ <code>1</code> đến <code>9</code> và một số nguyên <code>k</code>.</p>

<p>Một phép phân hoạch chuỗi <code>s</code> được gọi là <strong>tốt</strong> nếu:</p>

<ul>
	<li>Mỗi chữ số của <code>s</code> thuộc về <strong>chính xác</strong> một chuỗi con.</li>
	<li>Giá trị của mỗi chuỗi con nhỏ hơn hoặc bằng <code>k</code>.</li>
</ul>

<p>Trả về <em><strong>số lượng</strong> chuỗi con ít nhất trong một phép phân hoạch <strong>tốt</strong> của</em> <code>s</code>. Nếu không tồn tại phép phân hoạch <strong>tốt</strong> của <code>s</code>, trả về <code>-1</code>.</p>

<p><b>Lưu ý</b> rằng:</p>

<ul>
	<li><strong>Giá trị</strong> của một chuỗi là kết quả khi coi chuỗi đó là một số nguyên. Ví dụ, giá trị của <code>&quot;123&quot;</code> là <code>123</code> và giá trị của <code>&quot;1&quot;</code> là <code>1</code>.</li>
	<li>Một <strong>chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;165462&quot;, k = 60
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể phân hoạch chuỗi thành các chuỗi con &quot;16&quot;, &quot;54&quot;, &quot;6&quot; và &quot;2&quot;. Giá trị của mỗi chuỗi con đều nhỏ hơn hoặc bằng k = 60.
Có thể chứng minh rằng không thể phân hoạch chuỗi thành ít hơn 4 chuỗi con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;238182&quot;, k = 5
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không tồn tại phép phân hoạch tốt cho chuỗi này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là một chữ số từ <code>&#39;1&#39;</code> đến <code>&#39;9&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0;
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: margin 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Chia chuỗi chữ số thành các chuỗi con có giá trị nguyên không vượt quá $k$, với số mảnh ít nhất có thể. Không thể xét toàn bộ các phép phân hoạch khi $n\le 10^5$, nhưng một mảnh hợp lệ có độ dài nhiều nhất bằng số chữ số của $k$, vì vậy từ mỗi vị trí bắt đầu ta chỉ cần thử một số lượng cách không đổi.
>
> Gọi $\textit{dfs}(i)$ là số mảnh ít nhất cần dùng từ chỉ số $i$. Ta cộng dồn giá trị về phía bên phải, dừng lại khi giá trị vượt quá $k$, rồi lấy $1+\textit{dfs}(j+1)$ trên mọi điểm kết thúc hợp lệ. Memoization tính mỗi vị trí bắt đầu đúng một lần; trường hợp không thể phân hoạch được biểu diễn bằng $\infty$, sau đó trả về $-1$.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$ biểu diễn số lượng phân hoạch ít nhất bắt đầu từ chỉ số $i$ của chuỗi $s$. Đáp án là $dfs(0)$.

Quá trình tính hàm $dfs(i)$ như sau:

- Nếu $i \geq n$, nghĩa là đã đến cuối chuỗi, trả về $0$.
- Nếu không, ta liệt kê tất cả các chuỗi con bắt đầu từ $i$. Nếu giá trị của chuỗi con nhỏ hơn hoặc bằng $k$, ta có thể chọn chuỗi con đó làm một phần của phép phân hoạch. Khi đó, ta nhận được $dfs(j + 1)$, trong đó $j$ là chỉ số kết thúc của chuỗi con. Ta lấy giá trị nhỏ nhất trong tất cả các phép phân hoạch có thể, cộng thêm $1$, và đó là giá trị của $dfs(i)$.

Cuối cùng, nếu $dfs(0) = \infty$, nghĩa là không tồn tại phép phân hoạch tốt, ta trả về $-1$. Ngược lại, trả về $dfs(0)$.

Để tránh tính toán lặp lại, ta có thể sử dụng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPartition(self, s: str, k: int) -> int:
        @cache
        def dfs(i):
            if i >= n:
                return 0
            res, v = inf, 0
            for j in range(i, n):
                v = v * 10 + int(s[j])
                if v > k:
                    break
                res = min(res, dfs(j + 1))
            return res + 1

        n = len(s)
        ans = dfs(0)
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int n;
    private String s;
    private int k;
    private int inf = 1 << 30;

    public int minimumPartition(String s, int k) {
        n = s.length();
        f = new Integer[n];
        this.s = s;
        this.k = k;
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int res = inf;
        long v = 0;
        for (int j = i; j < n; ++j) {
            v = v * 10 + (s.charAt(j) - '0');
            if (v > k) {
                break;
            }
            res = Math.min(res, dfs(j + 1));
        }
        return f[i] = res + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPartition(string s, int k) {
        int n = s.size();
        int f[n];
        memset(f, 0, sizeof f);
        const int inf = 1 << 30;
        function<int(int)> dfs = [&](int i) -> int {
            if (i >= n) return 0;
            if (f[i]) return f[i];
            int res = inf;
            long v = 0;
            for (int j = i; j < n; ++j) {
                v = v * 10 + (s[j] - '0');
                if (v > k) break;
                res = min(res, dfs(j + 1));
            }
            return f[i] = res + 1;
        };
        int ans = dfs(0);
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
func minimumPartition(s string, k int) int {
	n := len(s)
	f := make([]int, n)
	const inf int = 1 << 30
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		res, v := inf, 0
		for j := i; j < n; j++ {
			v = v*10 + int(s[j]-'0')
			if v > k {
				break
			}
			res = min(res, dfs(j+1))
		}
		f[i] = res + 1
		return f[i]
	}
	ans := dfs(0)
	if ans < inf {
		return ans
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
