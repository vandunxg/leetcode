---
comments: true
difficulty: Hard
rating: 2026
source: Weekly Contest 183 Q4
tags:
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Zero-Sum Game
---

<!-- problem:start -->

# [1406. Stone Game III](https://leetcode.com/problems/stone-game-iii)

[中文文档](/solution/1400-1499/1406.Stone%20Game%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob tiếp tục chơi trò chơi với các đống đá. Có một số viên đá được <strong>xếp thành một hàng</strong>, mỗi viên có một giá trị là số nguyên trong mảng <code>stoneValue</code>.</p>

<p>Alice và Bob lần lượt chơi, Alice đi trước. Trong lượt của mình, mỗi người có thể lấy <code>1</code>, <code>2</code> hoặc <code>3</code> viên đá từ những viên <strong>đầu tiên</strong> còn lại trong hàng.</p>

<p>Điểm của mỗi người là tổng giá trị các viên đá đã lấy. Ban đầu điểm của mỗi người là <code>0</code>.</p>

<p>Mục tiêu của trò chơi là kết thúc với điểm cao nhất; người chơi có điểm cao hơn sẽ thắng và hai người có thể hòa. Trò chơi tiếp tục cho đến khi tất cả các viên đá đã được lấy.</p>

<p>Giả sử Alice và Bob đều <strong>chơi tối ưu</strong>.</p>

<p>Trả về <code>&quot;Alice&quot;</code><em> nếu Alice thắng, </em><code>&quot;Bob&quot;</code><em> nếu Bob thắng, hoặc </em><code>&quot;Tie&quot;</code><em> nếu họ kết thúc trò chơi với cùng số điểm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stoneValue = [1,2,3,7]
<strong>Đầu ra:</strong> &quot;Bob&quot;
<strong>Giải thích:</strong> Alice chắc chắn sẽ thua. Nước đi tốt nhất của cô ấy là lấy ba đống đá và được 6 điểm. Khi đó Bob có 7 điểm và thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stoneValue = [1,2,3,-9]
<strong>Đầu ra:</strong> &quot;Alice&quot;
<strong>Giải thích:</strong> Alice phải chọn cả ba đống đá ngay ở nước đầu tiên để thắng và khiến Bob có điểm âm.
Nếu Alice chọn một đống, cô ấy được 1 điểm và ở lượt tiếp theo Bob sẽ có 5 điểm. Ở lượt sau nữa, Alice sẽ lấy đống có giá trị = -9 và thua.
Nếu Alice chọn hai đống, cô ấy được 3 điểm và ở lượt tiếp theo Bob cũng có 3 điểm. Ở lượt sau nữa, Alice sẽ lấy đống có giá trị = -9 và cũng thua.
Hãy nhớ rằng cả hai đều chơi tối ưu, nên Alice sẽ chọn tình huống giúp cô ấy thắng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stoneValue = [1,2,3,6]
<strong>Đầu ra:</strong> &quot;Tie&quot;
<strong>Giải thích:</strong> Alice không thể thắng trò chơi này. Cô ấy có thể kết thúc với kết quả hòa nếu chọn cả ba đống đầu tiên; nếu không, cô ấy sẽ thua.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stoneValue.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= stoneValue[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người chơi lấy $1$– $3$ đống đá. Đệ quy đơn giản trên $n\le 5\times 10^4$ sẽ tính lại cùng một hậu tố nhiều lần.
>
> Người chơi hiện tại tối đa hóa “số đá lấy trong lượt này trừ đi chênh lệch tốt nhất của đối thủ ở phần còn lại”. Gọi $dfs(i)$ là giá trị đó tại chỉ số $i$, bằng cách thử ba prefix.
>
> Memoize $dfs(i)$ giúp đánh giá mỗi vị trí bắt đầu đúng một lần. Dấu của $dfs(0)$ quyết định Alice, Bob hay hòa.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu diễn chênh lệch điểm lớn nhất mà người chơi hiện tại có thể đạt được khi chơi trong phạm vi $[i, n)$. Nếu $dfs(0) > 0$, người chơi đầu tiên Alice có thể thắng; nếu $dfs(0) < 0$, người chơi thứ hai Bob có thể thắng; ngược lại, hai người hòa.

Logic thực thi của hàm $dfs(i)$ như sau:

- Nếu $i \geq n$, nghĩa là hiện không còn viên đá nào để lấy, nên ta trả về trực tiếp $0$;
- Nếu không, ta duyệt chỉ số $j$ của đống đá cuối cùng mà người chơi hiện tại lấy, với $i \le j < \min(i + 3, n)$, tức là người chơi hiện tại lấy tất cả các đống trong đoạn chỉ số $[i, j]$ và nhận được số điểm $\sum_{k=i}^{j} \textit{stoneValue}[k]$. Chênh lệch điểm mà người chơi kia có thể đạt được ở lượt tiếp theo là $dfs(j + 1)$, nên chênh lệch điểm mà người chơi hiện tại đạt được là $\sum_{k=i}^{j} \textit{stoneValue}[k] - dfs(j + 1)$. Ta muốn tối đa hóa chênh lệch điểm của người chơi hiện tại, nên có thể dùng hàm $\max$ để lấy chênh lệch lớn nhất, cụ thể là:

$$
dfs(i) = \max_{i \le j < \min(i+3, n)} \left\{\sum_{k=i}^{j} \textit{stoneValue}[k] - dfs(j + 1)\right\}
$$

Để tránh tính toán lặp lại, ta có thể dùng tìm kiếm có memoization.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số đống đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameIII(self, stoneValue: List[int]) -> str:
        @cache
        def dfs(i: int) -> int:
            if i >= len(stoneValue):
                return 0
            ans = -inf
            s = 0
            for j in range(i, i + 3):
                if j >= len(stoneValue):
                    break
                s += stoneValue[j]
                ans = max(ans, s - dfs(j + 1))
            return ans

        res = dfs(0)
        if res == 0:
            return 'Tie'
        return 'Alice' if res > 0 else 'Bob'
```

#### Java

```java
class Solution {
    private int[] stoneValue;
    private Integer[] f;
    private int n;

    public String stoneGameIII(int[] stoneValue) {
        this.stoneValue = stoneValue;
        this.n = stoneValue.length;
        this.f = new Integer[n];

        int res = dfs(0);

        if (res == 0) {
            return "Tie";
        }
        return res > 0 ? "Alice" : "Bob";
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }

        if (f[i] != null) {
            return f[i];
        }

        int ans = Integer.MIN_VALUE;
        int s = 0;

        for (int j = i; j < i + 3 && j < n; j++) {
            s += stoneValue[j];
            ans = Math.max(ans, s - dfs(j + 1));
        }

        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string stoneGameIII(vector<int>& stoneValue) {
        int n = stoneValue.size();
        vector<int> f(n, INT_MIN);

        auto dfs = [&](auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }

            if (f[i] != INT_MIN) {
                return f[i];
            }

            int ans = INT_MIN;
            int s = 0;

            for (int j = i; j < i + 3 && j < n; j++) {
                s += stoneValue[j];
                ans = max(ans, s - dfs(dfs, j + 1));
            }

            return f[i] = ans;
        };

        int res = dfs(dfs, 0);

        if (res == 0) {
            return "Tie";
        }
        return res > 0 ? "Alice" : "Bob";
    }
};
```

#### Go

```go
func stoneGameIII(stoneValue []int) string {
	n := len(stoneValue)
	f := make([]int, n)

	for i := range f {
		f[i] = -1 << 30
	}

	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}

		if f[i] != -1<<30 {
			return f[i]
		}

		ans := -1 << 30
		s := 0

		for j := i; j < i+3 && j < n; j++ {
			s += stoneValue[j]
			ans = max(ans, s-dfs(j+1))
		}

		f[i] = ans
		return ans
	}

	res := dfs(0)

	if res == 0 {
		return "Tie"
	}
	if res > 0 {
		return "Alice"
	}
	return "Bob"
}
```

#### TypeScript

```ts
function stoneGameIII(stoneValue: number[]): string {
    const n = stoneValue.length;
    const f = new Array<number>(n).fill(Number.MIN_SAFE_INTEGER);

    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }

        if (f[i] !== Number.MIN_SAFE_INTEGER) {
            return f[i];
        }

        let ans = Number.MIN_SAFE_INTEGER;
        let s = 0;

        for (let j = i; j < i + 3 && j < n; j++) {
            s += stoneValue[j];
            ans = Math.max(ans, s - dfs(j + 1));
        }

        f[i] = ans;
        return ans;
    };

    const res = dfs(0);

    if (res === 0) {
        return 'Tie';
    }
    return res > 0 ? 'Alice' : 'Bob';
}
```

#### Rust

```rust
impl Solution {
    pub fn stone_game_iii(stone_value: Vec<i32>) -> String {
        let n = stone_value.len();
        let mut f = vec![None; n];

        fn dfs(i: usize, stone_value: &Vec<i32>, f: &mut Vec<Option<i32>>) -> i32 {
            if i >= stone_value.len() {
                return 0;
            }

            if let Some(v) = f[i] {
                return v;
            }

            let mut ans = i32::MIN;
            let mut s = 0;

            for j in i..(i + 3).min(stone_value.len()) {
                s += stone_value[j];
                ans = ans.max(s - dfs(j + 1, stone_value, f));
            }

            f[i] = Some(ans);
            ans
        }

        let res = dfs(0, &stone_value, &mut f);

        if res == 0 {
            "Tie".to_string()
        } else if res > 0 {
            "Alice".to_string()
        } else {
            "Bob".to_string()
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
