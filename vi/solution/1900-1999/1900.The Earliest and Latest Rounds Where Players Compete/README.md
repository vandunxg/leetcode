---
comments: true
difficulty: Hard
rating: 2454
source: Weekly Contest 245 Q4
tags:
    - Memoization
    - Dynamic Programming
---

<!-- problem:start -->

# [1900. The Earliest and Latest Rounds Where Players Compete](https://leetcode.com/problems/the-earliest-and-latest-rounds-where-players-compete)

[中文文档](/solution/1900-1999/1900.The%20Earliest%20and%20Latest%20Rounds%20Where%20Players%20Compete/README.md)

## Mô tả

<!-- description:start -->

<p>Có một giải đấu với <code>n</code> người chơi tham gia. Những người chơi đứng thành một hàng và được đánh số từ <code>1</code> đến <code>n</code> theo vị trí đứng <strong>ban đầu</strong> (người chơi <code>1</code> đứng đầu hàng, người chơi <code>2</code> đứng thứ hai, v.v.).</p>

<p>Giải đấu gồm nhiều vòng (bắt đầu từ vòng số <code>1</code>). Trong mỗi vòng, người chơi thứ <code>i<sup>th</sup></code> tính từ đầu hàng sẽ thi đấu với người chơi thứ <code>i<sup>th</sup></code> tính từ cuối hàng, và người thắng sẽ tiến vào vòng tiếp theo. Nếu số người chơi trong vòng hiện tại là số lẻ, người chơi ở giữa sẽ tự động tiến vào vòng tiếp theo.</p>

<ul>
	<li>Ví dụ, nếu hàng gồm những người chơi <code>1, 2, 4, 6, 7</code>

    <ul>
    <li>Người chơi <code>1</code> thi đấu với người chơi <code>7</code>.</li>
    <li>Người chơi <code>2</code> thi đấu với người chơi <code>6</code>.</li>
    <li>Người chơi <code>4</code> tự động tiến vào vòng tiếp theo.</li>
    </ul>
    </li>

</ul>

<p>Sau khi mỗi vòng kết thúc, những người thắng được xếp lại thành hàng theo <strong>thứ tự ban đầu</strong> của họ (theo thứ tự tăng dần).</p>

<p>Hai người chơi được đánh số <code>firstPlayer</code> và <code>secondPlayer</code> là những người giỏi nhất giải. Họ có thể thắng bất kỳ người chơi nào khác trước khi thi đấu với nhau. Nếu hai người chơi khác thi đấu với nhau, một trong hai đều có thể thắng, vì vậy bạn có thể <strong>chọn</strong> kết quả của vòng đấu đó.</p>

<p>Với các số nguyên <code>n</code>, <code>firstPlayer</code> và <code>secondPlayer</code>, hãy trả về <em>một mảng số nguyên gồm hai giá trị lần lượt là số vòng <strong>sớm nhất</strong> và số vòng <strong>muộn nhất</strong> có thể xảy ra khi hai người chơi này thi đấu với nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 11, firstPlayer = 2, secondPlayer = 4
<strong>Đầu ra:</strong> [3,4]
<strong>Giải thích:</strong>
Một kịch bản có thể dẫn đến số vòng sớm nhất:
Vòng đầu tiên: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Vòng thứ hai: 2, 3, 4, 5, 6, 11
Vòng thứ ba: 2, 3, 4
Một kịch bản có thể dẫn đến số vòng muộn nhất:
Vòng đầu tiên: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11
Vòng thứ hai: 1, 2, 3, 4, 5, 6
Vòng thứ ba: 1, 2, 4
Vòng thứ tư: 2, 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, firstPlayer = 1, secondPlayer = 5
<strong>Đầu ra:</strong> [1,1]
<strong>Giải thích:</strong> Người chơi số 1 và 5 thi đấu với nhau ở vòng đầu tiên.
Không có cách nào để họ thi đấu ở vòng khác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 28</code></li>
	<li><code>1 &lt;= firstPlayer &lt; secondPlayer &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization + Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Nếu mô phỏng tất cả các cặp đấu còn lại, mỗi vòng sẽ rẽ nhánh theo $\lfloor n/2\rfloor$ trận. Với $n\le 28$, cây trạng thái ban đầu rất lớn và nhiều tiền tố lại dẫn đến cùng một thứ tự người chơi còn lại.
>
> Chỉ hai người chơi được chỉ định ảnh hưởng đến đáp án: kết quả của các trận khác chỉ thay đổi thứ hạng của họ ở vòng sau và số người còn lại. Vì vậy, trạng thái được rút gọn thành $(l,r,n)$.
>
> Ta liệt kê nhị phân người thắng ở nửa đầu, buộc $l$ và $r$ tiến vào vòng sau, ánh xạ lại những người sống sót theo thứ tự rồi đệ quy. Ghi nhớ bộ ba này cho phép lấy số vòng gặp nhau sớm nhất và muộn nhất bằng cách cộng thêm một vào giá trị nhỏ nhất và lớn nhất.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{dfs}(l, r, n)$, biểu diễn số vòng sớm nhất và muộn nhất mà người chơi mang số $l$ và $r$ thi đấu trong số $n$ người chơi ở vòng hiện tại.

Logic thực thi của hàm $\text{dfs}(l, r, n)$ như sau:

1. Nếu $l + r = n - 1$, nghĩa là hai người chơi thi đấu ở vòng hiện tại, trả về $[1, 1]$.
2. Nếu $f[l][r][n] \neq 0$, nghĩa là trạng thái này đã được tính trước đó, trả về trực tiếp kết quả.
3. Khởi tạo số vòng sớm nhất là dương vô cùng và số vòng muộn nhất là âm vô cùng.
4. Tính số người chơi trong nửa đầu của vòng hiện tại $m = n / 2$.
5. Liệt kê tất cả tổ hợp người thắng có thể có của nửa đầu (bằng cách liệt kê nhị phân), với mỗi tổ hợp:
    - Xác định người thắng dựa trên tổ hợp hiện tại.
    - Xác định vị trí của người chơi mang số $l$ và $r$ trong vòng hiện tại.
    - Đếm vị trí của người chơi mang số $l$ và $r$ trong số những người còn lại, lần lượt là $a$ và $b$, cùng tổng số người còn lại $c$.
    - Gọi đệ quy $\text{dfs}(a, b, c)$ để nhận số vòng sớm nhất và muộn nhất của trạng thái hiện tại.
    - Cập nhật số vòng sớm nhất và muộn nhất.
6. Lưu kết quả tính được vào $f[l][r][n]$ rồi trả về số vòng sớm nhất và muộn nhất.

Đáp án là $\text{dfs}(\text{firstPlayer} - 1, \text{secondPlayer} - 1, n)$.

<!-- tabs:start -->

#### Python3

```python
@cache
def dfs(l: int, r: int, n: int):
    if l + r == n - 1:
        return [1, 1]
    res = [inf, -inf]
    m = n >> 1
    for i in range(1 << m):
        win = [False] * n
        for j in range(m):
            if i >> j & 1:
                win[j] = True
            else:
                win[n - 1 - j] = True
        if n & 1:
            win[m] = True
        win[n - 1 - l] = win[n - 1 - r] = False
        win[l] = win[r] = True
        a = b = c = 0
        for j in range(n):
            if j == l:
                a = c
            if j == r:
                b = c
            if win[j]:
                c += 1
        x, y = dfs(a, b, c)
        res[0] = min(res[0], x + 1)
        res[1] = max(res[1], y + 1)
    return res


class Solution:
    def earliestAndLatest(
        self, n: int, firstPlayer: int, secondPlayer: int
    ) -> List[int]:
        return dfs(firstPlayer - 1, secondPlayer - 1, n)
```

#### Java

```java
class Solution {
    static int[][][] f = new int[30][30][31];

    public int[] earliestAndLatest(int n, int firstPlayer, int secondPlayer) {
        return dfs(firstPlayer - 1, secondPlayer - 1, n);
    }

    private int[] dfs(int l, int r, int n) {
        if (f[l][r][n] != 0) {
            return decode(f[l][r][n]);
        }
        if (l + r == n - 1) {
            f[l][r][n] = encode(1, 1);
            return new int[] {1, 1};
        }
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        int m = n >> 1;
        for (int i = 0; i < (1 << m); i++) {
            boolean[] win = new boolean[n];
            for (int j = 0; j < m; j++) {
                if (((i >> j) & 1) == 1) {
                    win[j] = true;
                } else {
                    win[n - 1 - j] = true;
                }
            }
            if ((n & 1) == 1) {
                win[m] = true;
            }
            win[n - 1 - l] = win[n - 1 - r] = false;
            win[l] = win[r] = true;
            int a = 0, b = 0, c = 0;
            for (int j = 0; j < n; j++) {
                if (j == l) {
                    a = c;
                }
                if (j == r) {
                    b = c;
                }
                if (win[j]) {
                    c++;
                }
            }
            int[] t = dfs(a, b, c);
            min = Math.min(min, t[0] + 1);
            max = Math.max(max, t[1] + 1);
        }
        f[l][r][n] = encode(min, max);
        return new int[] {min, max};
    }

    private int encode(int x, int y) {
        return (x << 8) | y;
    }

    private int[] decode(int val) {
        return new int[] {val >> 8, val & 255};
    }
}
```

#### C++

```cpp
int f[30][30][31];
class Solution {
public:
    vector<int> earliestAndLatest(int n, int firstPlayer, int secondPlayer) {
        return dfs(firstPlayer - 1, secondPlayer - 1, n);
    }

private:
    vector<int> dfs(int l, int r, int n) {
        if (f[l][r][n] != 0) {
            return decode(f[l][r][n]);
        }
        if (l + r == n - 1) {
            f[l][r][n] = encode(1, 1);
            return {1, 1};
        }

        int min = INT_MAX, max = INT_MIN;
        int m = n >> 1;

        for (int i = 0; i < (1 << m); i++) {
            vector<bool> win(n, false);
            for (int j = 0; j < m; j++) {
                if ((i >> j) & 1) {
                    win[j] = true;
                } else {
                    win[n - 1 - j] = true;
                }
            }
            if (n & 1) {
                win[m] = true;
            }

            win[n - 1 - l] = false;
            win[n - 1 - r] = false;
            win[l] = true;
            win[r] = true;

            int a = 0, b = 0, c = 0;
            for (int j = 0; j < n; j++) {
                if (j == l) a = c;
                if (j == r) b = c;
                if (win[j]) c++;
            }

            vector<int> t = dfs(a, b, c);
            min = std::min(min, t[0] + 1);
            max = std::max(max, t[1] + 1);
        }

        f[l][r][n] = encode(min, max);
        return {min, max};
    }

    int encode(int x, int y) {
        return (x << 8) | y;
    }

    vector<int> decode(int val) {
        return {val >> 8, val & 255};
    }
};
```

#### Go

```go
var f [30][30][31]int

func earliestAndLatest(n int, firstPlayer int, secondPlayer int) []int {
	return dfs(firstPlayer-1, secondPlayer-1, n)
}

func dfs(l, r, n int) []int {
	if f[l][r][n] != 0 {
		return decode(f[l][r][n])
	}
	if l+r == n-1 {
		f[l][r][n] = encode(1, 1)
		return []int{1, 1}
	}

	min, max := 1<<30, -1<<31
	m := n >> 1

	for i := 0; i < (1 << m); i++ {
		win := make([]bool, n)
		for j := 0; j < m; j++ {
			if (i>>j)&1 == 1 {
				win[j] = true
			} else {
				win[n-1-j] = true
			}
		}
		if n&1 == 1 {
			win[m] = true
		}
		win[n-1-l] = false
		win[n-1-r] = false
		win[l] = true
		win[r] = true

		a, b, c := 0, 0, 0
		for j := 0; j < n; j++ {
			if j == l {
				a = c
			}
			if j == r {
				b = c
			}
			if win[j] {
				c++
			}
		}

		t := dfs(a, b, c)
		if t[0]+1 < min {
			min = t[0] + 1
		}
		if t[1]+1 > max {
			max = t[1] + 1
		}
	}

	f[l][r][n] = encode(min, max)
	return []int{min, max}
}

func encode(x, y int) int {
	return (x << 8) | y
}

func decode(val int) []int {
	return []int{val >> 8, val & 255}
}
```

#### TypeScript

```ts
function earliestAndLatest(n: number, firstPlayer: number, secondPlayer: number): number[] {
    return dfs(firstPlayer - 1, secondPlayer - 1, n);
}

const f: number[][][] = Array.from({ length: 30 }, () =>
    Array.from({ length: 30 }, () => Array(31).fill(0)),
);

function dfs(l: number, r: number, n: number): number[] {
    if (f[l][r][n] !== 0) {
        return decode(f[l][r][n]);
    }
    if (l + r === n - 1) {
        f[l][r][n] = encode(1, 1);
        return [1, 1];
    }

    let min = Number.MAX_SAFE_INTEGER;
    let max = Number.MIN_SAFE_INTEGER;
    const m = n >> 1;

    for (let i = 0; i < 1 << m; i++) {
        const win: boolean[] = Array(n).fill(false);
        for (let j = 0; j < m; j++) {
            if ((i >> j) & 1) {
                win[j] = true;
            } else {
                win[n - 1 - j] = true;
            }
        }

        if (n & 1) {
            win[m] = true;
        }

        win[n - 1 - l] = false;
        win[n - 1 - r] = false;
        win[l] = true;
        win[r] = true;

        let a = 0,
            b = 0,
            c = 0;
        for (let j = 0; j < n; j++) {
            if (j === l) a = c;
            if (j === r) b = c;
            if (win[j]) c++;
        }

        const t = dfs(a, b, c);
        min = Math.min(min, t[0] + 1);
        max = Math.max(max, t[1] + 1);
    }

    f[l][r][n] = encode(min, max);
    return [min, max];
}

function encode(x: number, y: number): number {
    return (x << 8) | y;
}

function decode(val: number): number[] {
    return [val >> 8, val & 255];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
