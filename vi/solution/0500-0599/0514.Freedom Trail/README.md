---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [514. Freedom Trail](https://leetcode.com/problems/freedom-trail)

[中文文档](/solution/0500-0599/0514.Freedom%20Trail/README.md)

## Mô tả

<!-- description:start -->

<p>Trong trò chơi Fallout 4, nhiệm vụ <strong>&quot;Road to Freedom&quot;</strong> yêu cầu người chơi tìm đến một vòng xoay kim loại tên là <strong>&quot;Freedom Trail Ring&quot;</strong> và xoay vòng để nhập một từ khóa cụ thể nhằm mở cửa.</p>

<p>Cho chuỗi <code>ring</code> biểu diễn mã khắc trên vòng ngoài và chuỗi <code>key</code> biểu diễn từ khóa cần nhập. Hãy trả về <em>số bước ít nhất để nhập tất cả ký tự của từ khóa</em>.</p>

<p>Ban đầu, ký tự đầu tiên của vòng nằm ở vị trí <code>&quot;12:00&quot;</code>. Bạn cần nhập từng ký tự trong <code>key</code> bằng cách xoay <code>ring</code> theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ để đưa ký tự tương ứng trong chuỗi key đến vị trí <code>&quot;12:00&quot;</code>, sau đó nhấn nút ở giữa.</p>

<p>Ở bước xoay vòng để nhập ký tự <code>key[i]</code>:</p>

<ol>
	<li>Bạn có thể xoay vòng một vị trí theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ; mỗi lần xoay tính là <strong>một bước</strong>. Mục tiêu là đưa một ký tự trong <code>ring</code> trùng với <code>key[i]</code> đến vị trí <code>&quot;12:00&quot;</code>.</li>
	<li>Khi ký tự <code>key[i]</code> đã được đưa đến vị trí <code>&quot;12:00&quot;</code>, hãy nhấn nút ở giữa để nhập ký tự đó; thao tác này cũng tính là <strong>một bước</strong>. Sau khi nhấn, bạn có thể bắt đầu nhập ký tự tiếp theo trong key. Lặp lại cho đến khi nhập xong toàn bộ từ khóa.</li>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0500-0599/0514.Freedom%20Trail/images/ring.jpg" style="width: 450px; height: 450px;" />
<pre>
<strong>Đầu vào:</strong> ring = &quot;godding&quot;, key = &quot;gd&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Với ký tự đầu tiên &#39;g&#39; của key, ký tự này đã ở đúng vị trí nên chỉ cần 1 bước để nhập. 
Với ký tự thứ hai &#39;d&#39; của key, ta cần xoay ring &quot;godding&quot; ngược chiều kim đồng hồ 2 bước để vòng trở thành &quot;ddinggo&quot;.
Ngoài ra, cần thêm 1 bước để nhập ký tự.
Vậy kết quả cuối cùng là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ring = &quot;godding&quot;, key = &quot;godding&quot;
<strong>Đầu ra:</strong> 13
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ring.length, key.length &lt;= 100</code></li>
	<li><code>ring</code> và <code>key</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo rằng luôn có thể nhập <code>key</code> bằng cách xoay <code>ring</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi ký tự trong key, ta có thể xoay vòng theo chiều kim đồng hồ hoặc ngược chiều rồi nhấn xác nhận. Nếu thử mọi vị trí của ring cho từng ký tự trong key, số trường hợp sẽ tăng theo $|key|$.
>
> Gọi $f[i][j]$ là số bước ít nhất để nhập $i+1$ ký tự đầu của key và dừng tại chỉ số $j$. Tính trước các vị trí của từng chữ cái. Mỗi lần chuyển trạng thái cộng thêm cung xoay ngắn hơn và một lần nhấn. Đáp án là giá trị nhỏ nhất trong các vị trí chứa ký tự cuối cùng của key.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý các vị trí của từng ký tự $c$ trong chuỗi $ring$ và lưu chúng vào mảng $pos[c]$. Gọi độ dài của các chuỗi $key$ và $ring$ lần lượt là $m$ và $n$.

Tiếp theo, ta định nghĩa $f[i][j]$ là số bước ít nhất để nhập $i+1$ ký tự đầu tiên của chuỗi $key$ sao cho ký tự thứ $j$ của $ring$ nằm tại vị trí $12:00$. Ban đầu, $f[i][j]=+\infty$. Đáp án là $\min_{0 \leq j < n} f[m - 1][j]$.

Trước hết, ta khởi tạo $f[0][j]$, với $j$ là vị trí xuất hiện của ký tự $key[0]$ trong $ring$. Khi ký tự thứ $j$ của $ring$ được đưa đến vị trí $12:00$, ta chỉ cần $1$ bước để nhập $key[0]$. Số bước xoay $ring$ đến vị trí đó là $min(j, n - j)$. Do đó, $f[0][j]=min(j, n - j) + 1$.

Tiếp theo, xét chuyển trạng thái khi $i \geq 1$. Ta duyệt các vị trí $pos[key[i]]$ mà $key[i]$ xuất hiện trong $ring$, đồng thời duyệt các vị trí $pos[key[i-1]]$ mà $key[i-1]$ xuất hiện, rồi cập nhật $f[i][j]$ theo công thức $f[i][j]=\min_{k \in pos[key[i-1]]} f[i-1][k] + \min(\textit{abs}(j - k), n - \textit{abs}(j - k)) + 1$.

Cuối cùng, trả về $\min_{0 \leq j \lt n} f[m - 1][j]$.

Độ phức tạp thời gian là $O(m \times n^2)$, độ phức tạp không gian là $O(m \times n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của chuỗi $key$ và $ring$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRotateSteps(self, ring: str, key: str) -> int:
        m, n = len(key), len(ring)
        pos = defaultdict(list)
        for i, c in enumerate(ring):
            pos[c].append(i)
        f = [[inf] * n for _ in range(m)]
        for j in pos[key[0]]:
            f[0][j] = min(j, n - j) + 1
        for i in range(1, m):
            for j in pos[key[i]]:
                for k in pos[key[i - 1]]:
                    f[i][j] = min(
                        f[i][j], f[i - 1][k] + min(abs(j - k), n - abs(j - k)) + 1
                    )
        return min(f[-1][j] for j in pos[key[-1]])
```

#### Java

```java
class Solution {
    public int findRotateSteps(String ring, String key) {
        int m = key.length(), n = ring.length();
        List<Integer>[] pos = new List[26];
        Arrays.setAll(pos, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            int j = ring.charAt(i) - 'a';
            pos[j].add(i);
        }
        int[][] f = new int[m][n];
        for (var g : f) {
            Arrays.fill(g, 1 << 30);
        }
        for (int j : pos[key.charAt(0) - 'a']) {
            f[0][j] = Math.min(j, n - j) + 1;
        }
        for (int i = 1; i < m; ++i) {
            for (int j : pos[key.charAt(i) - 'a']) {
                for (int k : pos[key.charAt(i - 1) - 'a']) {
                    f[i][j] = Math.min(
                        f[i][j], f[i - 1][k] + Math.min(Math.abs(j - k), n - Math.abs(j - k)) + 1);
                }
            }
        }
        int ans = 1 << 30;
        for (int j : pos[key.charAt(m - 1) - 'a']) {
            ans = Math.min(ans, f[m - 1][j]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findRotateSteps(string ring, string key) {
        int m = key.size(), n = ring.size();
        vector<int> pos[26];
        for (int j = 0; j < n; ++j) {
            pos[ring[j] - 'a'].push_back(j);
        }
        int f[m][n];
        memset(f, 0x3f, sizeof(f));
        for (int j : pos[key[0] - 'a']) {
            f[0][j] = min(j, n - j) + 1;
        }
        for (int i = 1; i < m; ++i) {
            for (int j : pos[key[i] - 'a']) {
                for (int k : pos[key[i - 1] - 'a']) {
                    f[i][j] = min(f[i][j], f[i - 1][k] + min(abs(j - k), n - abs(j - k)) + 1);
                }
            }
        }
        int ans = 1 << 30;
        for (int j : pos[key[m - 1] - 'a']) {
            ans = min(ans, f[m - 1][j]);
        }
        return ans;
    }
};
```

#### Go

```go
func findRotateSteps(ring string, key string) int {
	m, n := len(key), len(ring)
	pos := [26][]int{}
	for j, c := range ring {
		pos[c-'a'] = append(pos[c-'a'], j)
	}
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = 1 << 30
		}
	}
	for _, j := range pos[key[0]-'a'] {
		f[0][j] = min(j, n-j) + 1
	}
	for i := 1; i < m; i++ {
		for _, j := range pos[key[i]-'a'] {
			for _, k := range pos[key[i-1]-'a'] {
				f[i][j] = min(f[i][j], f[i-1][k]+min(abs(j-k), n-abs(j-k))+1)
			}
		}
	}
	ans := 1 << 30
	for _, j := range pos[key[m-1]-'a'] {
		ans = min(ans, f[m-1][j])
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function findRotateSteps(ring: string, key: string): number {
    const m: number = key.length;
    const n: number = ring.length;
    const pos: number[][] = Array.from({ length: 26 }, () => []);
    for (let i = 0; i < n; ++i) {
        const j: number = ring.charCodeAt(i) - 'a'.charCodeAt(0);
        pos[j].push(i);
    }

    const f: number[][] = Array.from({ length: m }, () => Array(n).fill(1 << 30));
    for (const j of pos[key.charCodeAt(0) - 'a'.charCodeAt(0)]) {
        f[0][j] = Math.min(j, n - j) + 1;
    }

    for (let i = 1; i < m; ++i) {
        for (const j of pos[key.charCodeAt(i) - 'a'.charCodeAt(0)]) {
            for (const k of pos[key.charCodeAt(i - 1) - 'a'.charCodeAt(0)]) {
                f[i][j] = Math.min(
                    f[i][j],
                    f[i - 1][k] + Math.min(Math.abs(j - k), n - Math.abs(j - k)) + 1,
                );
            }
        }
    }

    let ans: number = 1 << 30;
    for (const j of pos[key.charCodeAt(m - 1) - 'a'.charCodeAt(0)]) {
        ans = Math.min(ans, f[m - 1][j]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
