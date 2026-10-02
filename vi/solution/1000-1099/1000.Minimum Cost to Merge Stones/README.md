---
comments: true
difficulty: Hard
rating: 2422
source: Weekly Contest 126 Q4
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1000. Minimum Cost to Merge Stones](https://leetcode.com/problems/minimum-cost-to-merge-stones)

[中文文档](/solution/1000-1099/1000.Minimum%20Cost%20to%20Merge%20Stones/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> đống <code>stones</code> được xếp thành một hàng. Đống thứ <code>i</code> có <code>stones[i]</code> viên đá.</p>

<p>Mỗi lượt, ta gộp đúng <code>k</code> đống <strong>liên tiếp</strong> thành một đống; chi phí của lượt đó bằng tổng số viên đá trong <code>k</code> đống này.</p>

<p>Hãy trả về <em>chi phí nhỏ nhất để gộp tất cả các đống đá thành một đống</em>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [3,2,4,1], k = 2
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Ban đầu, ta có [3, 2, 4, 1].
Ta gộp [3, 2] với chi phí 5, còn lại [5, 4, 1].
Ta gộp [4, 1] với chi phí 5, còn lại [5, 5].
Ta gộp [5, 5] với chi phí 10, còn lại [10].
Tổng chi phí là 20, đây là mức nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [3,2,4,1], k = 3
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Sau bất kỳ lượt gộp nào, còn lại 2 đống và ta không thể gộp tiếp. Vì vậy, không thể hoàn thành yêu cầu.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [3,5,1,2,6], k = 3
<strong>Đầu ra:</strong> 25
<strong>Giải thích:</strong> Ban đầu, ta có [3, 5, 1, 2, 6].
Ta gộp [5, 1, 2] với chi phí 8, còn lại [3, 8, 6].
Ta gộp [3, 8, 6] với chi phí 17, còn lại [17].
Tổng chi phí là 25, đây là mức nhỏ nhất có thể.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == stones.length</code></li>
	<li><code>1 &lt;= n &lt;= 30</code></li>
	<li><code>1 &lt;= stones[i] &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê mọi thứ tự gộp sẽ cho đáp án đúng, nhưng với $n \le 30$, số cách phân hoạch hợp lệ tăng quá nhanh để duyệt hết. Mỗi lượt chỉ gộp $K$ đống liên tiếp, nên chi phí được xác định bởi các bài toán con dạng “biến một đoạn liên tiếp thành một số đống nhất định”; cùng một đoạn có thể đạt được theo nhiều thứ tự gộp khác nhau.
>
> Mỗi lượt gộp làm số đống giảm đi $K-1$, vì vậy chỉ có thể còn một đống nếu $(n-1)\bmod (K-1)=0$; nếu không, đáp án là $-1$. Khi có thể thực hiện, để gộp đoạn $[i,j]$ thành $k$ đống, ta chia đoạn sao cho phần đầu thành $1$ đống và phần sau thành $k-1$ đống. Để gộp thành $1$ đống, trước tiên cần tạo được $K$ đống rồi cộng tổng số đá trong đoạn đó vào chi phí.
>
> Vì vậy, ta tính $f[i][j][k]$ theo thứ tự độ dài đoạn tăng dần và dùng prefix sum $s$ để tính tổng trên đoạn trong $O(1)$. Điểm chia $h$ luôn dùng $f[i][h][1]$ cho đoạn bên trái, phù hợp với ràng buộc chỉ gộp các đống liên tiếp. Đáp án là $f[1][n][1]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeStones(self, stones: List[int], K: int) -> int:
        n = len(stones)
        if (n - 1) % (K - 1):
            return -1
        s = list(accumulate(stones, initial=0))
        f = [[[inf] * (K + 1) for _ in range(n + 1)] for _ in range(n + 1)]
        for i in range(1, n + 1):
            f[i][i][1] = 0
        for l in range(2, n + 1):
            for i in range(1, n - l + 2):
                j = i + l - 1
                for k in range(1, K + 1):
                    for h in range(i, j):
                        f[i][j][k] = min(f[i][j][k], f[i][h][1] + f[h + 1][j][k - 1])
                f[i][j][1] = f[i][j][K] + s[j] - s[i - 1]
        return f[1][n][1]
```

#### Java

```java
class Solution {
    public int mergeStones(int[] stones, int K) {
        int n = stones.length;
        if ((n - 1) % (K - 1) != 0) {
            return -1;
        }
        int[] s = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + stones[i - 1];
        }
        int[][][] f = new int[n + 1][n + 1][K + 1];
        final int inf = 1 << 20;
        for (int[][] g : f) {
            for (int[] e : g) {
                Arrays.fill(e, inf);
            }
        }
        for (int i = 1; i <= n; ++i) {
            f[i][i][1] = 0;
        }
        for (int l = 2; l <= n; ++l) {
            for (int i = 1; i + l - 1 <= n; ++i) {
                int j = i + l - 1;
                for (int k = 1; k <= K; ++k) {
                    for (int h = i; h < j; ++h) {
                        f[i][j][k] = Math.min(f[i][j][k], f[i][h][1] + f[h + 1][j][k - 1]);
                    }
                }
                f[i][j][1] = f[i][j][K] + s[j] - s[i - 1];
            }
        }
        return f[1][n][1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mergeStones(vector<int>& stones, int K) {
        int n = stones.size();
        if ((n - 1) % (K - 1)) {
            return -1;
        }
        int s[n + 1];
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + stones[i - 1];
        }
        int f[n + 1][n + 1][K + 1];
        memset(f, 0x3f, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            f[i][i][1] = 0;
        }
        for (int l = 2; l <= n; ++l) {
            for (int i = 1; i + l - 1 <= n; ++i) {
                int j = i + l - 1;
                for (int k = 1; k <= K; ++k) {
                    for (int h = i; h < j; ++h) {
                        f[i][j][k] = min(f[i][j][k], f[i][h][1] + f[h + 1][j][k - 1]);
                    }
                }
                f[i][j][1] = f[i][j][K] + s[j] - s[i - 1];
            }
        }
        return f[1][n][1];
    }
};
```

#### Go

```go
func mergeStones(stones []int, K int) int {
	n := len(stones)
	if (n-1)%(K-1) != 0 {
		return -1
	}
	s := make([]int, n+1)
	for i, x := range stones {
		s[i+1] = s[i] + x
	}
	f := make([][][]int, n+1)
	for i := range f {
		f[i] = make([][]int, n+1)
		for j := range f[i] {
			f[i][j] = make([]int, K+1)
			for k := range f[i][j] {
				f[i][j][k] = 1 << 20
			}
		}
	}
	for i := 1; i <= n; i++ {
		f[i][i][1] = 0
	}
	for l := 2; l <= n; l++ {
		for i := 1; i <= n-l+1; i++ {
			j := i + l - 1
			for k := 2; k <= K; k++ {
				for h := i; h < j; h++ {
					f[i][j][k] = min(f[i][j][k], f[i][h][k-1]+f[h+1][j][1])
				}
			}
			f[i][j][1] = f[i][j][K] + s[j] - s[i-1]
		}
	}
	return f[1][n][1]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
