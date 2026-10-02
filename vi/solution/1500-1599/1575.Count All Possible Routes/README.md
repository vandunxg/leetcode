---
comments: true
difficulty: Hard
rating: 2055
source: Biweekly Contest 34 Q4
tags:
    - Memoization
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1575. Count All Possible Routes](https://leetcode.com/problems/count-all-possible-routes)

[中文文档](/solution/1500-1599/1575.Count%20All%20Possible%20Routes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <strong>phân biệt</strong> locations, trong đó <code>locations[i]</code> là vị trí của thành phố <code>i</code>. Bạn cũng được cho các số nguyên <code>start</code>, <code>finish</code> và <code>fuel</code>, lần lượt biểu thị thành phố bắt đầu, thành phố kết thúc và lượng nhiên liệu ban đầu.</p>

<p>Ở mỗi bước, nếu đang ở thành phố <code>i</code>, bạn có thể chọn bất kỳ thành phố <code>j</code> nào thỏa <code>j != i</code> và <code>0 &lt;= j &lt; locations.length</code> để di chuyển đến thành phố <code>j</code>. Di chuyển từ thành phố <code>i</code> đến thành phố <code>j</code> làm giảm nhiên liệu <code>|locations[i] - locations[j]|</code>. Lưu ý rằng <code>|x|</code> là giá trị tuyệt đối của <code>x</code>.</p>

<p>Lưu ý rằng <code>fuel</code> <strong>không thể</strong> âm tại bất kỳ thời điểm nào, và bạn <strong>được phép</strong> ghé thăm một thành phố nhiều lần (bao gồm <code>start</code> và <code>finish</code>).</p>

<p>Trả về <em>số lượng mọi lộ trình có thể đi từ </em><code>start</code> <em>đến</em> <code>finish</code>. Vì đáp án có thể rất lớn, hãy trả về phần dư khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> locations = [2,3,6,8,4], start = 1, finish = 3, fuel = 5
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các lộ trình sau là toàn bộ lộ trình có thể đi, mỗi lộ trình dùng 5 đơn vị nhiên liệu:
1 -&gt; 3
1 -&gt; 2 -&gt; 3
1 -&gt; 4 -&gt; 3
1 -&gt; 4 -&gt; 2 -&gt; 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> locations = [4,3,1], start = 1, finish = 0, fuel = 6
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các lộ trình sau là toàn bộ lộ trình có thể đi:
1 -&gt; 0, nhiên liệu đã dùng = 1
1 -&gt; 2 -&gt; 0, nhiên liệu đã dùng = 5
1 -&gt; 2 -&gt; 1 -&gt; 0, nhiên liệu đã dùng = 5
1 -&gt; 0 -&gt; 1 -&gt; 0, nhiên liệu đã dùng = 3
1 -&gt; 0 -&gt; 1 -&gt; 0 -&gt; 1 -&gt; 0, nhiên liệu đã dùng = 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> locations = [5,2,1], start = 0, finish = 2, fuel = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể đi từ 0 đến 2 chỉ với 3 đơn vị nhiên liệu vì lộ trình ngắn nhất cần 4 đơn vị nhiên liệu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= locations.length &lt;= 100</code></li>
	<li><code>1 &lt;= locations[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả số nguyên trong <code>locations</code> đều <strong>phân biệt</strong>.</li>
	<li><code>0 &lt;= start, finish &lt; locations.length</code></li>
	<li><code>1 &lt;= fuel &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các đường đi từ $start$ đến $finish$ dùng không quá $fuel$. Số thành phố và nhiên liệu không lớn, nhưng một đường đi có thể ghé lại thành phố, nên ta không thể liệt kê các đường đi đơn.
>
> Một trạng thái gồm thành phố hiện tại $i$ và nhiên liệu còn lại $k$. Nếu $k$ không đủ để đến finish thì số cách là 0; ngược lại, cộng một nếu $i$ đã là finish, rồi đệ quy đến mọi thành phố khác với khoảng cách đã tiêu tốn. Memoization lưu $O(n\cdot fuel)$ trạng thái.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, k)$ biểu thị số đường đi từ thành phố $i$ với $k$ nhiên liệu còn lại đến đích $finish$. Vì vậy đáp án là $dfs(start, fuel)$.

Quá trình tính hàm $dfs(i, k)$ như sau:

- Nếu $k \lt |locations[i] - locations[finish]|$, trả về $0$.
- Nếu $i = finish$, ban đầu số đường đi là $1$, ngược lại là $0$.
- Sau đó, ta duyệt mọi thành phố $j$. Nếu $j \ne i$, ta có thể đi từ thành phố $i$ đến thành phố $j$ và nhiên liệu còn lại là $k - |locations[i] - locations[j]|$. Ta cộng số đường đi $dfs(j, k - |locations[i] - locations[j]|)$ vào đáp án.
- Cuối cùng, trả về số đường đi.

Để tránh tính toán lặp lại, ta dùng memoization.

Độ phức tạp thời gian là $O(n^2 \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là kích thước của mảng $locations$ và $fuel$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countRoutes(
        self, locations: List[int], start: int, finish: int, fuel: int
    ) -> int:
        @cache
        def dfs(i: int, k: int) -> int:
            if k < abs(locations[i] - locations[finish]):
                return 0
            ans = int(i == finish)
            for j, x in enumerate(locations):
                if j != i:
                    ans = (ans + dfs(j, k - abs(locations[i] - x))) % mod
            return ans

        mod = 10**9 + 7
        return dfs(start, fuel)
```

#### Java

```java
class Solution {
    private int[] locations;
    private int finish;
    private int n;
    private Integer[][] f;
    private final int mod = (int) 1e9 + 7;

    public int countRoutes(int[] locations, int start, int finish, int fuel) {
        n = locations.length;
        this.locations = locations;
        this.finish = finish;
        f = new Integer[n][fuel + 1];
        return dfs(start, fuel);
    }

    private int dfs(int i, int k) {
        if (k < Math.abs(locations[i] - locations[finish])) {
            return 0;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        int ans = i == finish ? 1 : 0;
        for (int j = 0; j < n; ++j) {
            if (j != i) {
                ans = (ans + dfs(j, k - Math.abs(locations[i] - locations[j]))) % mod;
            }
        }
        return f[i][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countRoutes(vector<int>& locations, int start, int finish, int fuel) {
        int n = locations.size();
        int f[n][fuel + 1];
        memset(f, -1, sizeof(f));
        const int mod = 1e9 + 7;
        function<int(int, int)> dfs = [&](int i, int k) -> int {
            if (k < abs(locations[i] - locations[finish])) {
                return 0;
            }
            if (f[i][k] != -1) {
                return f[i][k];
            }
            int ans = i == finish;
            for (int j = 0; j < n; ++j) {
                if (j != i) {
                    ans = (ans + dfs(j, k - abs(locations[i] - locations[j]))) % mod;
                }
            }
            return f[i][k] = ans;
        };
        return dfs(start, fuel);
    }
};
```

#### Go

```go
func countRoutes(locations []int, start int, finish int, fuel int) int {
	n := len(locations)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, fuel+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	const mod = 1e9 + 7
	var dfs func(int, int) int
	dfs = func(i, k int) (ans int) {
		if k < abs(locations[i]-locations[finish]) {
			return 0
		}
		if f[i][k] != -1 {
			return f[i][k]
		}
		if i == finish {
			ans = 1
		}
		for j, x := range locations {
			if j != i {
				ans = (ans + dfs(j, k-abs(locations[i]-x))) % mod
			}
		}
		f[i][k] = ans
		return
	}
	return dfs(start, fuel)
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
function countRoutes(locations: number[], start: number, finish: number, fuel: number): number {
    const n = locations.length;
    const f = Array.from({ length: n }, () => Array(fuel + 1).fill(-1));
    const mod = 1e9 + 7;
    const dfs = (i: number, k: number): number => {
        if (k < Math.abs(locations[i] - locations[finish])) {
            return 0;
        }
        if (f[i][k] !== -1) {
            return f[i][k];
        }
        let ans = i === finish ? 1 : 0;
        for (let j = 0; j < n; ++j) {
            if (j !== i) {
                const x = Math.abs(locations[i] - locations[j]);
                ans = (ans + dfs(j, k - x)) % mod;
            }
        }
        return (f[i][k] = ans);
    };
    return dfs(start, fuel);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization mở rộng trạng thái khi cần và vẫn dùng stack đệ quy có độ sâu bị giới hạn bởi fuel. Ta có thể dùng cùng công thức truy hồi để điền bảng theo $k$ tăng dần: $f[i][k]$ là số đường đi từ $i$ với $k$ nhiên liệu, cột finish bắt đầu bằng $1$, và ta cộng các chuyển trạng thái có chi phí không vượt quá $k$. Cách triển khai lặp này có cùng độ phức tạp.

<!-- thinking:end -->

Ta cũng có thể chuyển memoization của Lời giải 1 thành quy hoạch động.

Ta định nghĩa $f[i][k]$ là số đường đi từ thành phố $i$ với $k$ nhiên liệu còn lại đến đích $finish$. Vì vậy đáp án là $f[start][fuel]$. Ban đầu $f[finish][k]=1$, các giá trị khác bằng $0$.

Tiếp theo, ta duyệt nhiên liệu còn lại $k$ từ nhỏ đến lớn, rồi duyệt mọi thành phố $i$. Với mỗi thành phố $i$, ta duyệt mọi thành phố $j$. Nếu $j \ne i$ và $|locations[i] - locations[j]| \le k$, ta có thể đi từ $i$ đến $j$ với nhiên liệu còn lại $k - |locations[i] - locations[j]|$. Khi đó ta cộng số đường đi $f[j][k - |locations[i] - locations[j]|]$ vào đáp án.

Cuối cùng, ta trả về số đường đi $f[start][fuel]$.

Độ phức tạp thời gian là $O(n^2 \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là kích thước của mảng $locations$ và $fuel$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countRoutes(
        self, locations: List[int], start: int, finish: int, fuel: int
    ) -> int:
        mod = 10**9 + 7
        n = len(locations)
        f = [[0] * (fuel + 1) for _ in range(n)]
        for k in range(fuel + 1):
            f[finish][k] = 1
        for k in range(fuel + 1):
            for i in range(n):
                for j in range(n):
                    if j != i and abs(locations[i] - locations[j]) <= k:
                        f[i][k] = (
                            f[i][k] + f[j][k - abs(locations[i] - locations[j])]
                        ) % mod
        return f[start][fuel]
```

#### Java

```java
class Solution {
    public int countRoutes(int[] locations, int start, int finish, int fuel) {
        final int mod = (int) 1e9 + 7;
        int n = locations.length;
        int[][] f = new int[n][fuel + 1];
        for (int k = 0; k <= fuel; ++k) {
            f[finish][k] = 1;
        }
        for (int k = 0; k <= fuel; ++k) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (j != i && Math.abs(locations[i] - locations[j]) <= k) {
                        f[i][k] = (f[i][k] + f[j][k - Math.abs(locations[i] - locations[j])]) % mod;
                    }
                }
            }
        }
        return f[start][fuel];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countRoutes(vector<int>& locations, int start, int finish, int fuel) {
        const int mod = 1e9 + 7;
        int n = locations.size();
        int f[n][fuel + 1];
        memset(f, 0, sizeof(f));
        for (int k = 0; k <= fuel; ++k) {
            f[finish][k] = 1;
        }
        for (int k = 0; k <= fuel; ++k) {
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    if (j != i && abs(locations[i] - locations[j]) <= k) {
                        f[i][k] = (f[i][k] + f[j][k - abs(locations[i] - locations[j])]) % mod;
                    }
                }
            }
        }
        return f[start][fuel];
    }
};
```

#### Go

```go
func countRoutes(locations []int, start int, finish int, fuel int) int {
	n := len(locations)
	const mod = 1e9 + 7
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, fuel+1)
	}
	for k := 0; k <= fuel; k++ {
		f[finish][k] = 1
	}
	for k := 0; k <= fuel; k++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				if j != i && abs(locations[i]-locations[j]) <= k {
					f[i][k] = (f[i][k] + f[j][k-abs(locations[i]-locations[j])]) % mod
				}
			}
		}
	}
	return f[start][fuel]
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
function countRoutes(locations: number[], start: number, finish: number, fuel: number): number {
    const n = locations.length;
    const f = Array.from({ length: n }, () => Array(fuel + 1).fill(0));
    for (let k = 0; k <= fuel; ++k) {
        f[finish][k] = 1;
    }
    const mod = 1e9 + 7;
    for (let k = 0; k <= fuel; ++k) {
        for (let i = 0; i < n; ++i) {
            for (let j = 0; j < n; ++j) {
                if (j !== i && Math.abs(locations[i] - locations[j]) <= k) {
                    f[i][k] = (f[i][k] + f[j][k - Math.abs(locations[i] - locations[j])]) % mod;
                }
            }
        }
    }
    return f[start][fuel];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
