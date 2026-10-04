---
comments: true
difficulty: Hard
rating: 2768
source: Weekly Contest 360 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2836. Maximize Value of Function in a Ball Passing Game](https://leetcode.com/problems/maximize-value-of-function-in-a-ball-passing-game)

[中文文档](/solution/2800-2899/2836.Maximize%20Value%20of%20Function%20in%20a%20Ball%20Passing%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>receiver</code> có độ dài <code>n</code> và một số nguyên <code>k</code>. Có <code>n</code> người chơi đang chơi trò chuyền bóng.</p>

<p>Bạn chọn người chơi bắt đầu là <code>i</code>. Trò chơi diễn ra như sau: người chơi <code>i</code> chuyền bóng cho người chơi <code>receiver[i]</code>, người này lại chuyền bóng cho <code>receiver[receiver[i]]</code>, và tiếp tục như vậy, tổng cộng <code>k</code> lần chuyền. Điểm của trò chơi là tổng các chỉ số của những người chơi đã chạm vào bóng, bao gồm cả các lần lặp lại, tức là <code>i + receiver[i] + receiver[receiver[i]] + ... + receiver<sup>(k)</sup>[i]</code>.</p>

<p>Trả về điểm <strong>lớn nhất</strong> có thể đạt được.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li><code>receiver</code> có thể chứa các phần tử trùng nhau.</li>
	<li><code>receiver[i]</code> có thể bằng <code>i</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">receiver = [2,0,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bắt đầu với người chơi <code>i = 2</code>, điểm ban đầu là 2:</p>

<table>
	<tbody>
		<tr>
			<th>Lượt chuyền</th>
			<th>Chỉ số người chuyền</th>
			<th>Chỉ số người nhận</th>
			<th>Điểm số</th>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td>1</td>
			<td>3</td>
		</tr>
		<tr>
			<td>2</td>
			<td>1</td>
			<td>0</td>
			<td>3</td>
		</tr>
		<tr>
			<td>3</td>
			<td>0</td>
			<td>2</td>
			<td>5</td>
		</tr>
		<tr>
			<td>4</td>
			<td>2</td>
			<td>1</td>
			<td>6</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">receiver = [1,1,1,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bắt đầu với người chơi <code>i = 4</code>, điểm ban đầu là 4:</p>

<table>
	<tbody>
		<tr>
			<th>Lượt chuyền</th>
			<th>Chỉ số người chuyền</th>
			<th>Chỉ số người nhận</th>
			<th>Điểm số</th>
		</tr>
		<tr>
			<td>1</td>
			<td>4</td>
			<td>3</td>
			<td>7</td>
		</tr>
		<tr>
			<td>2</td>
			<td>3</td>
			<td>2</td>
			<td>9</td>
		</tr>
		<tr>
			<td>3</td>
			<td>2</td>
			<td>1</td>
			<td>10</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= receiver.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= receiver[i] &lt;= n - 1</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>10</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Binary Lifting

<!-- thinking:start -->

> **Tư duy**
>
> Việc đi $k$ bước từ mỗi người chơi sẽ quá chậm khi $k$ lớn. Ánh xạ receiver tạo thành một functional graph, nên có thể áp dụng binary lifting: $f[i][j]$ là node sau $2^j$ lần chuyền và $g[i][j]$ là tổng các id trên đoạn đó (không bao gồm node cuối). Ghép các bước nhảy tương ứng với các bit của $k$.

<!-- thinking:end -->

Bài toán yêu cầu tìm tổng lớn nhất của các ID người chơi đã chạm vào bóng trong $k$ lần chuyền, bắt đầu từ mỗi người chơi $i$. Nếu giải bằng brute force, ta cần duyệt $k$ lần bắt đầu từ $i$, với độ phức tạp thời gian là $O(k)$, rõ ràng sẽ bị quá thời gian.

Ta có thể kết hợp quy hoạch động với binary lifting để xử lý bài toán này.

Ta định nghĩa $f[i][j]$ là ID của người chơi có thể đến được sau khi chuyền bóng $2^j$ lần, bắt đầu từ người chơi $i$, và $g[i][j]$ là tổng ID của những người chơi có thể đến được sau khi chuyền bóng $2^j$ lần, bắt đầu từ người chơi $i$ (không bao gồm người chơi cuối cùng).

Khi $j=0$, số lần chuyền là $1$, nên $f[i][0] = receiver[i]$ và $g[i][0] = i$.

Khi $j > 0$, số lần chuyền là $2^j$, tương đương với việc chuyền bóng $2^{j-1}$ lần bắt đầu từ người chơi $i$, sau đó chuyền bóng $2^{j-1}$ lần bắt đầu từ người chơi $f[i][j-1]$. Do đó, $f[i][j] = f[f[i][j-1]][j-1]$ và $g[i][j] = g[i][j-1] + g[f[i][j-1]][j-1]$.

Tiếp theo, ta có thể duyệt từng người chơi $i$ làm người bắt đầu, rồi cộng dồn theo biểu diễn nhị phân của $k$, cuối cùng tìm được tổng lớn nhất của các ID người chơi đã chạm vào bóng trong $k$ lần chuyền, bắt đầu từ người chơi $i$.

Độ phức tạp thời gian là $O(n \times \log k)$ và độ phức tạp không gian là $O(n \times \log k)$. Ở đây, $n$ là số người chơi.

Bài tương tự:

- [1483. Kth Ancestor of a Tree Node](https://github.com/doocs/leetcode/blob/main/solution/1400-1499/1483.Kth%20Ancestor%20of%20a%20Tree%20Node/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaxFunctionValue(self, receiver: List[int], k: int) -> int:
        n, m = len(receiver), k.bit_length()
        f = [[0] * m for _ in range(n)]
        g = [[0] * m for _ in range(n)]
        for i, x in enumerate(receiver):
            f[i][0] = x
            g[i][0] = i
        for j in range(1, m):
            for i in range(n):
                f[i][j] = f[f[i][j - 1]][j - 1]
                g[i][j] = g[i][j - 1] + g[f[i][j - 1]][j - 1]
        ans = 0
        for i in range(n):
            p, t = i, 0
            for j in range(m):
                if k >> j & 1:
                    t += g[p][j]
                    p = f[p][j]
            ans = max(ans, t + p)
        return ans
```

#### Java

```java
class Solution {
    public long getMaxFunctionValue(List<Integer> receiver, long k) {
        int n = receiver.size(), m = 64 - Long.numberOfLeadingZeros(k);
        int[][] f = new int[n][m];
        long[][] g = new long[n][m];
        for (int i = 0; i < n; ++i) {
            f[i][0] = receiver.get(i);
            g[i][0] = i;
        }
        for (int j = 1; j < m; ++j) {
            for (int i = 0; i < n; ++i) {
                f[i][j] = f[f[i][j - 1]][j - 1];
                g[i][j] = g[i][j - 1] + g[f[i][j - 1]][j - 1];
            }
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            int p = i;
            long t = 0;
            for (int j = 0; j < m; ++j) {
                if ((k >> j & 1) == 1) {
                    t += g[p][j];
                    p = f[p][j];
                }
            }
            ans = Math.max(ans, p + t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long getMaxFunctionValue(vector<int>& receiver, long long k) {
        int n = receiver.size(), m = 64 - __builtin_clzll(k);
        int f[n][m];
        long long g[n][m];
        for (int i = 0; i < n; ++i) {
            f[i][0] = receiver[i];
            g[i][0] = i;
        }
        for (int j = 1; j < m; ++j) {
            for (int i = 0; i < n; ++i) {
                f[i][j] = f[f[i][j - 1]][j - 1];
                g[i][j] = g[i][j - 1] + g[f[i][j - 1]][j - 1];
            }
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            int p = i;
            long long t = 0;
            for (int j = 0; j < m; ++j) {
                if (k >> j & 1) {
                    t += g[p][j];
                    p = f[p][j];
                }
            }
            ans = max(ans, p + t);
        }
        return ans;
    }
};
```

#### Go

```go
func getMaxFunctionValue(receiver []int, k int64) (ans int64) {
	n, m := len(receiver), bits.Len(uint(k))
	f := make([][]int, n)
	g := make([][]int64, n)
	for i := range f {
		f[i] = make([]int, m)
		g[i] = make([]int64, m)
		f[i][0] = receiver[i]
		g[i][0] = int64(i)
	}
	for j := 1; j < m; j++ {
		for i := 0; i < n; i++ {
			f[i][j] = f[f[i][j-1]][j-1]
			g[i][j] = g[i][j-1] + g[f[i][j-1]][j-1]
		}
	}
	for i := 0; i < n; i++ {
		p := i
		t := int64(0)
		for j := 0; j < m; j++ {
			if k>>j&1 == 1 {
				t += g[p][j]
				p = f[p][j]
			}
		}
		ans = max(ans, t+int64(p))
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
