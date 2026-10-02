---
comments: true
difficulty: Hard
rating: 1786
source: Biweekly Contest 30 Q4
tags:
    - Minimax
    - Math
    - Dynamic Programming
    - Game Theory
    - Nim Game
    - 'Sprague–Grundy '
    - Zero-Sum Game
---

<!-- problem:start -->

# [1510. Stone Game IV](https://leetcode.com/problems/stone-game-iv)

[中文文档](/solution/1500-1599/1510.Stone%20Game%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, Alice đi trước.</p>

<p>Ban đầu có <code>n</code> viên đá trong một đống. Trong lượt của mình, mỗi người chơi thực hiện một <em>nước đi</em> bằng cách lấy đi <strong>một số lượng bất kỳ</strong> các viên đá trong đống, với số lượng đó là một <strong>số chính phương khác không</strong>.</p>

<p>Nếu một người chơi không thể thực hiện nước đi, người đó thua trò chơi.</p>

<p>Với số nguyên dương <code>n</code>, hãy trả về <code>true</code> khi và chỉ khi Alice thắng trò chơi, ngược lại trả về <code>false</code>, giả sử cả hai người chơi đều chơi tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> true
<strong>Explanation: </strong>Alice có thể lấy 1 viên đá và thắng vì Bob không còn nước đi nào.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> false
<strong>Explanation: </strong>Alice chỉ có thể lấy 1 viên đá, sau đó Bob lấy viên cuối cùng và thắng (2 -&gt; 1 -&gt; 0).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 4
<strong>Output:</strong> true
<strong>Explanation:</strong> n đã là một số chính phương, Alice có thể thắng chỉ với một nước đi, lấy 4 viên đá (4 -&gt; 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lần lượt lấy đi một số chính phương viên đá khỏi đống gồm $n$ viên; người lấy viên cuối cùng sẽ thắng. Vì $n\le 10^5$, việc mở rộng toàn bộ cây trò chơi sẽ tính lại cùng một số đá còn lại nhiều lần.
>
> Một trạng thái chỉ phụ thuộc vào số đá còn lại: đó là trạng thái thắng nếu có một nước đi khiến đối thủ rơi vào trạng thái thua. Hàm $dfs(i)$ có memoization xác định người chơi hiện tại có thắng khi còn $i$ viên đá hay không, bằng cách thử mọi $j^2\le i$. Mỗi trạng thái có $O(\sqrt{i})$ chuyển tiếp.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu diễn việc người chơi hiện tại có thể thắng khi trong đống còn $i$ viên đá hay không. Nếu người chơi hiện tại có thể thắng, hàm trả về $true$; ngược lại trả về $false$. Đáp án là $dfs(n)$.

Quá trình tính hàm $dfs(i)$ như sau:

- Nếu $i \leq 0$, người chơi hiện tại không thể thực hiện nước đi nào, nên người đó thua; trả về $false$;
- Nếu không, liệt kê số viên đá $j$ mà người chơi hiện tại có thể lấy, trong đó $j$ là một số chính phương. Nếu người chơi còn lại không thể thắng sau khi người chơi hiện tại lấy $j$ viên đá, người chơi hiện tại thắng và ta trả về $true$. Nếu mọi $j$ được liệt kê đều không thỏa điều kiện trên, người chơi hiện tại thua và ta trả về $false$.

Để tránh tính toán lặp lại, ta có thể dùng memoization, tức là dùng mảng $f$ để ghi lại kết quả tính của hàm $dfs(i)$.

Độ phức tạp thời gian là $O(n \times \sqrt{n})$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số viên đá trong đống.

<!-- tabs:start -->

#### Python3

```python
@cache
def dfs(i: int) -> bool:
    if i <= 0:
        return False
    k = isqrt(i)
    return any(not dfs(i - j * j) for j in range(1, k + 1))


class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        return dfs(n)
```

#### Java

```java
class Solution {
    private Boolean[] f;

    public boolean winnerSquareGame(int n) {
        f = new Boolean[n + 1];
        return dfs(n);
    }

    private boolean dfs(int i) {
        if (i <= 0) {
            return false;
        }
        if (f[i] != null) {
            return f[i];
        }
        int k = (int) Math.sqrt(i);
        for (int j = 1; j <= k; j++) {
            if (!dfs(i - j * j)) {
                return f[i] = true;
            }
        }
        return f[i] = false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool winnerSquareGame(int n) {
        vector<int> f(n + 1, -1);

        auto dfs = [&](this auto&& dfs, int i) -> bool {
            if (i <= 0) {
                return false;
            }
            if (f[i] != -1) {
                return f[i];
            }

            int k = sqrt(i);
            for (int j = 1; j <= k; j++) {
                if (!dfs(i - j * j)) {
                    return f[i] = true;
                }
            }

            return f[i] = false;
        };

        return dfs(n);
    }
};
```

#### Go

```go
func winnerSquareGame(n int) bool {
	f := make([]int8, n+1)

	var dfs func(int) bool
	dfs = func(i int) bool {
		if i <= 0 {
			return false
		}
		if f[i] != 0 {
			return f[i] == 1
		}
		k := int(math.Sqrt(float64(i)))
		for j := 1; j <= k; j++ {
			if !dfs(i - j*j) {
				f[i] = 1
				return true
			}
		}
		f[i] = -1
		return false
	}

	return dfs(n)
}
```

#### TypeScript

```ts
function winnerSquareGame(n: number): boolean {
    const f = new Array<number>(n + 1).fill(-1);

    const dfs = (i: number): boolean => {
        if (i <= 0) {
            return false;
        }
        if (f[i] !== -1) {
            return f[i] === 1;
        }

        const k = Math.floor(Math.sqrt(i));
        for (let j = 1; j <= k; j++) {
            if (!dfs(i - j * j)) {
                f[i] = 1;
                return true;
            }
        }

        f[i] = 0;
        return false;
    };

    return dfs(n);
}
```

#### Rust

```rust
impl Solution {
    pub fn winner_square_game(n: i32) -> bool {
        let mut f = vec![-1; (n + 1) as usize];

        fn dfs(i: i32, f: &mut Vec<i8>) -> bool {
            if i <= 0 {
                return false;
            }

            let idx = i as usize;
            if f[idx] != -1 {
                return f[idx] == 1;
            }

            let k = (i as f64).sqrt() as i32;
            for j in 1..=k {
                if !dfs(i - j * j, f) {
                    f[idx] = 1;
                    return true;
                }
            }

            f[idx] = 0;
            false
        }

        dfs(n, &mut f)
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
> Memoization vẫn chịu chi phí đệ quy và cache, trong khi chi phí tiệm cận giống với việc xây dựng bảng từ dưới lên. Gọi $f[i]$ là việc trạng thái có $i$ viên đá có thắng hay không, rồi tính $i$ theo thứ tự tăng dần. Chỉ cần một số chính phương đưa đến trạng thái thua là đủ để $f[i]$ nhận giá trị true. Cách cài đặt chỉ dùng vòng lặp và không cần call stack.

<!-- thinking:end -->

Ta cũng có thể dùng dynamic programming để giải bài toán này.

Định nghĩa mảng $f$, trong đó $f[i]$ biểu diễn việc người chơi hiện tại có thể thắng khi trong đống có $i$ viên đá hay không. Nếu người chơi hiện tại có thể thắng thì $f[i]$ là $true$, ngược lại là $false$. Đáp án là $f[n]$.

Ta duyệt $i$ trong khoảng $[1,..n]$, và duyệt $j$ trong khoảng $[1,..i]$, trong đó $j$ là một số chính phương. Nếu người chơi còn lại không thể thắng sau khi người chơi hiện tại lấy $j$ viên đá, người chơi hiện tại thắng, tức là $f[i] = true$. Nếu mọi $j$ được duyệt đều không thỏa điều kiện trên, người chơi hiện tại thua, tức là $f[i] = false$. Do đó, ta có công thức chuyển trạng thái:

$$
f[i]=
\begin{cases}
true, & \textit{if } \exists j \in [1,..i], j^2 \leq i \textit{ and } f[i-j^2] = false\\
false, & \textit{otherwise}
\end{cases}
$$

Cuối cùng, ta trả về $f[n]$.

Độ phức tạp thời gian là $O(n \times \sqrt{n})$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số viên đá trong đống.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        f = [False] * (n + 1)
        for i in range(1, n + 1):
            j = 1
            while j <= i // j:
                if not f[i - j * j]:
                    f[i] = True
                    break
                j += 1
        return f[n]
```

#### Java

```java
class Solution {
    public boolean winnerSquareGame(int n) {
        boolean[] f = new boolean[n + 1];
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= i / j; ++j) {
                if (!f[i - j * j]) {
                    f[i] = true;
                    break;
                }
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool winnerSquareGame(int n) {
        bool f[n + 1];
        memset(f, false, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= i / j; ++j) {
                if (!f[i - j * j]) {
                    f[i] = true;
                    break;
                }
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func winnerSquareGame(n int) bool {
	f := make([]bool, n+1)
	for i := 1; i <= n; i++ {
		for j := 1; j <= i/j; j++ {
			if !f[i-j*j] {
				f[i] = true
				break
			}
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function winnerSquareGame(n: number): boolean {
    const f: boolean[] = new Array(n + 1).fill(false);
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j * j <= i; ++j) {
            if (!f[i - j * j]) {
                f[i] = true;
                break;
            }
        }
    }
    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn winner_square_game(n: i32) -> bool {
        let n = n as usize;
        let mut f = vec![false; n + 1];

        for i in 1..=n {
            let mut j = 1;
            while j <= i / j {
                if !f[i - j * j] {
                    f[i] = true;
                    break;
                }
                j += 1;
            }
        }

        f[n]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
