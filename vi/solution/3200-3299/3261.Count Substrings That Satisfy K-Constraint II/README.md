---
comments: true
difficulty: Hard
rating: 2658
source: Weekly Contest 411 Q4
tags:
    - Array
    - String
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3261. Count Substrings That Satisfy K-Constraint II](https://leetcode.com/problems/count-substrings-that-satisfy-k-constraint-ii)

[中文文档](/solution/3200-3299/3261.Count%20Substrings%20That%20Satisfy%20K-Constraint%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>nhị phân</strong> <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Đồng thời, cho một mảng số nguyên hai chiều <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Một <strong>chuỗi nhị phân</strong> thỏa mãn <strong>ràng buộc k</strong> nếu <strong>một trong hai</strong> điều kiện sau đúng:</p>

<ul>
	<li>Số lượng ký tự <code>0</code> trong chuỗi không vượt quá <code>k</code>.</li>
	<li>Số lượng ký tự <code>1</code> trong chuỗi không vượt quá <code>k</code>.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code>, trong đó <code>answer[i]</code> là số <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s[l<sub>i</sub>..r<sub>i</sub>]</code> thỏa mãn <strong>ràng buộc k</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0001111&quot;, k = 2, queries = [[0,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[26]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với truy vấn <code>[0, 6]</code>, tất cả chuỗi con của <code>s[0..6] = &quot;0001111&quot;</code> đều thỏa mãn ràng buộc k, ngoại trừ các chuỗi con <code>s[0..5] = &quot;000111&quot;</code> và <code>s[0..6] = &quot;0001111&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;010101&quot;, k = 1, queries = [[0,5],[1,4],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[15,9,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con của <code>s</code> có độ dài lớn hơn 3 không thỏa mãn ràng buộc k.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i] == [l<sub>i</sub>, r<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; s.length</code></li>
	<li>Tất cả truy vấn đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Một lần đếm theo ràng buộc $k$ có thể thực hiện bằng cách trượt tuyến tính như ở bài I, nhưng ở đây có $10^5$ truy vấn trên một chuỗi có độ dài $10^5$. Với mỗi điểm bắt đầu, các điểm kết thúc hợp lệ tạo thành một đoạn đầu; phép trượt lưu lại chỉ số không hợp lệ đầu tiên $d[i]$.
>
> $\textit{pre}[j]$ là số chuỗi con hợp lệ có điểm kết thúc $\le j$. Một truy vấn $[l,r]$ được tách thành tam giác các chuỗi bắt đầu tại $l$ và có điểm kết thúc $<\min(r+1,d[l])$, cộng với $\textit{pre}[r+1]-\textit{pre}[p]$. Tiền xử lý trong $O(n)$, trả lời mỗi truy vấn trong $O(1)$.

<!-- thinking:end -->

Chúng ta dùng hai biến $\textit{cnt0}$ và $\textit{cnt1}$ để lần lượt ghi nhận số lượng $0$ và $1$ trong cửa sổ hiện tại. Các con trỏ $i$ và $j$ đánh dấu biên trái và phải của cửa sổ. Ta dùng một mảng $d$ để ghi nhận, với mỗi vị trí $i$, vị trí đầu tiên bên phải không thỏa mãn ràng buộc $k$, ban đầu đặt $d[i] = n$. Ngoài ra, ta dùng mảng tổng tiền tố $\textit{pre}[i]$ có độ dài $n + 1$ để ghi nhận số chuỗi con thỏa mãn ràng buộc $k$ với biên phải ở vị trí $i$.

Khi di chuyển cửa sổ sang phải, nếu số lượng cả $0$ và $1$ trong cửa sổ đều lớn hơn $k$, ta cập nhật $d[i]$ thành $j$, cho biết vị trí đầu tiên bên phải $i$ không thỏa mãn ràng buộc $k$ là $j$. Sau đó, ta dịch $i$ sang phải một vị trí cho đến khi số lượng cả $0$ và $1$ trong cửa sổ đều nhỏ hơn hoặc bằng $k$. Lúc này, số chuỗi con thỏa mãn ràng buộc $k$ với biên phải tại $j$ là $j - i + 1$, và ta cập nhật giá trị này vào mảng tổng tiền tố.

Cuối cùng, với mỗi truy vấn $[l, r]$, trước tiên ta tìm vị trí đầu tiên $p$ bên phải $l$ không thỏa mãn ràng buộc $k$, với $p = \min(r + 1, d[l])$. Tất cả chuỗi con trong đoạn $[l, p - 1]$ đều thỏa mãn ràng buộc $k$, và số lượng của chúng là $(1 + p - l) \times (p - l) / 2$. Tiếp theo, ta tính số chuỗi con thỏa mãn ràng buộc $k$ có biên phải trong đoạn $[p, r]$, bằng $\textit{pre}[r + 1] - \textit{pre}[p]$. Cuối cùng, cộng hai kết quả này lại.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của chuỗi $s$ và mảng truy vấn $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKConstraintSubstrings(
        self, s: str, k: int, queries: List[List[int]]
    ) -> List[int]:
        cnt = [0, 0]
        i, n = 0, len(s)
        d = [n] * n
        pre = [0] * (n + 1)
        for j, x in enumerate(map(int, s)):
            cnt[x] += 1
            while cnt[0] > k and cnt[1] > k:
                d[i] = j
                cnt[int(s[i])] -= 1
                i += 1
            pre[j + 1] = pre[j] + j - i + 1
        ans = []
        for l, r in queries:
            p = min(r + 1, d[l])
            a = (1 + p - l) * (p - l) // 2
            b = pre[r + 1] - pre[p]
            ans.append(a + b)
        return ans
```

#### Java

```java
class Solution {
    public long[] countKConstraintSubstrings(String s, int k, int[][] queries) {
        int[] cnt = new int[2];
        int n = s.length();
        int[] d = new int[n];
        Arrays.fill(d, n);
        long[] pre = new long[n + 1];
        for (int i = 0, j = 0; j < n; ++j) {
            cnt[s.charAt(j) - '0']++;
            while (cnt[0] > k && cnt[1] > k) {
                d[i] = j;
                cnt[s.charAt(i++) - '0']--;
            }
            pre[j + 1] = pre[j] + j - i + 1;
        }
        int m = queries.length;
        long[] ans = new long[m];
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            int p = Math.min(r + 1, d[l]);
            long a = (1L + p - l) * (p - l) / 2;
            long b = pre[r + 1] - pre[p];
            ans[i] = a + b;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> countKConstraintSubstrings(string s, int k, vector<vector<int>>& queries) {
        int cnt[2]{};
        int n = s.size();
        vector<int> d(n, n);
        long long pre[n + 1];
        pre[0] = 0;
        for (int i = 0, j = 0; j < n; ++j) {
            cnt[s[j] - '0']++;
            while (cnt[0] > k && cnt[1] > k) {
                d[i] = j;
                cnt[s[i++] - '0']--;
            }
            pre[j + 1] = pre[j] + j - i + 1;
        }
        vector<long long> ans;
        for (const auto& q : queries) {
            int l = q[0], r = q[1];
            int p = min(r + 1, d[l]);
            long long a = (1LL + p - l) * (p - l) / 2;
            long long b = pre[r + 1] - pre[p];
            ans.push_back(a + b);
        }
        return ans;
    }
};
```

#### Go

```go
func countKConstraintSubstrings(s string, k int, queries [][]int) (ans []int64) {
	cnt := [2]int{}
	n := len(s)
	d := make([]int, n)
	for i := range d {
		d[i] = n
	}
	pre := make([]int, n+1)
	for i, j := 0, 0; j < n; j++ {
		cnt[s[j]-'0']++
		for cnt[0] > k && cnt[1] > k {
			d[i] = j
			cnt[s[i]-'0']--
			i++
		}
		pre[j+1] = pre[j] + j - i + 1
	}
	for _, q := range queries {
		l, r := q[0], q[1]
		p := min(r+1, d[l])
		a := (1 + p - l) * (p - l) / 2
		b := pre[r+1] - pre[p]
		ans = append(ans, int64(a+b))
	}
	return
}
```

#### TypeScript

```ts
function countKConstraintSubstrings(s: string, k: number, queries: number[][]): number[] {
    const cnt: [number, number] = [0, 0];
    const n = s.length;
    const d: number[] = Array(n).fill(n);
    const pre: number[] = Array(n + 1).fill(0);
    for (let i = 0, j = 0; j < n; ++j) {
        cnt[+s[j]]++;
        while (Math.min(cnt[0], cnt[1]) > k) {
            d[i] = j;
            cnt[+s[i++]]--;
        }
        pre[j + 1] = pre[j] + j - i + 1;
    }
    const ans: number[] = [];
    for (const [l, r] of queries) {
        const p = Math.min(r + 1, d[l]);
        const a = ((1 + p - l) * (p - l)) / 2;
        const b = pre[r + 1] - pre[p];
        ans.push(a + b);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
