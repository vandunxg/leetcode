---
comments: true
difficulty: Hard
rating: 2241
source: Weekly Contest 160 Q4
tags:
    - Backtracking
---

<!-- problem:start -->

# [1240. Tiling a Rectangle with the Fewest Squares](https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares)

[中文文档](/solution/1200-1299/1240.Tiling%20a%20Rectangle%20with%20the%20Fewest%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hình chữ nhật kích thước <code>n</code> x <code>m</code>, hãy trả về <em>số hình vuông có cạnh nguyên ít nhất cần dùng để lát kín hình chữ nhật</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1240.Tiling%20a%20Rectangle%20with%20the%20Fewest%20Squares/images/sample_11_1592.png" style="width: 154px; height: 106px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 2, m = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cần <code>3</code> hình vuông để phủ kín hình chữ nhật.
<code>2</code> (hình vuông kích thước <code>1x1</code>)
<code>1</code> (hình vuông kích thước <code>2x2</code>)</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1240.Tiling%20a%20Rectangle%20with%20the%20Fewest%20Squares/images/sample_22_1592.png" style="width: 224px; height: 126px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 5, m = 8
<strong>Đầu ra:</strong> 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1240.Tiling%20a%20Rectangle%20with%20the%20Fewest%20Squares/images/sample_33_1592.png" style="width: 224px; height: 189px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 11, m = 13
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, m &lt;= 13</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Backtracking đệ quy + Nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lát hình chữ nhật $n\times m$ bằng ít hình vuông nhất. Vì $n,m \le 13$, trạng thái lấp đầy mỗi hàng có thể lưu bằng một bitmask. Có nhiều cách phân hoạch, nhưng số viên đã dùng giúp prune các nhánh.
>
> Duyệt theo thứ tự từng hàng để tìm ô trống đầu tiên, thử mọi độ dài cạnh khả thi, cập nhật $filled$ rồi đệ quy; phép XOR sẽ hoàn tác thay đổi. Dừng khi số viên đã dùng không thể tốt hơn đáp án hiện tại. Bitmask giúp kiểm tra ô đã lấp bằng thao tác bit, còn backtracking thử các cách lát hình chữ nhật.

<!-- thinking:end -->

Ta có thể backtracking đệ quy theo vị trí, dùng biến $t$ để ghi nhận số viên đã sử dụng.

- Nếu $j = m$, tức hàng thứ $i$ đã được lấp đầy, ta đệ quy sang hàng tiếp theo tại $(i + 1, 0)$.
- Nếu $i = n$, nghĩa là mọi vị trí đều đã được lấp đầy; ta cập nhật đáp án rồi trả về.
- Nếu vị trí hiện tại $(i, j)$ đã được lấp đầy, đệ quy thẳng đến vị trí kế tiếp $(i, j + 1)$.
- Nếu không, ta tìm độ dài cạnh hình vuông lớn nhất $w$ có thể đặt tại vị trí $(i, j)$, lấp tất cả vị trí từ $(i, j)$ đến $(i + w - 1, j + w - 1)$, rồi đệ quy đến vị trí tiếp theo $(i, j + w)$. Khi backtrack, cần xóa trạng thái đã lấp ở các vị trí từ $(i, j)$ đến $(i + w - 1, j + w - 1)$.

Vì mỗi vị trí chỉ có hai trạng thái: đã lấp hoặc chưa lấp, ta có thể dùng số nguyên để biểu diễn trạng thái hiện tại. Dùng mảng số nguyên $filled$ dài $n$, trong đó $filled[i]$ biểu diễn trạng thái của hàng thứ $i$. Nếu bit thứ $j$ của $filled[i]$ là $1$, ô tại hàng $i$, cột $j$ đã được lấp; nếu không thì ô đó chưa được lấp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tilingRectangle(self, n: int, m: int) -> int:
        def dfs(i: int, j: int, t: int):
            nonlocal ans
            if j == m:
                i += 1
                j = 0
            if i == n:
                ans = t
                return
            if filled[i] >> j & 1:
                dfs(i, j + 1, t)
            elif t + 1 < ans:
                r = c = 0
                for k in range(i, n):
                    if filled[k] >> j & 1:
                        break
                    r += 1
                for k in range(j, m):
                    if filled[i] >> k & 1:
                        break
                    c += 1
                mx = r if r < c else c
                for w in range(1, mx + 1):
                    for k in range(w):
                        filled[i + w - 1] |= 1 << (j + k)
                        filled[i + k] |= 1 << (j + w - 1)
                    dfs(i, j + w, t + 1)
                for x in range(i, i + mx):
                    for y in range(j, j + mx):
                        filled[x] ^= 1 << y

        ans = n * m
        filled = [0] * n
        dfs(0, 0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int m;
    private int[] filled;
    private int ans;

    public int tilingRectangle(int n, int m) {
        this.n = n;
        this.m = m;
        ans = n * m;
        filled = new int[n];
        dfs(0, 0, 0);
        return ans;
    }

    private void dfs(int i, int j, int t) {
        if (j == m) {
            ++i;
            j = 0;
        }
        if (i == n) {
            ans = t;
            return;
        }
        if ((filled[i] >> j & 1) == 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            int r = 0, c = 0;
            for (int k = i; k < n; ++k) {
                if ((filled[k] >> j & 1) == 1) {
                    break;
                }
                ++r;
            }
            for (int k = j; k < m; ++k) {
                if ((filled[i] >> k & 1) == 1) {
                    break;
                }
                ++c;
            }
            int mx = Math.min(r, c);
            for (int w = 1; w <= mx; ++w) {
                for (int k = 0; k < w; ++k) {
                    filled[i + w - 1] |= 1 << (j + k);
                    filled[i + k] |= 1 << (j + w - 1);
                }
                dfs(i, j + w, t + 1);
            }
            for (int x = i; x < i + mx; ++x) {
                for (int y = j; y < j + mx; ++y) {
                    filled[x] ^= 1 << y;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tilingRectangle(int n, int m) {
        memset(filled, 0, sizeof(filled));
        this->n = n;
        this->m = m;
        ans = n * m;
        dfs(0, 0, 0);
        return ans;
    }

private:
    int filled[13];
    int n, m;
    int ans;

    void dfs(int i, int j, int t) {
        if (j == m) {
            ++i;
            j = 0;
        }
        if (i == n) {
            ans = t;
            return;
        }
        if (filled[i] >> j & 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            int r = 0, c = 0;
            for (int k = i; k < n; ++k) {
                if (filled[k] >> j & 1) {
                    break;
                }
                ++r;
            }
            for (int k = j; k < m; ++k) {
                if (filled[i] >> k & 1) {
                    break;
                }
                ++c;
            }
            int mx = min(r, c);
            for (int w = 1; w <= mx; ++w) {
                for (int k = 0; k < w; ++k) {
                    filled[i + w - 1] |= 1 << (j + k);
                    filled[i + k] |= 1 << (j + w - 1);
                }
                dfs(i, j + w, t + 1);
            }
            for (int x = i; x < i + mx; ++x) {
                for (int y = j; y < j + mx; ++y) {
                    filled[x] ^= 1 << y;
                }
            }
        }
    }
};
```

#### Go

```go
func tilingRectangle(n int, m int) int {
	ans := n * m
	filled := make([]int, n)
	var dfs func(i, j, t int)
	dfs = func(i, j, t int) {
		if j == m {
			i++
			j = 0
		}
		if i == n {
			ans = t
			return
		}
		if filled[i]>>j&1 == 1 {
			dfs(i, j+1, t)
		} else if t+1 < ans {
			var r, c int
			for k := i; k < n; k++ {
				if filled[k]>>j&1 == 1 {
					break
				}
				r++
			}
			for k := j; k < m; k++ {
				if filled[i]>>k&1 == 1 {
					break
				}
				c++
			}
			mx := min(r, c)
			for w := 1; w <= mx; w++ {
				for k := 0; k < w; k++ {
					filled[i+w-1] |= 1 << (j + k)
					filled[i+k] |= 1 << (j + w - 1)
				}
				dfs(i, j+w, t+1)
			}
			for x := i; x < i+mx; x++ {
				for y := j; y < j+mx; y++ {
					filled[x] ^= 1 << y
				}
			}
		}
	}
	dfs(0, 0, 0)
	return ans
}
```

#### TypeScript

```ts
function tilingRectangle(n: number, m: number): number {
    let ans = n * m;
    const filled: number[] = new Array(n).fill(0);
    const dfs = (i: number, j: number, t: number) => {
        if (j === m) {
            ++i;
            j = 0;
        }
        if (i === n) {
            ans = t;
            return;
        }
        if ((filled[i] >> j) & 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            let [r, c] = [0, 0];
            for (let k = i; k < n; ++k) {
                if ((filled[k] >> j) & 1) {
                    break;
                }
                ++r;
            }
            for (let k = j; k < m; ++k) {
                if ((filled[i] >> k) & 1) {
                    break;
                }
                ++c;
            }
            const mx = Math.min(r, c);
            for (let w = 1; w <= mx; ++w) {
                for (let k = 0; k < w; ++k) {
                    filled[i + w - 1] |= 1 << (j + k);
                    filled[i + k] |= 1 << (j + w - 1);
                }
                dfs(i, j + w, t + 1);
            }
            for (let x = i; x < i + mx; ++x) {
                for (let y = j; y < j + mx; ++y) {
                    filled[x] ^= 1 << y;
                }
            }
        }
    };
    dfs(0, 0, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đặt hình vuông từ nhỏ đến lớn. Hình vuông lớn lấp phần trống nhanh hơn, giúp điều kiện prune dựa trên $ans$ kích hoạt sớm hơn. Lời giải 2 đặt hình vuông lớn nhất tại ô hiện tại trước, rồi thu nhỏ dần từ lớn xuống nhỏ. Các lựa chọn vẫn như nhau; chỉ thứ tự thử giúp prune hiệu quả hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tilingRectangle(self, n: int, m: int) -> int:
        def dfs(i: int, j: int, t: int):
            nonlocal ans
            if j == m:
                i += 1
                j = 0
            if i == n:
                ans = t
                return
            if filled[i] >> j & 1:
                dfs(i, j + 1, t)
            elif t + 1 < ans:
                r = c = 0
                for k in range(i, n):
                    if filled[k] >> j & 1:
                        break
                    r += 1
                for k in range(j, m):
                    if filled[i] >> k & 1:
                        break
                    c += 1
                mx = min(r, c)
                for x in range(i, i + mx):
                    for y in range(j, j + mx):
                        filled[x] |= 1 << y
                for w in range(mx, 0, -1):
                    dfs(i, j + w, t + 1)
                    for k in range(w):
                        filled[i + w - 1] ^= 1 << (j + k)
                        if k < w - 1:
                            filled[i + k] ^= 1 << (j + w - 1)

        ans = n * m
        filled = [0] * n
        dfs(0, 0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int m;
    private int[] filled;
    private int ans;

    public int tilingRectangle(int n, int m) {
        this.n = n;
        this.m = m;
        ans = n * m;
        filled = new int[n];
        dfs(0, 0, 0);
        return ans;
    }

    private void dfs(int i, int j, int t) {
        if (j == m) {
            ++i;
            j = 0;
        }
        if (i == n) {
            ans = t;
            return;
        }
        if ((filled[i] >> j & 1) == 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            int r = 0, c = 0;
            for (int k = i; k < n; ++k) {
                if ((filled[k] >> j & 1) == 1) {
                    break;
                }
                ++r;
            }
            for (int k = j; k < m; ++k) {
                if ((filled[i] >> k & 1) == 1) {
                    break;
                }
                ++c;
            }
            int mx = Math.min(r, c);
            for (int x = i; x < i + mx; ++x) {
                for (int y = j; y < j + mx; ++y) {
                    filled[x] |= 1 << y;
                }
            }
            for (int w = mx; w > 0; --w) {
                dfs(i, j + w, t + 1);
                for (int k = 0; k < w; ++k) {
                    filled[i + w - 1] ^= 1 << (j + k);
                    if (k < w - 1) {
                        filled[i + k] ^= 1 << (j + w - 1);
                    }
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int tilingRectangle(int n, int m) {
        memset(filled, 0, sizeof(filled));
        this->n = n;
        this->m = m;
        ans = n * m;
        dfs(0, 0, 0);
        return ans;
    }

private:
    int filled[13];
    int n, m;
    int ans;

    void dfs(int i, int j, int t) {
        if (j == m) {
            ++i;
            j = 0;
        }
        if (i == n) {
            ans = t;
            return;
        }
        if (filled[i] >> j & 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            int r = 0, c = 0;
            for (int k = i; k < n; ++k) {
                if (filled[k] >> j & 1) {
                    break;
                }
                ++r;
            }
            for (int k = j; k < m; ++k) {
                if (filled[i] >> k & 1) {
                    break;
                }
                ++c;
            }
            int mx = min(r, c);
            for (int x = i; x < i + mx; ++x) {
                for (int y = j; y < j + mx; ++y) {
                    filled[x] |= 1 << y;
                }
            }
            for (int w = mx; w; --w) {
                dfs(i, j + w, t + 1);
                for (int k = 0; k < w; ++k) {
                    filled[i + w - 1] ^= 1 << (j + k);
                    if (k < w - 1) {
                        filled[i + k] ^= 1 << (j + w - 1);
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func tilingRectangle(n int, m int) int {
	ans := n * m
	filled := make([]int, n)
	var dfs func(i, j, t int)
	dfs = func(i, j, t int) {
		if j == m {
			i++
			j = 0
		}
		if i == n {
			ans = t
			return
		}
		if filled[i]>>j&1 == 1 {
			dfs(i, j+1, t)
		} else if t+1 < ans {
			var r, c int
			for k := i; k < n; k++ {
				if filled[k]>>j&1 == 1 {
					break
				}
				r++
			}
			for k := j; k < m; k++ {
				if filled[i]>>k&1 == 1 {
					break
				}
				c++
			}
			mx := min(r, c)
			for x := i; x < i+mx; x++ {
				for y := j; y < j+mx; y++ {
					filled[x] |= 1 << y
				}
			}
			for w := mx; w > 0; w-- {
				dfs(i, j+w, t+1)
				for k := 0; k < w; k++ {
					filled[i+w-1] ^= 1 << (j + k)
					if k < w-1 {
						filled[i+k] ^= 1 << (j + w - 1)
					}
				}
			}
		}
	}
	dfs(0, 0, 0)
	return ans
}
```

#### TypeScript

```ts
function tilingRectangle(n: number, m: number): number {
    let ans = n * m;
    const filled: number[] = new Array(n).fill(0);
    const dfs = (i: number, j: number, t: number) => {
        if (j === m) {
            ++i;
            j = 0;
        }
        if (i === n) {
            ans = t;
            return;
        }
        if ((filled[i] >> j) & 1) {
            dfs(i, j + 1, t);
        } else if (t + 1 < ans) {
            let [r, c] = [0, 0];
            for (let k = i; k < n; ++k) {
                if ((filled[k] >> j) & 1) {
                    break;
                }
                ++r;
            }
            for (let k = j; k < m; ++k) {
                if ((filled[i] >> k) & 1) {
                    break;
                }
                ++c;
            }
            const mx = Math.min(r, c);
            for (let x = i; x < i + mx; ++x) {
                for (let y = j; y < j + mx; ++y) {
                    filled[x] |= 1 << y;
                }
            }
            for (let w = mx; w > 0; --w) {
                dfs(i, j + w, t + 1);
                for (let k = 0; k < w; ++k) {
                    filled[i + w - 1] ^= 1 << (j + k);
                    if (k < w - 1) {
                        filled[i + k] ^= 1 << (j + w - 1);
                    }
                }
            }
        }
    };
    dfs(0, 0, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
