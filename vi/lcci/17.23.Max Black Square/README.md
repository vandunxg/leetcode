---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.23. Max Black Square](https://leetcode.cn/problems/max-black-square-lcci)

[中文文档](/lcci/17.23.Max%20Black%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy hình dung bạn có một ma trận vuông, trong đó mỗi ô (pixel) có màu đen hoặc trắng. Hãy thiết kế một thuật toán để tìm hình vuông con lớn nhất sao cho cả bốn đường biên đều được tô bằng các pixel đen.</p>
<p>Trả về một mảng <code>[r, c, size]</code>, trong đó <code>r</code>, <code>c</code> lần lượt là số hàng và số cột của góc trên bên trái của hình vuông con, còn <code>size</code> là độ dài cạnh của hình vuông. Nếu có nhiều đáp án, trả về đáp án có <code>r</code> nhỏ nhất. Nếu có nhiều đáp án có cùng <code>r</code>, trả về đáp án có <code>c</code> nhỏ nhất. Nếu không có đáp án, trả về một mảng rỗng.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:

</strong>[

&nbsp; [1,0,1],

&nbsp; [<strong>0,0</strong>,1],

&nbsp; [<strong>0,0</strong>,1]

]

<strong>Đầu ra: </strong>[1,0,2]

<strong>Giải thích:</strong> 0 đại diện cho màu đen và 1 đại diện cho màu trắng; các phần tử được in đậm trong đầu vào là đáp án.

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:

</strong>[

&nbsp; [<strong>0</strong>,1,1],

&nbsp; [1,0,1],

&nbsp; [1,1,0]

]

<strong>Đầu ra: </strong>[0,0,1]

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>matrix.length == matrix[0].length &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hình vuông lớn nhất có viền đen. Duyệt mọi độ dài cạnh và quét bốn cạnh có độ phức tạp $O(n^4)$.
>
> Với các đoạn liên tiếp gồm số 0 theo hướng xuống và sang phải, cạnh $k$ hợp lệ khi bốn tia xuất phát từ các góc đều có độ dài ít nhất $k$.
>
> Tính $down$ và $right$ từ góc dưới bên phải; thử $k$ từ lớn đến nhỏ cùng từng ô góc trên bên trái. Lần đầu tìm thấy là hình vuông lớn nhất, trả về $[i,j,k]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSquare(self, matrix: List[List[int]]) -> List[int]:
        n = len(matrix)
        down = [[0] * n for _ in range(n)]
        right = [[0] * n for _ in range(n)]
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if matrix[i][j] == 0:
                    down[i][j] = down[i + 1][j] + 1 if i + 1 < n else 1
                    right[i][j] = right[i][j + 1] + 1 if j + 1 < n else 1
        for k in range(n, 0, -1):
            for i in range(n - k + 1):
                for j in range(n - k + 1):
                    if (
                        down[i][j] >= k
                        and right[i][j] >= k
                        and right[i + k - 1][j] >= k
                        and down[i][j + k - 1] >= k
                    ):
                        return [i, j, k]
        return []
```

#### Java

```java
class Solution {
    public int[] findSquare(int[][] matrix) {
        int n = matrix.length;
        int[][] down = new int[n][n];
        int[][] right = new int[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (matrix[i][j] == 0) {
                    down[i][j] = i + 1 < n ? down[i + 1][j] + 1 : 1;
                    right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
                }
            }
        }
        for (int k = n; k > 0; --k) {
            for (int i = 0; i <= n - k; ++i) {
                for (int j = 0; j <= n - k; ++j) {
                    if (down[i][j] >= k && right[i][j] >= k && right[i + k - 1][j] >= k
                        && down[i][j + k - 1] >= k) {
                        return new int[] {i, j, k};
                    }
                }
            }
        }
        return new int[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findSquare(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int down[n][n];
        int right[n][n];
        memset(down, 0, sizeof(down));
        memset(right, 0, sizeof(right));
        for (int i = n - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (matrix[i][j] == 0) {
                    down[i][j] = i + 1 < n ? down[i + 1][j] + 1 : 1;
                    right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
                }
            }
        }
        for (int k = n; k > 0; --k) {
            for (int i = 0; i <= n - k; ++i) {
                for (int j = 0; j <= n - k; ++j) {
                    if (down[i][j] >= k && right[i][j] >= k && right[i + k - 1][j] >= k && down[i][j + k - 1] >= k) {
                        return {i, j, k};
                    }
                }
            }
        }
        return {};
    }
};
```

#### Go

```go
func findSquare(matrix [][]int) []int {
	n := len(matrix)
	down := make([][]int, n)
	right := make([][]int, n)
	for i := range down {
		down[i] = make([]int, n)
		right[i] = make([]int, n)
	}
	for i := n - 1; i >= 0; i-- {
		for j := n - 1; j >= 0; j-- {
			if matrix[i][j] == 0 {
				down[i][j], right[i][j] = 1, 1
				if i+1 < n {
					down[i][j] += down[i+1][j]
				}
				if j+1 < n {
					right[i][j] += right[i][j+1]
				}
			}
		}
	}
	for k := n; k > 0; k-- {
		for i := 0; i <= n-k; i++ {
			for j := 0; j <= n-k; j++ {
				if down[i][j] >= k && right[i][j] >= k && right[i+k-1][j] >= k && down[i][j+k-1] >= k {
					return []int{i, j, k}
				}
			}
		}
	}
	return []int{}
}
```

#### TypeScript

```ts
function findSquare(matrix: number[][]): number[] {
    const n = matrix.length;
    const down: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const right: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = n - 1; i >= 0; --i) {
        for (let j = n - 1; j >= 0; --j) {
            if (matrix[i][j] === 0) {
                down[i][j] = i + 1 < n ? down[i + 1][j] + 1 : 1;
                right[i][j] = j + 1 < n ? right[i][j + 1] + 1 : 1;
            }
        }
    }
    for (let k = n; k > 0; --k) {
        for (let i = 0; i <= n - k; ++i) {
            for (let j = 0; j <= n - k; ++j) {
                if (
                    down[i][j] >= k &&
                    right[i][j] >= k &&
                    right[i + k - 1][j] >= k &&
                    down[i][j + k - 1] >= k
                ) {
                    return [i, j, k];
                }
            }
        }
    }
    return [];
}
```

#### Swift

```swift
class Solution {
    func findSquare(_ matrix: [[Int]]) -> [Int] {
        let n = matrix.count
        var down = Array(repeating: Array(repeating: 0, count: n), count: n)
        var right = Array(repeating: Array(repeating: 0, count: n), count: n)

        for i in stride(from: n - 1, through: 0, by: -1) {
            for j in stride(from: n - 1, through: 0, by: -1) {
                if matrix[i][j] == 0 {
                    down[i][j] = (i + 1 < n) ? down[i + 1][j] + 1 : 1
                    right[i][j] = (j + 1 < n) ? right[i][j + 1] + 1 : 1
                }
            }
        }

        for k in stride(from: n, through: 1, by: -1) {
            for i in 0...(n - k) {
                for j in 0...(n - k) {
                    if down[i][j] >= k && right[i][j] >= k &&
                       right[i + k - 1][j] >= k && down[i][j + k - 1] >= k {
                        return [i, j, k]
                    }
                }
            }
        }

        return []
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
