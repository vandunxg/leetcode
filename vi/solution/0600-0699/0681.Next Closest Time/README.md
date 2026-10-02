---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [681. Next Closest Time 🔒](https://leetcode.com/problems/next-closest-time)

[中文文档](/solution/0600-0699/0681.Next%20Closest%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>time</code> ở định dạng <code>&quot;HH:MM&quot;</code>, hãy tạo thời điểm gần nhất tiếp theo bằng cách dùng lại các chữ số hiện có. Có thể dùng lại một chữ số tùy ý số lần.</p>

<p>Có thể giả sử chuỗi đầu vào luôn là thời gian hợp lệ. Ví dụ, <code>&quot;01:34&quot;</code> và <code>&quot;12:09&quot;</code> đều hợp lệ, còn <code>&quot;1:34&quot;</code> và <code>&quot;12:9&quot;</code> đều không hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = &quot;19:34&quot;
<strong>Đầu ra:</strong> &quot;19:39&quot;
<strong>Giải thích:</strong> Thời điểm gần nhất tiếp theo tạo từ các chữ số <strong>1</strong>, <strong>9</strong>, <strong>3</strong>, <strong>4</strong> là <strong>19:39</strong>, tức là sau 5 phút.
Không chọn <strong>19:33</strong> vì thời điểm đó cách hiện tại 23 giờ 59 phút.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = &quot;23:59&quot;
<strong>Đầu ra:</strong> &quot;22:22&quot;
<strong>Giải thích:</strong> Thời điểm gần nhất tiếp theo tạo từ các chữ số <strong>2</strong>, <strong>3</strong>, <strong>5</strong>, <strong>9</strong> là <strong>22:22</strong>.
Có thể xem thời gian trả về thuộc ngày hôm sau vì giá trị số của nó nhỏ hơn thời gian đầu vào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>time.length == 5</code></li>
	<li><code>time</code> là thời gian hợp lệ theo định dạng <code>&quot;HH:MM&quot;</code>.</li>
	<li><code>0 &lt;= HH &lt; 24</code></li>
	<li><code>0 &lt;= MM &lt; 60</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dùng lại các chữ số của thời gian hiện tại để tạo thời điểm hợp lệ tiếp theo, kể cả khi phải vòng qua nửa đêm. Chỉ có $4^4$ ứng viên.
>
> Dùng DFS để tạo bốn chữ số, chỉ nhận giờ và phút hợp lệ, đồng thời giữ thời gian nhỏ nhất nhưng lớn hơn thời điểm hiện tại. Nếu không có ứng viên nào, lặp lại chữ số nhỏ nhất để tạo thời gian của ngày hôm sau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextClosestTime(self, time: str) -> str:
        def check(t):
            h, m = int(t[:2]), int(t[2:])
            return 0 <= h < 24 and 0 <= m < 60

        def dfs(curr):
            if len(curr) == 4:
                if not check(curr):
                    return
                nonlocal ans, d
                p = int(curr[:2]) * 60 + int(curr[2:])
                if t < p < t + d:
                    d = p - t
                    ans = curr[:2] + ':' + curr[2:]
                return
            for c in s:
                dfs(curr + c)

        s = {c for c in time if c != ':'}
        t = int(time[:2]) * 60 + int(time[3:])
        d = inf
        ans = None
        dfs('')
        if ans is None:
            mi = min(int(c) for c in s)
            ans = f'{mi}{mi}:{mi}{mi}'
        return ans
```

#### Java

```java
class Solution {
    private int t;
    private int d;
    private String ans;
    private Set<Character> s;

    public String nextClosestTime(String time) {
        t = Integer.parseInt(time.substring(0, 2)) * 60 + Integer.parseInt(time.substring(3));
        d = Integer.MAX_VALUE;
        s = new HashSet<>();
        char mi = 'z';
        for (char c : time.toCharArray()) {
            if (c != ':') {
                s.add(c);
                if (c < mi) {
                    mi = c;
                }
            }
        }
        ans = null;
        dfs("");
        if (ans == null) {
            ans = "" + mi + mi + ":" + mi + mi;
        }
        return ans;
    }

    private void dfs(String curr) {
        if (curr.length() == 4) {
            if (!check(curr)) {
                return;
            }
            int p
                = Integer.parseInt(curr.substring(0, 2)) * 60 + Integer.parseInt(curr.substring(2));
            if (p > t && p - t < d) {
                d = p - t;
                ans = curr.substring(0, 2) + ":" + curr.substring(2);
            }
            return;
        }
        for (char c : s) {
            dfs(curr + c);
        }
    }

    private boolean check(String t) {
        int h = Integer.parseInt(t.substring(0, 2));
        int m = Integer.parseInt(t.substring(2));
        return 0 <= h && h < 24 && 0 <= m && m < 60;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
