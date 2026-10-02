---
comments: true
difficulty: Hard
tags:
    - Math
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop)

[中文文档](/solution/0800-0899/0887.Super%20Egg%20Drop/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>k</code> quả trứng giống hệt nhau và một tòa nhà có <code>n</code> tầng, được đánh số từ <code>1</code> đến <code>n</code>.</p>

<p>Bạn biết tồn tại một tầng <code>f</code>, với <code>0 &lt;= f &lt;= n</code>, sao cho mọi quả trứng thả từ tầng <strong>cao hơn</strong> <code>f</code> đều sẽ <strong>vỡ</strong>, còn trứng thả từ tầng <strong>bằng hoặc thấp hơn</strong> <code>f</code> thì <strong>không vỡ</strong>.</p>

<p>Trong mỗi lượt, bạn có thể lấy một quả trứng chưa vỡ và thả từ bất kỳ tầng nào <code>x</code> (với <code>1 &lt;= x &lt;= n</code>). Nếu trứng vỡ, bạn không thể dùng nó nữa. Nếu trứng không vỡ, bạn có thể <strong>dùng lại</strong> trong các lượt sau.</p>

<p>Hãy trả về <em><strong>số lượt ít nhất</strong> cần thực hiện để xác định <strong>chắc chắn</strong> giá trị của </em><code>f</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 1, n = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>
Thả trứng từ tầng 1. Nếu trứng vỡ, ta biết f = 0.
Nếu không, thả trứng từ tầng 2. Nếu trứng vỡ, ta biết f = 1.
Nếu trứng không vỡ, ta biết f = 2.
Vì vậy, cần ít nhất 2 lượt để xác định chắc chắn giá trị của f.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2, n = 6
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3, n = 14
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với $k$ quả trứng và $n$ tầng, cần tối thiểu hóa số lần thả trong trường hợp xấu nhất. Thử mọi tầng để thả với độ phức tạp bậc ba sẽ rất tốn kém. Với số trứng cố định, chi phí trường hợp xấu nhất đơn điệu theo tầng được chọn, nên tồn tại một tầng cân bằng hai bài toán con ứng với trứng vỡ và không vỡ.
>
> $dfs(i,j)$ biểu diễn bài toán với $i$ tầng và $j$ quả trứng. Dùng tìm kiếm nhị phân để chọn tầng thả bằng cách so sánh hai nhánh, sau đó cộng thêm một lượt. Trường hợp chỉ có một tầng hoặc một quả trứng có công thức trực tiếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i < 1:
                return 0
            if j == 1:
                return i
            l, r = 1, i
            while l < r:
                mid = (l + r + 1) >> 1
                a = dfs(mid - 1, j - 1)
                b = dfs(i - mid, j)
                if a <= b:
                    l = mid
                else:
                    r = mid - 1
            return max(dfs(l - 1, j - 1), dfs(i - l, j)) + 1

        return dfs(n, k)
```

#### Java

```java
class Solution {
    private int[][] f;

    public int superEggDrop(int k, int n) {
        f = new int[n + 1][k + 1];
        return dfs(n, k);
    }

    private int dfs(int i, int j) {
        if (i < 1) {
            return 0;
        }
        if (j == 1) {
            return i;
        }
        if (f[i][j] != 0) {
            return f[i][j];
        }
        int l = 1, r = i;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            int a = dfs(mid - 1, j - 1);
            int b = dfs(i - mid, j);
            if (a <= b) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return f[i][j] = Math.max(dfs(l - 1, j - 1), dfs(i - l, j)) + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int superEggDrop(int k, int n) {
        int f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i < 1) {
                return 0;
            }
            if (j == 1) {
                return i;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            int l = 1, r = i;
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                int a = dfs(mid - 1, j - 1);
                int b = dfs(i - mid, j);
                if (a <= b) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            return f[i][j] = max(dfs(l - 1, j - 1), dfs(i - l, j)) + 1;
        };
        return dfs(n, k);
    }
};
```

#### Go

```go
func superEggDrop(k int, n int) int {
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i < 1 {
			return 0
		}
		if j == 1 {
			return i
		}
		if f[i][j] != 0 {
			return f[i][j]
		}
		l, r := 1, i
		for l < r {
			mid := (l + r + 1) >> 1
			a, b := dfs(mid-1, j-1), dfs(i-mid, j)
			if a <= b {
				l = mid
			} else {
				r = mid - 1
			}
		}
		f[i][j] = max(dfs(l-1, j-1), dfs(i-l, j)) + 1
		return f[i][j]
	}
	return dfs(n, k)
}
```

#### TypeScript

```ts
function superEggDrop(k: number, n: number): number {
    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(k + 1).fill(0));
    const dfs = (i: number, j: number): number => {
        if (i < 1) {
            return 0;
        }
        if (j === 1) {
            return i;
        }
        if (f[i][j]) {
            return f[i][j];
        }
        let l = 1;
        let r = i;
        while (l < r) {
            const mid = (l + r + 1) >> 1;
            const a = dfs(mid - 1, j - 1);
            const b = dfs(i - mid, j);
            if (a <= b) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return (f[i][j] = Math.max(dfs(l - 1, j - 1), dfs(i - l, j)) + 1);
    };
    return dfs(n, k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Có thể dùng cùng công thức chuyển để điền $f[i][j]$ theo số tầng và số trứng mà không cần đệ quy. Với một quả trứng, ta có $f[i][1]=i$.
>
> Vòng lặp bên trong vẫn dùng tìm kiếm nhị phân để tìm tầng cân bằng. Đáp án là $f[n][k]$; độ phức tạp tiệm cận không đổi nhưng hằng số nhỏ hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        f = [[0] * (k + 1) for _ in range(n + 1)]
        for i in range(1, n + 1):
            f[i][1] = i
        for i in range(1, n + 1):
            for j in range(2, k + 1):
                l, r = 1, i
                while l < r:
                    mid = (l + r + 1) >> 1
                    a, b = f[mid - 1][j - 1], f[i - mid][j]
                    if a <= b:
                        l = mid
                    else:
                        r = mid - 1
                f[i][j] = max(f[l - 1][j - 1], f[i - l][j]) + 1
        return f[n][k]
```

#### Java

```java
class Solution {
    public int superEggDrop(int k, int n) {
        int[][] f = new int[n + 1][k + 1];
        for (int i = 1; i <= n; ++i) {
            f[i][1] = i;
        }
        for (int i = 1; i <= n; ++i) {
            for (int j = 2; j <= k; ++j) {
                int l = 1, r = i;
                while (l < r) {
                    int mid = (l + r + 1) >> 1;
                    int a = f[mid - 1][j - 1];
                    int b = f[i - mid][j];
                    if (a <= b) {
                        l = mid;
                    } else {
                        r = mid - 1;
                    }
                }
                f[i][j] = Math.max(f[l - 1][j - 1], f[i - l][j]) + 1;
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int superEggDrop(int k, int n) {
        int f[n + 1][k + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            f[i][1] = i;
        }
        for (int i = 1; i <= n; ++i) {
            for (int j = 2; j <= k; ++j) {
                int l = 1, r = i;
                while (l < r) {
                    int mid = (l + r + 1) >> 1;
                    int a = f[mid - 1][j - 1];
                    int b = f[i - mid][j];
                    if (a <= b) {
                        l = mid;
                    } else {
                        r = mid - 1;
                    }
                }
                f[i][j] = max(f[l - 1][j - 1], f[i - l][j]) + 1;
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func superEggDrop(k int, n int) int {
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	for i := 1; i <= n; i++ {
		f[i][1] = i
	}
	for i := 1; i <= n; i++ {
		for j := 2; j <= k; j++ {
			l, r := 1, i
			for l < r {
				mid := (l + r + 1) >> 1
				a, b := f[mid-1][j-1], f[i-mid][j]
				if a <= b {
					l = mid
				} else {
					r = mid - 1
				}
			}
			f[i][j] = max(f[l-1][j-1], f[i-l][j]) + 1
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function superEggDrop(k: number, n: number): number {
    const f: number[][] = new Array(n + 1).fill(0).map(() => new Array(k + 1).fill(0));
    for (let i = 1; i <= n; ++i) {
        f[i][1] = i;
    }
    for (let i = 1; i <= n; ++i) {
        for (let j = 2; j <= k; ++j) {
            let l = 1;
            let r = i;
            while (l < r) {
                const mid = (l + r + 1) >> 1;
                const a = f[mid - 1][j - 1];
                const b = f[i - mid][j];
                if (a <= b) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            f[i][j] = Math.max(f[l - 1][j - 1], f[i - l][j]) + 1;
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
