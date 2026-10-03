---
comments: true
difficulty: Hard
rating: 2661
source: Weekly Contest 248 Q4
tags:
    - Array
    - Binary Search
    - Suffix Array
    - Suffix Tree
    - Hash Function
    - Rolling Hash
    - Suffix Automato
---

<!-- problem:start -->

# [1923. Longest Common Subpath](https://leetcode.com/problems/longest-common-subpath)

[中文文档](/solution/1900-1999/1923.Longest%20Common%20Subpath/README.md)

## Mô tả

<!-- description:start -->

<p>Có một quốc gia gồm <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n - 1</code>. Ở quốc gia này, có một con đường nối <b>mọi cặp</b> thành phố.</p>

<p>Có <code>m</code> người bạn được đánh số từ <code>0</code> đến <code>m - 1</code> đang đi qua quốc gia. Mỗi người sẽ đi theo một lộ trình gồm một số thành phố. Mỗi lộ trình được biểu diễn bằng một mảng số nguyên chứa các thành phố đã đi qua theo thứ tự. Một lộ trình có thể đi qua một thành phố <strong>nhiều hơn một lần</strong>, nhưng cùng một thành phố sẽ không xuất hiện liên tiếp.</p>

<p>Cho số nguyên <code>n</code> và mảng số nguyên 2 chiều <code>paths</code>, trong đó <code>paths[i]</code> là mảng số nguyên biểu diễn lộ trình của người bạn thứ <code>i<sup>th</sup></code>, hãy trả về <em>độ dài của <strong>lộ trình con chung dài nhất</strong> xuất hiện trong lộ trình của <strong>mọi</strong> người bạn, hoặc </em><code>0</code><em> nếu không tồn tại lộ trình con chung nào</em>.</p>

<p><strong>Lộ trình con</strong> của một lộ trình là một dãy thành phố liên tiếp trong lộ trình đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, paths = [[0,1,<u>2,3</u>,4],
                       [<u>2,3</u>,4],
                       [4,0,1,<u>2,3</u>]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lộ trình con chung dài nhất là [2,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, paths = [[0],[1],[2]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có lộ trình con chung nào xuất hiện trong cả ba lộ trình.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, paths = [[<u>0</u>,1,2,3,4],
                       [4,3,2,1,<u>0</u>]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Các lộ trình con chung dài nhất có thể là [0], [1], [2], [3] và [4]. Tất cả đều có độ dài bằng 1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>m == paths.length</code></li>
	<li><code>2 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>sum(paths[i].length) &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= paths[i][j] &lt; n</code></li>
	<li>Cùng một thành phố không xuất hiện liên tiếp nhiều lần trong <code>paths[i]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lộ trình con chung càng dài thì càng khó tồn tại, nên độ dài khả thi có tính đơn điệu. Việc so sánh trực tiếp các đoạn cho mọi độ dài là quá chậm khi tổng độ dài là $10^5$.
>
> Ta tìm kiếm nhị phân $k$ và dùng rolling hash cho mọi cửa sổ có độ dài $k$ trên từng lộ trình. Nếu có một hash xuất hiện trong cả $m$ lộ trình, thì có thể tồn tại một $k$ lớn hơn.
>
> Hash tiền tố và các lũy thừa giúp mỗi lần kiểm tra có độ phức tạp gần tuyến tính, sau đó nhân với số lần tìm kiếm theo logarit.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonSubpath(self, n: int, paths: List[List[int]]) -> int:
        def check(k: int) -> bool:
            cnt = Counter()
            for h in hh:
                vis = set()
                for i in range(1, len(h) - k + 1):
                    j = i + k - 1
                    x = (h[j] - h[i - 1] * p[j - i + 1]) % mod
                    if x not in vis:
                        vis.add(x)
                        cnt[x] += 1
            return max(cnt.values()) == m

        m = len(paths)
        mx = max(len(path) for path in paths)
        base = 133331
        mod = 2**64 + 1
        p = [0] * (mx + 1)
        p[0] = 1
        for i in range(1, len(p)):
            p[i] = p[i - 1] * base % mod
        hh = []
        for path in paths:
            k = len(path)
            h = [0] * (k + 1)
            for i, x in enumerate(path, 1):
                h[i] = h[i - 1] * base % mod + x
            hh.append(h)
        l, r = 0, min(len(path) for path in paths)
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    int N = 100010;
    long[] h = new long[N];
    long[] p = new long[N];
    private int[][] paths;
    Map<Long, Integer> cnt = new HashMap<>();
    Map<Long, Integer> inner = new HashMap<>();

    public int longestCommonSubpath(int n, int[][] paths) {
        int left = 0, right = N;
        for (int[] path : paths) {
            right = Math.min(right, path.length);
        }
        this.paths = paths;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    private boolean check(int mid) {
        cnt.clear();
        inner.clear();
        p[0] = 1;
        for (int j = 0; j < paths.length; ++j) {
            int n = paths[j].length;
            for (int i = 1; i <= n; ++i) {
                p[i] = p[i - 1] * 133331;
                h[i] = h[i - 1] * 133331 + paths[j][i - 1];
            }
            for (int i = mid; i <= n; ++i) {
                long val = get(i - mid + 1, i);
                if (!inner.containsKey(val) || inner.get(val) != j) {
                    inner.put(val, j);
                    cnt.put(val, cnt.getOrDefault(val, 0) + 1);
                }
            }
        }
        int max = 0;
        for (int val : cnt.values()) {
            max = Math.max(max, val);
        }
        return max == paths.length;
    }

    private long get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
