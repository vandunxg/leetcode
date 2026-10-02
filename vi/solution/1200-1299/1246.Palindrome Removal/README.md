---
comments: true
difficulty: Hard
rating: 2203
source: Biweekly Contest 12 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1246. Palindrome Removal 🔒](https://leetcode.com/problems/palindrome-removal)

[中文文档](/solution/1200-1299/1246.Palindrome%20Removal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>.</p>

<p>Trong một lượt, bạn có thể chọn một mảng con <strong>đối xứng</strong> <code>arr[i], arr[i + 1], ..., arr[j]</code> với <code>i &lt;= j</code>, rồi xóa mảng con đó khỏi mảng đã cho. Sau khi xóa, các phần tử ở bên trái và bên phải mảng con sẽ dịch chuyển để lấp vào khoảng trống.</p>

<p>Trả về <em>số lượt ít nhất cần thiết để xóa tất cả các số khỏi mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,3,4,1,5]
<strong>Đầu ra:</strong> 3
<b>Giải thích: </b>Xóa [4], sau đó xóa [1,3,1], rồi xóa [5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Interval DP)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt xóa một mảng con đối xứng. $n \le 100$. Tính chất cấu trúc con tối ưu trên các đoạn gợi ý dùng DP: $f[i][j]$ là số lượt ít nhất để xóa $arr[i..j]$.
>
> Nếu hai đầu đoạn bằng nhau, chúng có thể được xóa cùng với đoạn bên trong; nếu không, ta chia tại $k$ rồi xóa riêng hai phần. Tính theo độ dài đoạn đảm bảo các đoạn ngắn hơn đã có kết quả. Độ phức tạp $n^3$ chấp nhận được với $n=100$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số thao tác ít nhất cần để xóa mọi số trong đoạn chỉ số $[i,..j]$. Ban đầu, $f[i][i] = 1$, nghĩa là nếu chỉ có một số thì cần một thao tác xóa.

Với $f[i][j]$, nếu $i + 1 = j$, tức đoạn chỉ có hai số, thì $f[i][j] = 1$ nếu $arr[i]=arr[j]$; nếu không, $f[i][j] = 2$.

Với đoạn có hơn hai số, nếu $arr[i]=arr[j]$, ta có thể lấy $f[i][j] = f[i + 1][j - 1]$. Ngoài ra, ta có thể duyệt $k$ trong đoạn chỉ số $[i,..j-1]$, lấy giá trị nhỏ nhất của $f[i][k] + f[k + 1][j]$, rồi gán giá trị nhỏ nhất đó cho $f[i][j]$.

Đáp án là $f[0][n - 1]$.

Độ phức tạp thời gian là $O(n^3)$, độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, arr: List[int]) -> int:
        n = len(arr)
        f = [[0] * n for _ in range(n)]
        for i in range(n):
            f[i][i] = 1
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                if i + 1 == j:
                    f[i][j] = 1 if arr[i] == arr[j] else 2
                else:
                    t = f[i + 1][j - 1] if arr[i] == arr[j] else inf
                    for k in range(i, j):
                        t = min(t, f[i][k] + f[k + 1][j])
                    f[i][j] = t
        return f[0][n - 1]
```

#### Java

```java
class Solution {
    public int minimumMoves(int[] arr) {
        int n = arr.length;
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (i + 1 == j) {
                    f[i][j] = arr[i] == arr[j] ? 1 : 2;
                } else {
                    int t = arr[i] == arr[j] ? f[i + 1][j - 1] : 1 << 30;
                    for (int k = i; k < j; ++k) {
                        t = Math.min(t, f[i][k] + f[k + 1][j]);
                    }
                    f[i][j] = t;
                }
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumMoves(vector<int>& arr) {
        int n = arr.size();
        int f[n][n];
        memset(f, 0, sizeof f);
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (i + 1 == j) {
                    f[i][j] = arr[i] == arr[j] ? 1 : 2;
                } else {
                    int t = arr[i] == arr[j] ? f[i + 1][j - 1] : 1 << 30;
                    for (int k = i; k < j; ++k) {
                        t = min(t, f[i][k] + f[k + 1][j]);
                    }
                    f[i][j] = t;
                }
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func minimumMoves(arr []int) int {
	n := len(arr)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		f[i][i] = 1
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			if i+1 == j {
				f[i][j] = 2
				if arr[i] == arr[j] {
					f[i][j] = 1
				}
			} else {
				t := 1 << 30
				if arr[i] == arr[j] {
					t = f[i+1][j-1]
				}
				for k := i; k < j; k++ {
					t = min(t, f[i][k]+f[k+1][j])
				}
				f[i][j] = t
			}
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function minimumMoves(arr: number[]): number {
    const n = arr.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

    for (let i = 0; i < n; ++i) {
        f[i][i] = 1;
    }

    for (let i = n - 2; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            if (i + 1 === j) {
                f[i][j] = arr[i] === arr[j] ? 1 : 2;
            } else {
                let t = arr[i] === arr[j] ? f[i + 1][j - 1] : Infinity;
                for (let k = i; k < j; ++k) {
                    t = Math.min(t, f[i][k] + f[k + 1][j]);
                }
                f[i][j] = t;
            }
        }
    }

    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
