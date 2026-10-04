---
comments: true
difficulty: Medium
rating: 1959
source: Weekly Contest 332 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2564. Substring XOR Queries](https://leetcode.com/problems/substring-xor-queries)

[中文文档](/solution/2500-2599/2564.Substring%20XOR%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <strong>chuỗi nhị phân</strong> <code>s</code> và một mảng số nguyên <strong>2D</strong> <code>queries</code>, trong đó <code>queries[i] = [first<sub>i</sub>, second<sub>i</sub>]</code>.</p>

<p>Với truy vấn <code>i<sup>th</sup></code>, hãy tìm <strong>chuỗi con ngắn nhất</strong> của <code>s</code> có <strong>giá trị thập phân</strong> là <code>val</code>, sao cho khi thực hiện <strong>phép XOR theo bit</strong> với <code>first<sub>i</sub></code> thì thu được <code>second<sub>i</sub></code>. Nói cách khác, <code>val ^ first<sub>i</sub> == second<sub>i</sub></code>.</p>

<p>Đáp án của truy vấn <code>i<sup>th</sup></code> là hai đầu mút (<strong>đánh chỉ số từ 0</strong>) của chuỗi con <code>[left<sub>i</sub>, right<sub>i</sub>]</code>, hoặc <code>[-1, -1]</code> nếu không tồn tại chuỗi con phù hợp. Nếu có nhiều đáp án, hãy chọn đáp án có <code>left<sub>i</sub></code> <strong>nhỏ nhất</strong>.</p>

<p><em>Trả về một mảng</em> <code>ans</code> <em>trong đó </em><code>ans[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> <em>là đáp án của truy vấn </em><code>i<sup>th</sup></code><em>.</em></p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp, không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;101101&quot;, queries = [[0,5],[1,2]]
<strong>Đầu ra:</strong> [[0,2],[2,3]]
<strong>Giải thích:</strong> Với truy vấn đầu tiên, chuỗi con trong phạm vi <code>[0,2]</code> là <strong>&quot;101&quot;</strong>, có giá trị thập phân là <strong><code>5</code></strong>, và <strong><code>5 ^ 0 = 5</code></strong>, vì vậy đáp án của truy vấn đầu tiên là <code>[0,2]</code>. Với truy vấn thứ hai, chuỗi con trong phạm vi <code>[2,3]</code> là <strong>&quot;11&quot;,</strong> có giá trị thập phân là <strong>3</strong>, và <strong>3<code> ^ 1 = 2</code></strong>.&nbsp;Do đó, ta trả về <code>[2,3]</code> cho truy vấn thứ hai.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0101&quot;, queries = [[12,8]]
<strong>Đầu ra:</strong> [[-1,-1]]
<strong>Giải thích:</strong> Trong ví dụ này, không có chuỗi con nào thỏa mãn truy vấn, vì vậy <code>[-1,-1] is returned</code>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1&quot;, queries = [[4,5]]
<strong>Đầu ra:</strong> [[0,0]]
<strong>Giải thích:</strong> Trong ví dụ này, chuỗi con trong phạm vi <code>[0,0]</code> có giá trị thập phân là <strong><code>1</code></strong>, và <strong><code>1 ^ 4 = 5</code></strong>. Vì vậy, đáp án là <code>[0,0]</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= first<sub>i</sub>, second<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn cần chuỗi con ngắn nhất (nếu có nhiều chuỗi, chọn chuỗi có vị trí bắt đầu nhỏ nhất) có giá trị bằng $first\oplus second$. Có $10^5$ truy vấn và các giá trị là số $32$-bit, nên việc duyệt chuỗi cho từng truy vấn sẽ quá chậm.
>
> Mọi số nguyên $32$-bit đều xuất hiện dưới dạng một chuỗi con có độ dài nhiều nhất là $32$. Với mỗi vị trí bắt đầu, ta mở rộng tối đa $32$ bit và ghi lại lần xuất hiện đầu tiên của mỗi giá trị. Dừng khi giá trị thu được bằng 0 do gặp các số 0 ở đầu, vì cách biểu diễn dài hơn không thể cho một đáp án ngắn hơn. Khi đó, các truy vấn trở thành những lần tra cứu trong hash table.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý tất cả chuỗi con có độ dài từ $1$ đến $32$ thành các giá trị thập phân tương ứng, tìm chỉ số nhỏ nhất và chỉ số ở đầu mút phải tương ứng của mỗi giá trị, rồi lưu chúng vào hash table $d$.

Sau đó, ta liệt kê từng truy vấn. Với mỗi truy vấn $[first, second]$, ta chỉ cần kiểm tra trong hash table $d$ xem có cặp khóa-giá trị nào có khóa là $first \oplus second$ hay không. Nếu có, ta thêm chỉ số nhỏ nhất và chỉ số ở đầu mút phải tương ứng vào mảng đáp án. Nếu không, ta thêm $[-1, -1]$.

Độ phức tạp thời gian là $O(n \times \log M + m)$, và độ phức tạp không gian là $O(n \times \log M)$. Trong đó, $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và mảng truy vấn $queries$, còn $M$ có thể nhận giá trị lớn nhất của một số nguyên là $2^{31} - 1$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def substringXorQueries(self, s: str, queries: List[List[int]]) -> List[List[int]]:
        d = {}
        n = len(s)
        for i in range(n):
            x = 0
            for j in range(32):
                if i + j >= n:
                    break
                x = x << 1 | int(s[i + j])
                if x not in d:
                    d[x] = [i, i + j]
                if x == 0:
                    break
        return [d.get(first ^ second, [-1, -1]) for first, second in queries]
```

#### Java

```java
class Solution {
    public int[][] substringXorQueries(String s, int[][] queries) {
        Map<Integer, int[]> d = new HashMap<>();
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int x = 0;
            for (int j = 0; j < 32 && i + j < n; ++j) {
                x = x << 1 | (s.charAt(i + j) - '0');
                d.putIfAbsent(x, new int[] {i, i + j});
                if (x == 0) {
                    break;
                }
            }
        }
        int m = queries.length;
        int[][] ans = new int[m][2];
        for (int i = 0; i < m; ++i) {
            int first = queries[i][0], second = queries[i][1];
            int val = first ^ second;
            ans[i] = d.getOrDefault(val, new int[] {-1, -1});
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> substringXorQueries(string s, vector<vector<int>>& queries) {
        unordered_map<int, vector<int>> d;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            int x = 0;
            for (int j = 0; j < 32 && i + j < n; ++j) {
                x = x << 1 | (s[i + j] - '0');
                if (!d.count(x)) {
                    d[x] = {i, i + j};
                }
                if (x == 0) {
                    break;
                }
            }
        }
        vector<vector<int>> ans;
        for (auto& q : queries) {
            int first = q[0], second = q[1];
            int val = first ^ second;
            if (d.count(val)) {
                ans.emplace_back(d[val]);
            } else {
                ans.push_back({-1, -1});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func substringXorQueries(s string, queries [][]int) (ans [][]int) {
	d := map[int][]int{}
	for i := range s {
		x := 0
		for j := 0; j < 32 && i+j < len(s); j++ {
			x = x<<1 | int(s[i+j]-'0')
			if _, ok := d[x]; !ok {
				d[x] = []int{i, i + j}
			}
			if x == 0 {
				break
			}
		}
	}
	for _, q := range queries {
		first, second := q[0], q[1]
		val := first ^ second
		if v, ok := d[val]; ok {
			ans = append(ans, v)
		} else {
			ans = append(ans, []int{-1, -1})
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
