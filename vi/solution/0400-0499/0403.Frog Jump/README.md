---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [403. Frog Jump](https://leetcode.com/problems/frog-jump)

[中文文档](/solution/0400-0499/0403.Frog%20Jump/README.md)

## Mô tả

<!-- description:start -->

<p>Một con ếch đang băng qua sông. Sông được chia thành các đơn vị độ dài bằng nhau, mỗi vị trí có thể có hoặc không có hòn đá. Ếch có thể nhảy lên đá nhưng không được rơi xuống nước.</p>

<p>Cho danh sách vị trí <code>stones</code> của các hòn đá (tính theo đơn vị độ dài), được sắp xếp theo thứ tự <strong>tăng dần</strong>. Hãy xác định ếch có thể băng qua sông bằng cách đáp xuống hòn đá cuối cùng hay không. Ban đầu, ếch đứng trên hòn đá đầu tiên và cú nhảy đầu tiên phải dài <code>1</code> đơn vị.</p>

<p>Nếu cú nhảy trước đó dài <code>k</code> đơn vị, cú nhảy tiếp theo phải dài <code>k - 1</code>, <code>k</code> hoặc <code>k + 1</code> đơn vị. Ếch chỉ có thể nhảy về phía trước.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [0,1,3,5,6,8,12,17]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ếch có thể tới hòn đá cuối cùng bằng cách nhảy 1 đơn vị đến hòn đá thứ 2, rồi nhảy 2 đơn vị đến hòn đá thứ 3, tiếp tục nhảy 2 đơn vị đến hòn đá thứ 4, sau đó nhảy 3 đơn vị đến hòn đá thứ 6, 4 đơn vị đến hòn đá thứ 7 và 5 đơn vị đến hòn đá thứ 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [0,1,2,3,4,8,9,11]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có cách nào nhảy đến hòn đá cuối cùng vì khoảng cách giữa hòn đá thứ 5 và thứ 6 quá lớn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= stones.length &lt;= 2000</code></li>
	<li><code>0 &lt;= stones[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>stones[0] == 0</code></li>
	<li><code>stones</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Thử các cú nhảy dài $k-1,k,k+1$ từ mỗi hòn đá có thể gặp lại cùng trạng thái (hòn đá, độ dài cú nhảy trước). Với $n\le 1100$, cặp $(i,k)$ có tối đa $O(n^2)$ trạng thái.
>
> Ánh xạ vị trí sang chỉ số, memoize $dfs(i,k)$ và chỉ xét các vị trí có hòn đá. Hash table giúp kiểm tra vị trí đáp xuống trong $O(1)$.

<!-- thinking:end -->

Dùng hash table $pos$ để lưu chỉ số của mỗi hòn đá. Tiếp theo, định nghĩa hàm $dfs(i, k)$, biểu thị việc ếch đang ở hòn đá thứ $i$ và cú nhảy trước dài $k$. Hàm trả về `true` nếu ếch có thể tới đích, nếu không thì trả về `false`.

Hàm $dfs(i, k)$ hoạt động như sau:

Nếu $i$ là chỉ số của hòn đá cuối cùng, ếch đã tới đích nên trả về `true`;

Nếu không, liệt kê độ dài cú nhảy tiếp theo $j$, với $j \in [k-1, k, k+1]$. Nếu $j$ là số nguyên dương và vị trí $stones[i] + j$ có trong hash table $pos$, ếch có thể nhảy $j$ đơn vị từ hòn đá thứ $i$. Nếu $dfs(pos[stones[i] + j], j)$ trả về `true`, ếch có thể tới đích từ hòn đá thứ $i$, khi đó trả về `true`.

Nếu đã xét hết các lựa chọn mà ếch vẫn không thể chọn cú nhảy phù hợp từ hòn đá thứ $i$ để tới đích, trả về `false`.

Để tránh tính lại $dfs(i, k)$, dùng memoization lưu kết quả của hàm trong mảng $f$. Mỗi lần $dfs(i, k)$ trả về kết quả, gán kết quả đó cho $f[i][k]$; nếu gặp lại trạng thái này thì trả về ngay $f[i][k]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số hòn đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canCross(self, stones: List[int]) -> bool:
        @cache
        def dfs(i, k):
            if i == n - 1:
                return True
            for j in range(k - 1, k + 2):
                if j > 0 and stones[i] + j in pos and dfs(pos[stones[i] + j], j):
                    return True
            return False

        n = len(stones)
        pos = {s: i for i, s in enumerate(stones)}
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Boolean[][] f;
    private Map<Integer, Integer> pos = new HashMap<>();
    private int[] stones;
    private int n;

    public boolean canCross(int[] stones) {
        n = stones.length;
        f = new Boolean[n][n];
        this.stones = stones;
        for (int i = 0; i < n; ++i) {
            pos.put(stones[i], i);
        }
        return dfs(0, 0);
    }

    private boolean dfs(int i, int k) {
        if (i == n - 1) {
            return true;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        for (int j = k - 1; j <= k + 1; ++j) {
            if (j > 0) {
                int h = stones[i] + j;
                if (pos.containsKey(h) && dfs(pos.get(h), j)) {
                    return f[i][k] = true;
                }
            }
        }
        return f[i][k] = false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        int f[n][n];
        memset(f, -1, sizeof(f));
        unordered_map<int, int> pos;
        for (int i = 0; i < n; ++i) {
            pos[stones[i]] = i;
        }
        function<bool(int, int)> dfs = [&](int i, int k) -> bool {
            if (i == n - 1) {
                return true;
            }
            if (f[i][k] != -1) {
                return f[i][k];
            }
            for (int j = k - 1; j <= k + 1; ++j) {
                if (j > 0 && pos.count(stones[i] + j) && dfs(pos[stones[i] + j], j)) {
                    return f[i][k] = true;
                }
            }
            return f[i][k] = false;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func canCross(stones []int) bool {
	n := len(stones)
	f := make([][]int, n)
	pos := map[int]int{}
	for i := range f {
		pos[stones[i]] = i
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int) bool
	dfs = func(i, k int) bool {
		if i == n-1 {
			return true
		}
		if f[i][k] != -1 {
			return f[i][k] == 1
		}
		for j := k - 1; j <= k+1; j++ {
			if j > 0 {
				if p, ok := pos[stones[i]+j]; ok {
					if dfs(p, j) {
						f[i][k] = 1
						return true
					}
				}
			}
		}
		f[i][k] = 0
		return false
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function canCross(stones: number[]): boolean {
    const n = stones.length;
    const pos: Map<number, number> = new Map();
    for (let i = 0; i < n; ++i) {
        pos.set(stones[i], i);
    }
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(-1));
    const dfs = (i: number, k: number): boolean => {
        if (i === n - 1) {
            return true;
        }
        if (f[i][k] !== -1) {
            return f[i][k] === 1;
        }
        for (let j = k - 1; j <= k + 1; ++j) {
            if (j > 0 && pos.has(stones[i] + j)) {
                if (dfs(pos.get(stones[i] + j)!, j)) {
                    f[i][k] = 1;
                    return true;
                }
            }
        }
        f[i][k] = 0;
        return false;
    };
    return dfs(0, 0);
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    #[allow(dead_code)]
    pub fn can_cross(stones: Vec<i32>) -> bool {
        let n = stones.len();
        let mut record = vec![vec![-1; n]; n];
        let mut pos = HashMap::new();
        for (i, &s) in stones.iter().enumerate() {
            pos.insert(s, i);
        }

        Self::dfs(&mut record, 0, 0, n, &pos, &stones)
    }

    #[allow(dead_code)]
    fn dfs(
        record: &mut Vec<Vec<i32>>,
        i: usize,
        k: usize,
        n: usize,
        pos: &HashMap<i32, usize>,
        stones: &Vec<i32>,
    ) -> bool {
        if i == n - 1 {
            return true;
        }

        if record[i][k] != -1 {
            return record[i][k] == 1;
        }

        let k = k as i32;
        for j in k - 1..=k + 1 {
            if j > 0
                && pos.contains_key(&(stones[i] + j))
                && Self::dfs(record, pos[&(stones[i] + j)], j as usize, n, pos, stones)
            {
                record[i][k as usize] = 1;
                return true;
            }
        }

        record[i][k as usize] = 0;
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm đệ quy có thể tạo call stack sâu. Đặt $f[i][k]$ biểu thị có thể đến hòn đá $i$ với cú nhảy trước dài $k$; trạng thái được chuyển từ một hòn đá trước đó $j$ sao cho $k=\textit{stones}[i]-\textit{stones}[j]$.
>
> Nếu $k-1>j$, các hòn đá xa hơn không thể tạo ra cú nhảy cần thiết, nên có thể dừng vòng lặp bên trong. Điền bảng theo thứ tự bottom-up giúp loại bỏ đệ quy.

<!-- thinking:end -->

Định nghĩa $f[i][k]$ là true khi và chỉ khi có thể đến hòn đá $i$ với cú nhảy trước dài $k$. Ban đầu, $f[0][0] = true$ và mọi phần tử khác của $f$ đều là false.

Có thể tính $f[i][k]$ cho mọi $i$ và $k$ bằng hai vòng lặp. Với mỗi độ dài cú nhảy khả dĩ $k$, xét các hòn đá có thể là điểm xuất phát của cú nhảy đó: $i-k$, $i-k+1$, $i-k+2$. Nếu một trong các hòn đá này tồn tại và có thể đến đó với cú nhảy trước dài $k-1$, $k$ hoặc $k+1$, thì ta có thể đến hòn đá $i$ với cú nhảy dài $k$.

Nếu có thể đến hòn đá cuối cùng thì đáp án là true; nếu không thì đáp án là false.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số hòn đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canCross(self, stones: List[int]) -> bool:
        n = len(stones)
        f = [[False] * n for _ in range(n)]
        f[0][0] = True
        for i in range(1, n):
            for j in range(i - 1, -1, -1):
                k = stones[i] - stones[j]
                if k - 1 > j:
                    break
                f[i][k] = f[j][k - 1] or f[j][k] or f[j][k + 1]
                if i == n - 1 and f[i][k]:
                    return True
        return False
```

#### Java

```java
class Solution {
    public boolean canCross(int[] stones) {
        int n = stones.length;
        boolean[][] f = new boolean[n][n];
        f[0][0] = true;
        for (int i = 1; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                int k = stones[i] - stones[j];
                if (k - 1 > j) {
                    break;
                }
                f[i][k] = f[j][k - 1] || f[j][k] || f[j][k + 1];
                if (i == n - 1 && f[i][k]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        bool f[n][n];
        memset(f, false, sizeof(f));
        f[0][0] = true;
        for (int i = 1; i < n; ++i) {
            for (int j = i - 1; j >= 0; --j) {
                int k = stones[i] - stones[j];
                if (k - 1 > j) {
                    break;
                }
                f[i][k] = f[j][k - 1] || f[j][k] || f[j][k + 1];
                if (i == n - 1 && f[i][k]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func canCross(stones []int) bool {
	n := len(stones)
	f := make([][]bool, n)
	for i := range f {
		f[i] = make([]bool, n)
	}
	f[0][0] = true
	for i := 1; i < n; i++ {
		for j := i - 1; j >= 0; j-- {
			k := stones[i] - stones[j]
			if k-1 > j {
				break
			}
			f[i][k] = f[j][k-1] || f[j][k] || f[j][k+1]
			if i == n-1 && f[i][k] {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function canCross(stones: number[]): boolean {
    const n = stones.length;
    const f: boolean[][] = new Array(n).fill(0).map(() => new Array(n).fill(false));
    f[0][0] = true;
    for (let i = 1; i < n; ++i) {
        for (let j = i - 1; j >= 0; --j) {
            const k = stones[i] - stones[j];
            if (k - 1 > j) {
                break;
            }
            f[i][k] = f[j][k - 1] || f[j][k] || f[j][k + 1];
            if (i == n - 1 && f[i][k]) {
                return true;
            }
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn can_cross(stones: Vec<i32>) -> bool {
        let n = stones.len();
        let mut dp = vec![vec![false; n]; n];

        // Initialize the dp vector
        dp[0][0] = true;

        // Begin the actual dp process
        for i in 1..n {
            for j in (0..=i - 1).rev() {
                let k = (stones[i] - stones[j]) as usize;
                if k - 1 > j {
                    break;
                }
                dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
                if i == n - 1 && dp[i][k] {
                    return true;
                }
            }
        }

        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
