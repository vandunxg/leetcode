---
comments: true
difficulty: Hard
rating: 2116
source: Weekly Contest 201 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1547. Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick)

[中文文档](/solution/1500-1599/1547.Minimum%20Cost%20to%20Cut%20a%20Stick/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một thanh gỗ dài <code>n</code> đơn vị. Thanh được đánh dấu từ <code>0</code> đến <code>n</code>. Ví dụ, thanh dài <strong>6</strong> được đánh dấu như sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1547.Minimum%20Cost%20to%20Cut%20a%20Stick/images/statement.jpg" style="width: 521px; height: 111px;" />
<p>Cho mảng số nguyên <code>cuts</code>, trong đó <code>cuts[i]</code> là vị trí cần cắt.</p>

<p>Bạn phải thực hiện tất cả lần cắt, nhưng có thể tùy ý thay đổi thứ tự.</p>

<p>Chi phí của một lần cắt là độ dài thanh đang bị cắt, tổng chi phí là tổng chi phí của mọi lần cắt. Khi cắt, thanh được chia thành hai thanh nhỏ hơn (tổng độ dài của chúng bằng độ dài thanh trước khi cắt). Hãy xem ví dụ đầu tiên để hiểu rõ hơn.</p>

<p>Trả về <em>tổng chi phí nhỏ nhất</em> của các lần cắt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1547.Minimum%20Cost%20to%20Cut%20a%20Stick/images/e1.jpg" style="width: 350px; height: 284px;" />
<pre>
<strong>Input:</strong> n = 7, cuts = [1,3,4,5]
<strong>Output:</strong> 16
<strong>Explanation:</strong> Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1547.Minimum%20Cost%20to%20Cut%20a%20Stick/images/e11.jpg" style="width: 350px; height: 284px;" />
Lần cắt đầu tiên thực hiện trên thanh dài 7 nên chi phí là 7. Lần thứ hai thực hiện trên thanh dài 6 (tức phần thứ hai sau lần cắt đầu), lần thứ ba trên thanh dài 4 và lần cuối trên thanh dài 3. Tổng chi phí là 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be [3, 5, 1, 4] for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 9, cuts = [5,6,1,4,2]
<strong>Output:</strong> 22
<strong>Explanation:</strong> Nếu thực hiện theo thứ tự cuts đã cho, chi phí sẽ là 25.
There are much ordering with total cost &lt;= 25, for example, the order [4, 6, 5, 2, 1] has total cost = 22 which is the minimum possible.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= cuts.length &lt;= min(n - 1, 100)</code></li>
	<li><code>1 &lt;= cuts[i] &lt;= n - 1</code></li>
	<li>All the integers in <code>cuts</code> array are <strong>distinct</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming (Interval DP)

<!-- thinking:start -->

> **Tư duy**
>
> Cắt thanh dài $n$ tại các vị trí cho trước; mỗi lần cắt có chi phí bằng độ dài mảnh hiện tại. $n$ có thể tới $10^6$ nhưng chỉ có tối đa $100$ vị trí cắt, nên DP cần dựa trên danh sách vị trí cắt, không phải tọa độ của thanh.
>
> Thêm $0$ và $n$ rồi sắp xếp. $f[i][j]$ là chi phí nhỏ nhất để hoàn tất mọi lần cắt trong danh sách $\textit{cuts}$, cụ thể là $(cuts[i],cuts[j])$. Lần cắt cuối tại $cuts[k]$ có chi phí $f[i][k]+f[k][j]+cuts[j]-cuts[i]$. Tính theo độ dài $l$ của khoảng tăng dần giúp mọi khoảng con đã sẵn sàng.

<!-- thinking:end -->

Ta thêm hai phần tử $0$ và $n$ vào mảng $\textit{cuts}$ để biểu diễn hai đầu thanh. Sau đó sắp xếp mảng này để chia toàn bộ thanh thành các khoảng, mỗi khoảng có hai điểm biên. Gọi độ dài mảng $\textit{cuts}$ là $m$.

Tiếp theo, định nghĩa $\textit{f}[i][j]$ là chi phí nhỏ nhất để cắt khoảng $[\textit{cuts}[i], \textit{cuts}[j]]$.

Nếu một khoảng chỉ có hai điểm biên, nghĩa là không cần cắt khoảng này, thì $\textit{f}[i][j] = 0$.

Ngược lại, ta duyệt độ dài $l$ của khoảng, bằng số điểm cắt trừ $1$. Sau đó duyệt đầu trái $i$, đầu phải $j$ có thể lấy từ $i + l$. Với mỗi khoảng, duyệt điểm cắt $k$ sao cho $i \lt k \lt j$, chia khoảng $[i, j]$ thành $[i, k]$ và $[k, j]$. Chi phí là $\textit{f}[i][k] + \textit{f}[k][j] + \textit{cuts}[j] - \textit{cuts}[i]$; lấy nhỏ nhất trên mọi $k$ để được $\textit{f}[i][j]$.

Cuối cùng, trả về $\textit{f}[0][m - 1]$.

Độ phức tạp thời gian là $O(m^3)$ và độ phức tạp không gian là $O(m^2)$, trong đó $m$ là độ dài mảng $\textit{cuts}$ sau khi sửa đổi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int, cuts: List[int]) -> int:
        cuts.extend([0, n])
        cuts.sort()
        m = len(cuts)
        f = [[0] * m for _ in range(m)]
        for l in range(2, m):
            for i in range(m - l):
                j = i + l
                f[i][j] = inf
                for k in range(i + 1, j):
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i])
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int minCost(int n, int[] cuts) {
        List<Integer> nums = new ArrayList<>();
        for (int x : cuts) {
            nums.add(x);
        }
        nums.add(0);
        nums.add(n);
        Collections.sort(nums);
        int m = nums.size();
        int[][] f = new int[m][m];
        for (int l = 2; l < m; ++l) {
            for (int i = 0; i + l < m; ++i) {
                int j = i + l;
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + nums.get(j) - nums.get(i));
                }
            }
        }
        return f[0][m - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(cuts.begin(), cuts.end());
        int m = cuts.size();
        int f[110][110]{};
        for (int l = 2; l < m; ++l) {
            for (int i = 0; i + l < m; ++i) {
                int j = i + l;
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i]);
                }
            }
        }
        return f[0][m - 1];
    }
};
```

#### Go

```go
func minCost(n int, cuts []int) int {
	cuts = append(cuts, []int{0, n}...)
	sort.Ints(cuts)
	m := len(cuts)
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, m)
	}
	for l := 2; l < m; l++ {
		for i := 0; i+l < m; i++ {
			j := i + l
			f[i][j] = 1 << 30
			for k := i + 1; k < j; k++ {
				f[i][j] = min(f[i][j], f[i][k]+f[k][j]+cuts[j]-cuts[i])
			}
		}
	}
	return f[0][m-1]
}
```

#### TypeScript

```ts
function minCost(n: number, cuts: number[]): number {
    cuts.push(0, n);
    cuts.sort((a, b) => a - b);
    const m = cuts.length;
    const f: number[][] = Array.from({ length: m }, () => Array(m).fill(0));
    for (let l = 2; l < m; l++) {
        for (let i = 0; i < m - l; i++) {
            const j = i + l;
            f[i][j] = Infinity;
            for (let k = i + 1; k < j; k++) {
                f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i]);
            }
        }
    }
    return f[0][m - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming (Một cách duyệt khác)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt theo độ dài để tính các khoảng ngắn trước. Công thức truy hồi vẫn đúng khi $i$ chạy giảm và $j$ chạy tăng, vì mọi $i<k<j$ cũng được tính trước. Độ phức tạp không đổi, chỉ khác thứ tự vòng lặp.

<!-- thinking:end -->

Ta cũng có thể duyệt $i$ từ lớn đến nhỏ và $j$ từ nhỏ đến lớn. Điều này đảm bảo khi tính $f[i][j]$, các trạng thái $f[i][k]$ và $f[k][j]$ với $i \lt k \lt j$ đã được tính.

Độ phức tạp thời gian là $O(m^3)$ và độ phức tạp không gian là $O(m^2)$, trong đó $m$ là độ dài mảng $\textit{cuts}$ sau khi sửa đổi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int, cuts: List[int]) -> int:
        cuts.extend([0, n])
        cuts.sort()
        m = len(cuts)
        f = [[0] * m for _ in range(m)]
        for i in range(m - 1, -1, -1):
            for j in range(i + 2, m):
                f[i][j] = inf
                for k in range(i + 1, j):
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i])
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int minCost(int n, int[] cuts) {
        List<Integer> nums = new ArrayList<>();
        for (int x : cuts) {
            nums.add(x);
        }
        nums.add(0);
        nums.add(n);
        Collections.sort(nums);
        int m = nums.size();
        int[][] f = new int[m][m];
        for (int i = m - 1; i >= 0; --i) {
            for (int j = i + 2; j < m; ++j) {
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + nums.get(j) - nums.get(i));
                }
            }
        }
        return f[0][m - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(cuts.begin(), cuts.end());
        int m = cuts.size();
        int f[110][110]{};
        for (int i = m - 1; ~i; --i) {
            for (int j = i + 2; j < m; ++j) {
                f[i][j] = 1 << 30;
                for (int k = i + 1; k < j; ++k) {
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i]);
                }
            }
        }
        return f[0][m - 1];
    }
};
```

#### Go

```go
func minCost(n int, cuts []int) int {
	cuts = append(cuts, []int{0, n}...)
	sort.Ints(cuts)
	m := len(cuts)
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, m)
	}
	for i := m - 1; i >= 0; i-- {
		for j := i + 2; j < m; j++ {
			f[i][j] = 1 << 30
			for k := i + 1; k < j; k++ {
				f[i][j] = min(f[i][j], f[i][k]+f[k][j]+cuts[j]-cuts[i])
			}
		}
	}
	return f[0][m-1]
}
```

#### TypeScript

```ts
function minCost(n: number, cuts: number[]): number {
    cuts.push(0);
    cuts.push(n);
    cuts.sort((a, b) => a - b);
    const m = cuts.length;
    const f: number[][] = Array.from({ length: m }, () => Array(m).fill(0));
    for (let i = m - 2; i >= 0; --i) {
        for (let j = i + 2; j < m; ++j) {
            f[i][j] = 1 << 30;
            for (let k = i + 1; k < j; ++k) {
                f[i][j] = Math.min(f[i][j], f[i][k] + f[k][j] + cuts[j] - cuts[i]);
            }
        }
    }
    return f[0][m - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
