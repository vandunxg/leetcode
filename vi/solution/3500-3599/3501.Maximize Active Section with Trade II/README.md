---
comments: true
difficulty: Hard
rating: 2940
source: Biweekly Contest 153 Q4
tags:
    - Segment Tree
    - Array
    - String
    - Binary Search
---

<!-- problem:start -->

# [3501. Maximize Active Section with Trade II](https://leetcode.com/problems/maximize-active-section-with-trade-ii)

[Tài liệu tiếng Trung](/solution/3500-3599/3501.Maximize%20Active%20Section%20with%20Trade%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> có độ dài <code>n</code>, trong đó:</p>

<ul>
    <li><code>&#39;1&#39;</code> biểu thị một section <strong>đang active</strong>.</li>
    <li><code>&#39;0&#39;</code> biểu thị một section <strong>inactive</strong>.</li>
</ul>

<p>Bạn có thể thực hiện <strong>nhiều nhất một trade</strong> để tối đa hóa số section đang active trong <code>s</code>. Trong một trade, bạn sẽ:</p>

<ul>
    <li>Chuyển một block liên tiếp gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code> thành toàn bộ <code>&#39;0&#39;</code>.</li>
    <li>Sau đó, chuyển một block liên tiếp gồm các <code>&#39;0&#39;</code> được bao quanh bởi các <code>&#39;1&#39;</code> thành toàn bộ <code>&#39;1&#39;</code>.</li>
</ul>

<p>Ngoài ra, bạn được cho một <strong>mảng 2D</strong> <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu thị <span data-keyword="substring-nonempty">substring</span> <code>s[l<sub>i</sub>...r<sub>i</sub>]</code>.</p>

<p>Với mỗi query, hãy xác định số section đang active <strong>lớn nhất</strong> có thể đạt được trong <code>s</code> sau khi thực hiện trade tối ưu trên substring <code>s[l<sub>i</sub>...r<sub>i</sub>]</code>.</p>

<p>Trả về một mảng <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của <code>queries[i]</code>.</p>

<p><strong>Lưu ý</strong></p>

<ul>
    <li>Với mỗi query, xem <code>s[l<sub>i</sub>...r<sub>i</sub>]</code> như được <strong>mở rộng</strong> thêm một <code>&#39;1&#39;</code> ở cả hai đầu, tạo thành <code>t = &#39;1&#39; + s[l<sub>i</sub>...r<sub>i</sub>] + &#39;1&#39;</code>. Các <code>&#39;1&#39;</code> được thêm vào <strong>không</strong> được tính vào kết quả cuối cùng.</li>
    <li>Các query độc lập với nhau.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01&quot;, queries = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không có block gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, không thể thực hiện trade hợp lệ nào. Số section đang active lớn nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0100&quot;, queries = [[0,3],[0,2],[1,3],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,3,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>
    <p>Query <code>[0, 3]</code> &rarr; Substring <code>&quot;0100&quot;</code> &rarr; Mở rộng thành <code>&quot;101001&quot;</code><br />
    Chọn <code>&quot;0100&quot;</code>, chuyển <code>&quot;0100&quot;</code> &rarr; <code>&quot;0000&quot;</code> &rarr; <code>&quot;1111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code>&quot;1111&quot;</code>. Số section đang active lớn nhất là 4.</p>
    </li>
    <li>
    <p>Query <code>[0, 2]</code> &rarr; Substring <code>&quot;010&quot;</code> &rarr; Mở rộng thành <code>&quot;10101&quot;</code><br />
    Chọn <code>&quot;010&quot;</code>, chuyển <code>&quot;010&quot;</code> &rarr; <code>&quot;000&quot;</code> &rarr; <code>&quot;111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code>&quot;1110&quot;</code>. Số section đang active lớn nhất là 3.</p>
    </li>
    <li>
    <p>Query <code>[1, 3]</code> &rarr; Substring <code>&quot;100&quot;</code> &rarr; Mở rộng thành <code>&quot;11001&quot;</code><br />
    Vì không có block gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, không thể thực hiện trade hợp lệ nào. Số section đang active lớn nhất là 1.</p>
    </li>
    <li>
    <p>Query <code>[2, 3]</code> &rarr; Substring <code>&quot;00&quot;</code> &rarr; Mở rộng thành <code>&quot;1001&quot;</code><br />
    Vì không có block gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, không thể thực hiện trade hợp lệ nào. Số section đang active lớn nhất là 1.</p>
    </li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1000100&quot;, queries = [[1,5],[0,6],[0,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,7,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="383" data-start="217">
    <p data-end="383" data-start="219">Query <code>[1, 5]</code> &rarr; Substring <code data-end="255" data-start="246">&quot;00010&quot;</code> &rarr; Mở rộng thành <code data-end="282" data-start="271">&quot;1000101&quot;</code><br data-end="285" data-start="282" />
    Chọn <code data-end="303" data-start="294">&quot;00010&quot;</code>, chuyển <code data-end="322" data-start="313">&quot;00010&quot;</code> &rarr; <code data-end="322" data-start="313">&quot;00000&quot;</code> &rarr; <code data-end="334" data-start="325">&quot;11111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code data-end="404" data-start="396">&quot;1111110&quot;</code>. Số section đang active lớn nhất là 6.</p>
    </li>
    <li data-end="561" data-start="385">
    <p data-end="561" data-start="387">Query <code>[0, 6]</code> &rarr; Substring <code data-end="425" data-start="414">&quot;1000100&quot;</code> &rarr; Mở rộng thành <code data-end="454" data-start="441">&quot;110001001&quot;</code><br data-end="457" data-start="454" />
    Chọn <code data-end="477" data-start="466">&quot;000100&quot;</code>, chuyển <code data-end="498" data-start="487">&quot;000100&quot;</code> &rarr; <code data-end="498" data-start="487">&quot;000000&quot;</code> &rarr; <code data-end="512" data-start="501">&quot;111111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code data-end="404" data-start="396">&quot;1111111&quot;</code>. Số section đang active lớn nhất là 7.</p>
    </li>
    <li data-end="741" data-start="563">
    <p data-end="741" data-start="565">Query <code>[0, 4]</code> &rarr; Substring <code data-end="601" data-start="592">&quot;10001&quot;</code> &rarr; Mở rộng thành <code data-end="627" data-start="617">&quot;1100011&quot;</code><br data-end="630" data-start="627" />
    Vì không có block gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, không thể thực hiện trade hợp lệ nào. Số section đang active lớn nhất là 2.</p>
    </li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01010&quot;, queries = [[0,3],[1,4],[1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,4,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>
    <p>Query <code>[0, 3]</code> &rarr; Substring <code>&quot;0101&quot;</code> &rarr; Mở rộng thành <code>&quot;101011&quot;</code><br />
    Chọn <code>&quot;010&quot;</code>, chuyển <code>&quot;010&quot;</code> &rarr; <code>&quot;000&quot;</code> &rarr; <code>&quot;111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code>&quot;11110&quot;</code>. Số section đang active lớn nhất là 4.</p>
    </li>
    <li>
    <p>Query <code>[1, 4]</code> &rarr; Substring <code>&quot;1010&quot;</code> &rarr; Mở rộng thành <code>&quot;110101&quot;</code><br />
    Chọn <code>&quot;010&quot;</code>, chuyển <code>&quot;010&quot;</code> &rarr; <code>&quot;000&quot;</code> &rarr; <code>&quot;111&quot;</code>.<br />
    Chuỗi cuối cùng không tính phần mở rộng là <code>&quot;01111&quot;</code>. Số section đang active lớn nhất là 4.</p>
    </li>
    <li>
    <p>Query <code>[1, 3]</code> &rarr; Substring <code>&quot;101&quot;</code> &rarr; Mở rộng thành <code>&quot;11011&quot;</code><br />
    Vì không có block gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, không thể thực hiện trade hợp lệ nào. Số section đang active lớn nhất là 2.</p>
    </li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
    <li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sparse Table

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt qua mọi đoạn `'0'` trong một query có độ phức tạp $\Theta(q \cdot n)$ trong trường hợp xấu nhất, trong khi cả $n$ và $q$ đều có thể bằng $10^5$. Gain ròng của một trade hợp lệ là tổng độ dài của hai đoạn `'0'` được ngăn cách bởi các `'1'`, vì vậy đáp án là tổng số `'1'` trên toàn cục cộng với gain lớn nhất trong query.
>
> Có thể truy vấn các cặp kề nhau nằm hoàn toàn trong interval trong $O(1)$ bằng sparse table trên tổng độ dài các đoạn kề nhau. Các đoạn còn lại vắt qua $l$ hoặc $r$ sẽ ghép với đoạn đầy đủ kế tiếp hoặc ghép với nhau. Sau khi lưu mỗi đoạn dưới dạng $(\textit{start},\textit{len})$, mỗi query chỉ cần một số phép so sánh hằng số.

<!-- thinking:end -->

Một trade hợp lệ chọn hai đoạn `'0'` liên tiếp được ngăn cách bởi các `'1'`, chuyển các `'1'` thành `'0'`, rồi chuyển đoạn `'0'` đã gộp trở lại thành `'1'`. Gain ròng là tổng độ dài của hai đoạn `'0'`, còn số lượng `'1'` ban đầu không đổi. Vì vậy, đáp án cho mỗi query là tổng số `'1'` trong $s$ cộng với gain lớn nhất có thể đạt được trong range của query.

Query $[l, r]$ chỉ được thao tác trên $s[l..r]$, được xem như $t = \texttt{'1'} + s[l..r] + \texttt{'1'}$. Do đó:

- Mỗi cặp đoạn `'0'` kề nhau nằm hoàn toàn trong range là một ứng viên, với gain bằng tổng độ dài của chúng;
- Nếu $s[l]$ hoặc $s[r]$ nằm giữa một đoạn `'0'`, phần suffix / prefix của đoạn đó nằm trong query range cũng có thể được ghép.

Ta lưu mỗi đoạn `'0'` dưới dạng $(\textit{start}, \textit{len})$ và xây dựng sparse table trên tổng của các đoạn kề nhau. Với mỗi query:

1. Truy vấn sparse table để tìm tổng lớn nhất của hai đoạn kề nhau nằm hoàn toàn trong range;
2. Đồng thời xét đoạn còn lại bên trái với đoạn kế tiếp, đoạn còn lại bên phải với đoạn trước đó, và trường hợp đặc biệt khi hai đoạn còn lại bao quanh một đoạn `'1'` duy nhất.

Cộng gain lớn nhất vào tổng số `'1'` trên toàn cục. Nếu không thể thực hiện trade, gain bằng $0$.

Độ phức tạp thời gian là $O(n \log n + q)$, và độ phức tạp không gian là $O(n \log n)$, trong đó $n$ là độ dài của $s$ và $q$ là số query.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxActiveSectionsAfterTrade(
        self, s: str, queries: List[List[int]]
    ) -> List[int]:
        n = len(s)
        active = s.count('1')
        if '0' not in s:
            return [active] * len(queries)

        zeros = []
        idx = [0] * n
        for i in range(n):
            if s[i] == '0':
                if i and s[i - 1] == '0':
                    zeros[-1][1] += 1
                else:
                    zeros.append([i, 1])
            idx[i] = len(zeros) - 1

        m = len(zeros) - 1
        K = m.bit_length() if m else 0
        st = [[0] * max(m, 0) for _ in range(max(K, 1))]
        for i in range(m):
            st[0][i] = zeros[i][1] + zeros[i + 1][1]
        for k in range(1, K):
            for i in range(m - (1 << k) + 1):
                st[k][i] = max(st[k - 1][i], st[k - 1][i + (1 << (k - 1))])

        def query(l: int, r: int) -> int:
            if l > r or m <= 0:
                return 0
            k = (r - l + 1).bit_length() - 1
            return max(st[k][l], st[k][r - (1 << k) + 1])

        ans = []
        for L, R in queries:
            iL, iR = idx[L], idx[R]
            cntL = -1 if iL < 0 else zeros[iL][1] - (L - zeros[iL][0])
            cntR = -1 if iR < 0 else R - zeros[iR][0] + 1
            start = iL + 1
            end = iR - (s[R] == '0')
            best = active
            if start < end:
                best = max(best, active + query(start, end - 1))
            if s[L] == '0' and s[R] == '0' and iL + 1 == iR:
                best = max(best, active + cntL + cntR)
            if s[L] == '0' and iL + 1 < iR + (s[R] == '1'):
                best = max(best, active + cntL + zeros[iL + 1][1])
            if s[R] == '0' and iL < iR - 1:
                best = max(best, active + cntR + zeros[iR - 1][1])
            ans.append(best)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> maxActiveSectionsAfterTrade(String s, int[][] queries) {
        int n = s.length();
        int active = 0;
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '1') {
                ++active;
            }
        }
        List<Integer> ans = new ArrayList<>();
        if (s.indexOf('0') < 0) {
            for (int i = 0; i < queries.length; ++i) {
                ans.add(active);
            }
            return ans;
        }

        int[][] zeros = new int[n][2];
        int z = 0;
        int[] idx = new int[n];
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '0') {
                if (i > 0 && s.charAt(i - 1) == '0') {
                    ++zeros[z - 1][1];
                } else {
                    zeros[z][0] = i;
                    zeros[z++][1] = 1;
                }
            }
            idx[i] = z - 1;
        }

        int m = z - 1;
        int K = m > 0 ? 32 - Integer.numberOfLeadingZeros(m) : 0;
        int[][] st = new int[Math.max(K, 1)][Math.max(m, 0)];
        for (int i = 0; i < m; ++i) {
            st[0][i] = zeros[i][1] + zeros[i + 1][1];
        }
        for (int k = 1; k < K; ++k) {
            for (int i = 0; i + (1 << k) <= m; ++i) {
                st[k][i] = Math.max(st[k - 1][i], st[k - 1][i + (1 << (k - 1))]);
            }
        }

        for (int[] q : queries) {
            int L = q[0], R = q[1];
            int iL = idx[L], iR = idx[R];
            int cntL = iL < 0 ? -1 : zeros[iL][1] - (L - zeros[iL][0]);
            int cntR = iR < 0 ? -1 : R - zeros[iR][0] + 1;
            int start = iL + 1;
            int end = iR - (s.charAt(R) == '0' ? 1 : 0);
            int best = active;
            if (start < end) {
                best = Math.max(best, active + query(st, m, start, end - 1));
            }
            if (s.charAt(L) == '0' && s.charAt(R) == '0' && iL + 1 == iR) {
                best = Math.max(best, active + cntL + cntR);
            }
            if (s.charAt(L) == '0' && iL + 1 < iR + (s.charAt(R) == '1' ? 1 : 0)) {
                best = Math.max(best, active + cntL + zeros[iL + 1][1]);
            }
            if (s.charAt(R) == '0' && iL < iR - 1) {
                best = Math.max(best, active + cntR + zeros[iR - 1][1]);
            }
            ans.add(best);
        }
        return ans;
    }

    private int query(int[][] st, int m, int l, int r) {
        if (l > r || m <= 0) {
            return 0;
        }
        int k = 31 - Integer.numberOfLeadingZeros(r - l + 1);
        return Math.max(st[k][l], st[k][r - (1 << k) + 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxActiveSectionsAfterTrade(string s, vector<vector<int>>& queries) {
        int n = s.size();
        int active = count(s.begin(), s.end(), '1');
        if (s.find('0') == string::npos) {
            return vector<int>(queries.size(), active);
        }

        vector<pair<int, int>> zeros;
        vector<int> idx(n);
        for (int i = 0; i < n; ++i) {
            if (s[i] == '0') {
                if (i && s[i - 1] == '0') {
                    ++zeros.back().second;
                } else {
                    zeros.emplace_back(i, 1);
                }
            }
            idx[i] = (int) zeros.size() - 1;
        }

        int m = (int) zeros.size() - 1;
        int K = m ? 32 - __builtin_clz(m) : 0;
        vector<vector<int>> st(max(K, 1), vector<int>(max(m, 0)));
        for (int i = 0; i < m; ++i) {
            st[0][i] = zeros[i].second + zeros[i + 1].second;
        }
        for (int k = 1; k < K; ++k) {
            for (int i = 0; i + (1 << k) <= m; ++i) {
                st[k][i] = max(st[k - 1][i], st[k - 1][i + (1 << (k - 1))]);
            }
        }

        auto query = [&](int l, int r) {
            if (l > r || m <= 0) {
                return 0;
            }
            int k = 31 - __builtin_clz(r - l + 1);
            return max(st[k][l], st[k][r - (1 << k) + 1]);
        };

        vector<int> ans;
        ans.reserve(queries.size());
        for (auto& q : queries) {
            int L = q[0], R = q[1];
            int iL = idx[L], iR = idx[R];
            int cntL = iL < 0 ? -1 : zeros[iL].second - (L - zeros[iL].first);
            int cntR = iR < 0 ? -1 : R - zeros[iR].first + 1;
            int start = iL + 1;
            int end = iR - (s[R] == '0');
            int best = active;
            if (start < end) {
                best = max(best, active + query(start, end - 1));
            }
            if (s[L] == '0' && s[R] == '0' && iL + 1 == iR) {
                best = max(best, active + cntL + cntR);
            }
            if (s[L] == '0' && iL + 1 < iR + (s[R] == '1')) {
                best = max(best, active + cntL + zeros[iL + 1].second);
            }
            if (s[R] == '0' && iL < iR - 1) {
                best = max(best, active + cntR + zeros[iR - 1].second);
            }
            ans.push_back(best);
        }
        return ans;
    }
};
```

#### Go

```go
func maxActiveSectionsAfterTrade(s string, queries [][]int) []int {
    n := len(s)
    active := 0
    for i := 0; i < n; i++ {
        if s[i] == '1' {
            active++
        }
    }
    if strings.IndexByte(s, '0') < 0 {
        ans := make([]int, len(queries))
        for i := range ans {
            ans[i] = active
        }
        return ans
    }

    zeros := make([][2]int, 0, n)
    idx := make([]int, n)
    for i := 0; i < n; i++ {
        if s[i] == '0' {
            if i > 0 && s[i-1] == '0' {
                zeros[len(zeros)-1][1]++
            } else {
                zeros = append(zeros, [2]int{i, 1})
            }
        }
        idx[i] = len(zeros) - 1
    }

    m := len(zeros) - 1
    K := 0
    if m > 0 {
        K = bits.Len(uint(m))
    }
    st := make([][]int, max(K, 1))
    for k := range st {
        st[k] = make([]int, max(m, 0))
    }
    for i := 0; i < m; i++ {
        st[0][i] = zeros[i][1] + zeros[i+1][1]
    }
    for k := 1; k < K; k++ {
        for i := 0; i+(1<<k) <= m; i++ {
            st[k][i] = max(st[k-1][i], st[k-1][i+(1<<(k-1))])
        }
    }

    query := func(l, r int) int {
        if l > r || m <= 0 {
            return 0
        }
        k := bits.Len(uint(r-l+1)) - 1
        return max(st[k][l], st[k][r-(1<<k)+1])
    }

    ans := make([]int, 0, len(queries))
    for _, q := range queries {
        L, R := q[0], q[1]
        iL, iR := idx[L], idx[R]
        cntL, cntR := -1, -1
        if iL >= 0 {
            cntL = zeros[iL][1] - (L - zeros[iL][0])
        }
        if iR >= 0 {
            cntR = R - zeros[iR][0] + 1
        }
        start := iL + 1
        end := iR
        if s[R] == '0' {
            end--
        }
        best := active
        if start < end {
            best = max(best, active+query(start, end-1))
        }
        if s[L] == '0' && s[R] == '0' && iL+1 == iR {
            best = max(best, active+cntL+cntR)
        }
        add := 0
        if s[R] == '1' {
            add = 1
        }
        if s[L] == '0' && iL+1 < iR+add {
            best = max(best, active+cntL+zeros[iL+1][1])
        }
        if s[R] == '0' && iL < iR-1 {
            best = max(best, active+cntR+zeros[iR-1][1])
        }
        ans = append(ans, best)
    }
    return ans
}
```

#### Rust

```rust
impl Solution {
    pub fn max_active_sections_after_trade(s: String, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let bytes = s.as_bytes();
        let length = bytes.len();
        let total_ones = bytes.iter().filter(|byte| **byte == b'1').count() as i32;
        if !bytes.contains(&b'0') {
            return vec![total_ones; queries.len()];
        }
        let mut zero_blocks: Vec<(usize, usize)> = Vec::new();
        let mut zero_block_at_position = Vec::with_capacity(length);
        for index in 0..length {
            if bytes[index] == b'0' {
                if index > 0 && bytes[index - 1] == b'0' {
                    zero_blocks.last_mut().unwrap().1 += 1;
                } else {
                    zero_blocks.push((index, 1usize));
                }
            }
            zero_block_at_position.push(zero_blocks.len() as isize - 1);
        }
        let zero_block_count = zero_blocks.len();
        let adjacent_pair_count = zero_block_count.saturating_sub(1);
        let sparse_level_count = if adjacent_pair_count == 0 {
            0
        } else {
            usize::BITS as usize - adjacent_pair_count.leading_zeros() as usize
        };
        let mut sparse_table = vec![0; adjacent_pair_count * sparse_level_count];
        for pair_index in 0..adjacent_pair_count {
            sparse_table[pair_index] =
                (zero_blocks[pair_index].1 + zero_blocks[pair_index + 1].1) as i32;
        }
        for level in 1..sparse_level_count {
            let half_span = 1usize << (level - 1);
            let span = 1usize << level;
            for start in 0..=adjacent_pair_count - span {
                sparse_table[level * adjacent_pair_count + start] = sparse_table
                    [(level - 1) * adjacent_pair_count + start]
                    .max(sparse_table[(level - 1) * adjacent_pair_count + start + half_span]);
            }
        }
        let max_pair_sum = |left_pair: usize, right_pair: usize| -> i32 {
            let right_pair = right_pair.min(adjacent_pair_count - 1);
            if left_pair > right_pair {
                return 0;
            }
            let level =
                usize::BITS as usize - (right_pair - left_pair + 1).leading_zeros() as usize - 1;
            let span = 1usize << level;
            sparse_table[level * adjacent_pair_count + left_pair]
                .max(sparse_table[level * adjacent_pair_count + right_pair - span + 1])
        };
        queries
            .into_iter()
            .map(|query| {
                let left = query[0] as usize;
                let right = query[1] as usize;
                let left_block_index = zero_block_at_position[left];
                let right_block_index = zero_block_at_position[right];
                let left_zero_count = if left_block_index == -1 {
                    -1
                } else {
                    let block_index = left_block_index as usize;
                    zero_blocks[block_index].1 as i32 - (left - zero_blocks[block_index].0) as i32
                };
                let right_zero_count = if right_block_index == -1 {
                    -1
                } else {
                    let block_index = right_block_index as usize;
                    (right - zero_blocks[block_index].0 + 1) as i32
                };
                let first_internal_pair = left_block_index + 1;
                let last_internal_pair = (if bytes[right] == b'1' {
                    right_block_index
                } else {
                    right_block_index - 1
                }) - 1;
                let last_full_zero_block = if bytes[right] == b'1' {
                    right_block_index
                } else {
                    right_block_index - 1
                };
                let mut best_total = total_ones;
                if bytes[left] == b'0'
                    && bytes[right] == b'0'
                    && left_block_index + 1 == right_block_index
                {
                    best_total = best_total.max(total_ones + left_zero_count + right_zero_count);
                } else if first_internal_pair <= last_internal_pair {
                    best_total = best_total.max(
                        total_ones
                            + max_pair_sum(
                                first_internal_pair as usize,
                                last_internal_pair as usize,
                            ),
                    );
                }
                if bytes[left] == b'0' && left_block_index + 1 <= last_full_zero_block {
                    best_total = best_total.max(
                        total_ones
                            + left_zero_count
                            + zero_blocks[(left_block_index + 1) as usize].1 as i32,
                    );
                }
                if bytes[right] == b'0' && left_block_index < right_block_index - 1 {
                    best_total = best_total.max(
                        total_ones
                            + right_zero_count
                            + zero_blocks[(right_block_index - 1) as usize].1 as i32,
                    );
                }
                best_total
            })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
