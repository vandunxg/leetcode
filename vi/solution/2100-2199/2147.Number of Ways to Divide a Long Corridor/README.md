---
comments: true
difficulty: Hard
rating: 1914
source: Biweekly Contest 70 Q4
tags:
    - Math
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2147. Number of Ways to Divide a Long Corridor](https://leetcode.com/problems/number-of-ways-to-divide-a-long-corridor)

[中文文档](/solution/2100-2199/2147.Number%20of%20Ways%20to%20Divide%20a%20Long%20Corridor/README.md)

## Mô tả

<!-- description:start -->

<p>Dọc theo một hành lang dài của thư viện có một dãy ghế và các chậu cây trang trí. Cho chuỗi <code>corridor</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, chỉ gồm các ký tự <code>&#39;S&#39;</code> và <code>&#39;P&#39;</code>, trong đó mỗi <code>&#39;S&#39;</code> biểu thị một chiếc ghế và mỗi <code>&#39;P&#39;</code> biểu thị một chậu cây.</p>

<p>Một vách ngăn phòng <strong>đã</strong> được lắp đặt ở bên trái chỉ số <code>0</code>, và <strong>một vách ngăn khác</strong> ở bên phải chỉ số <code>n - 1</code>. Ta có thể lắp thêm các vách ngăn. Với mỗi vị trí nằm giữa các chỉ số <code>i - 1</code> và <code>i</code> (<code>1 &lt;= i &lt;= n - 1</code>), có thể lắp nhiều nhất một vách ngăn.</p>

<p>Chia hành lang thành các đoạn không giao nhau, sao cho mỗi đoạn có <strong>chính xác hai chiếc ghế</strong> và có thể có tùy ý số lượng chậu cây. Có thể có nhiều cách chia hành lang. Hai cách được xem là <strong>khác nhau</strong> nếu tồn tại một vị trí có vách ngăn trong cách thứ nhất nhưng không có trong cách thứ hai.</p>

<p>Trả về <em>số cách chia hành lang</em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>. Nếu không có cách chia nào, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2147.Number%20of%20Ways%20to%20Divide%20a%20Long%20Corridor/images/1.png" style="width: 410px; height: 199px;" />
<pre>
<strong>Đầu vào:</strong> corridor = &quot;SSPPSPS&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cách chia hành lang khác nhau.
Các thanh màu đen trong hình trên biểu thị hai vách ngăn phòng đã được lắp đặt.
Lưu ý rằng trong mỗi cách chia, <strong>mỗi</strong> đoạn đều có <strong>chính xác hai</strong> chiếc ghế.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2147.Number%20of%20Ways%20to%20Divide%20a%20Long%20Corridor/images/2.png" style="width: 357px; height: 68px;" />
<pre>
<strong>Đầu vào:</strong> corridor = &quot;PPSPSP&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có 1 cách chia hành lang, đó là không lắp thêm vách ngăn nào.
Nếu lắp thêm bất kỳ vách ngăn nào, sẽ tạo ra một đoạn không có chính xác hai chiếc ghế.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2147.Number%20of%20Ways%20to%20Divide%20a%20Long%20Corridor/images/3.png" style="width: 115px; height: 68px;" />
<pre>
<strong>Đầu vào:</strong> corridor = &quot;S&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cách nào chia hành lang vì luôn có một đoạn không có chính xác hai chiếc ghế.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == corridor.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>corridor[i]</code> là <code>&#39;S&#39;</code> hoặc <code>&#39;P&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đoạn phải có chính xác hai chiếc ghế, và vách ngăn có thể được đặt trên các chậu cây nằm giữa các đoạn. Hành lang rất dài; việc chọn có đóng một đoạn sau hai chiếc ghế hay không tạo ra các bài toán con chồng lấn.
>
> Trạng thái $(i,k)$ là số cách tại vị trí $i$ khi đoạn đang mở có $k$ chiếc ghế. Một chiếc ghế làm $k$ tăng lên; $k>2$ là không hợp lệ; khi $k=2$, ta có thể cắt đoạn (đặt lại $k$) hoặc tiếp tục. Memoization giúp số trạng thái chỉ còn $O(n)$.
>
> Trả về $\textit{dfs}(0,0)$.

<!-- thinking:end -->

Ta thiết kế hàm $\textit{dfs}(i, k)$, biểu thị số cách chia hành lang tại vị trí thứ $i$, khi đã đặt $k$ vách ngăn. Khi đó, đáp án là $\textit{dfs}(0, 0)$.

Quá trình tính toán của hàm $\textit{dfs}(i, k)$ như sau:

Nếu $i \geq \textit{len}(\textit{corridor})$, nghĩa là ta đã duyệt hết hành lang. Khi đó, nếu $k = 2$, điều này cho biết ta đã tìm được một cách chia hợp lệ, nên trả về $1$. Ngược lại, trả về $0$.

Nếu không, ta cần xét tình huống tại vị trí hiện tại $i$:

- Nếu $\textit{corridor}[i] = \text{'S'}$, vị trí hiện tại là một chiếc ghế, nên ta tăng $k$ lên $1$.
- Nếu $k > 2$, nghĩa là số ghế trong đoạn hiện tại vượt quá $2$, nên trả về $0$.
- Nếu không, ta có thể chọn không đặt vách ngăn, tức là $\textit{dfs}(i + 1, k)$. Nếu $k = 2$, ta cũng có thể chọn đặt một vách ngăn, tức là $\textit{dfs}(i + 1, 0)$. Cộng kết quả của hai trường hợp này và lấy modulo $10^9 + 7$, tức là $\textit{ans} = (\textit{ans} + \textit{dfs}(i + 1, k)) \bmod \text{mod}$.

Cuối cùng, trả về $\textit{dfs}(0, 0)$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của hành lang.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, corridor: str) -> int:
        @cache
        def dfs(i: int, k: int) -> int:
            if i >= len(corridor):
                return int(k == 2)
            k += int(corridor[i] == "S")
            if k > 2:
                return 0
            ans = dfs(i + 1, k)
            if k == 2:
                ans = (ans + dfs(i + 1, 0)) % mod
            return ans

        mod = 10**9 + 7
        ans = dfs(0, 0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;
    private Integer[][] f;
    private final int mod = (int) 1e9 + 7;

    public int numberOfWays(String corridor) {
        s = corridor.toCharArray();
        n = s.length;
        f = new Integer[n][3];
        return dfs(0, 0);
    }

    private int dfs(int i, int k) {
        if (i >= n) {
            return k == 2 ? 1 : 0;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        k += s[i] == 'S' ? 1 : 0;
        if (k > 2) {
            return 0;
        }
        int ans = dfs(i + 1, k);
        if (k == 2) {
            ans = (ans + dfs(i + 1, 0)) % mod;
        }
        return f[i][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(string corridor) {
        int n = corridor.size();
        int f[n][3];
        memset(f, -1, sizeof(f));
        const int mod = 1e9 + 7;
        auto dfs = [&](this auto&& dfs, int i, int k) -> int {
            if (i >= n) {
                return k == 2;
            }
            if (f[i][k] != -1) {
                return f[i][k];
            }
            k += corridor[i] == 'S';
            if (k > 2) {
                return 0;
            }
            f[i][k] = dfs(i + 1, k);
            if (k == 2) {
                f[i][k] = (f[i][k] + dfs(i + 1, 0)) % mod;
            }
            return f[i][k];
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func numberOfWays(corridor string) int {
	n := len(corridor)
	f := make([][3]int, n)
	for i := range f {
		f[i] = [3]int{-1, -1, -1}
	}
	const mod = 1e9 + 7
	var dfs func(int, int) int
	dfs = func(i, k int) int {
		if i >= n {
			if k == 2 {
				return 1
			}
			return 0
		}
		if f[i][k] != -1 {
			return f[i][k]
		}
		if corridor[i] == 'S' {
			k++
		}
		if k > 2 {
			return 0
		}
		f[i][k] = dfs(i+1, k)
		if k == 2 {
			f[i][k] = (f[i][k] + dfs(i+1, 0)) % mod
		}
		return f[i][k]
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function numberOfWays(corridor: string): number {
    const n = corridor.length;
    const mod = 10 ** 9 + 7;
    const f: number[][] = Array.from({ length: n }, () => Array(3).fill(-1));
    const dfs = (i: number, k: number): number => {
        if (i >= n) {
            return k === 2 ? 1 : 0;
        }
        if (f[i][k] !== -1) {
            return f[i][k];
        }
        if (corridor[i] === 'S') {
            ++k;
        }
        if (k > 2) {
            return (f[i][k] = 0);
        }
        f[i][k] = dfs(i + 1, k);
        if (k === 2) {
            f[i][k] = (f[i][k] + dfs(i + 1, 0)) % mod;
        }
        return f[i][k];
    };
    return dfs(0, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_ways(corridor: String) -> i32 {
        let n: usize = corridor.len();
        let bytes = corridor.as_bytes();
        let modv: i32 = 1_000_000_007;

        let mut f = vec![vec![-1; 3]; n];

        fn dfs(
            i: usize,
            k: usize,
            n: usize,
            bytes: &[u8],
            f: &mut Vec<Vec<i32>>,
            modv: i32,
        ) -> i32 {
            if i >= n {
                return if k == 2 { 1 } else { 0 };
            }
            if f[i][k] != -1 {
                return f[i][k];
            }

            let mut nk = k;
            if bytes[i] == b'S' {
                nk += 1;
            }
            if nk > 2 {
                return 0;
            }

            let mut res = dfs(i + 1, nk, n, bytes, f, modv);
            if nk == 2 {
                res = (res + dfs(i + 1, 0, n, bytes, f, modv)) % modv;
            }

            f[i][k] = res;
            res
        }

        dfs(0, 0, n, bytes, &mut f, modv)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng bộ nhớ tuyến tính. Các ghế phải được ghép thành từng cặp, và số chậu cây giữa các cặp liên tiếp sẽ tạo ra các lựa chọn độc lập, rồi nhân với nhau.
>
> Duyệt một lần, ta theo dõi số ghế và chiếc ghế trước đó; mỗi khi bắt đầu một cặp mới, ta nhân đáp án với khoảng cách sau cặp trước. Nếu có 0 hoặc số ghế lẻ, đáp án là $0$.
>
> Bộ nhớ phụ chỉ ở mức hằng số.

<!-- thinking:end -->

Ta có thể chia cứ hai chiếc ghế thành một nhóm. Giữa hai nhóm ghế liền kề, nếu khoảng cách giữa chiếc ghế cuối của nhóm trước và chiếc ghế đầu của nhóm sau là $x$, thì có $x$ cách đặt vách ngăn.

Ta duyệt qua hành lang, sử dụng biến $\textit{cnt}$ để ghi lại số ghế hiện tại và biến $\textit{last}$ để ghi lại vị trí của chiếc ghế cuối cùng.

Khi gặp một chiếc ghế, ta tăng $\textit{cnt}$ lên $1$. Nếu $\textit{cnt}$ lớn hơn $2$ và $\textit{cnt}$ là số lẻ, ta cần đặt một vách ngăn giữa $\textit{last}$ và chiếc ghế hiện tại. Số cách đặt vách ngăn là $\textit{ans} \times (i - \textit{last})$, trong đó $\textit{ans}$ là số cách trước đó. Sau đó, ta cập nhật $\textit{last}$ thành vị trí $i$ của chiếc ghế hiện tại.

Cuối cùng, nếu $\textit{cnt}$ lớn hơn $0$ và $\textit{cnt}$ là số chẵn, trả về $\textit{ans}$; ngược lại, trả về $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của hành lang. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, corridor: str) -> int:
        mod = 10**9 + 7
        ans, cnt, last = 1, 0, 0
        for i, c in enumerate(corridor):
            if c == "S":
                cnt += 1
                if cnt > 2 and cnt % 2:
                    ans = ans * (i - last) % mod
                last = i
        return ans if cnt and cnt % 2 == 0 else 0
```

#### Java

```java
class Solution {
    public int numberOfWays(String corridor) {
        final int mod = (int) 1e9 + 7;
        long ans = 1, cnt = 0, last = 0;
        for (int i = 0; i < corridor.length(); ++i) {
            if (corridor.charAt(i) == 'S') {
                if (++cnt > 2 && cnt % 2 == 1) {
                    ans = ans * (i - last) % mod;
                }
                last = i;
            }
        }
        return cnt > 0 && cnt % 2 == 0 ? (int) ans : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWays(string corridor) {
        const int mod = 1e9 + 7;
        long long ans = 1;
        int cnt = 0, last = 0;
        for (int i = 0; i < corridor.length(); ++i) {
            if (corridor[i] == 'S') {
                if (++cnt > 2 && cnt % 2) {
                    ans = ans * (i - last) % mod;
                }
                last = i;
            }
        }
        return cnt > 0 && cnt % 2 == 0 ? ans : 0;
    }
};
```

#### Go

```go
func numberOfWays(corridor string) int {
	const mod int = 1e9 + 7
	ans, cnt, last := 1, 0, 0
	for i, c := range corridor {
		if c == 'S' {
			cnt++
			if cnt > 2 && cnt%2 == 1 {
				ans = ans * (i - last) % mod
			}
			last = i
		}
	}
	if cnt > 0 && cnt%2 == 0 {
		return ans
	}
	return 0
}
```

#### TypeScript

```ts
function numberOfWays(corridor: string): number {
    const mod = 10 ** 9 + 7;
    const n = corridor.length;
    let [ans, cnt, last] = [1, 0, 0];
    for (let i = 0; i < n; ++i) {
        if (corridor[i] === 'S') {
            if (++cnt > 2 && cnt % 2) {
                ans = (ans * (i - last)) % mod;
            }
            last = i;
        }
    }
    return cnt > 0 && cnt % 2 === 0 ? ans : 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_ways(corridor: String) -> i32 {
        let modv: i64 = 1_000_000_007;
        let mut ans: i64 = 1;
        let mut cnt: i64 = 0;
        let mut last: i64 = 0;

        for (i, ch) in corridor.chars().enumerate() {
            if ch == 'S' {
                cnt += 1;
                if cnt > 2 && cnt % 2 == 1 {
                    ans = ans * (i as i64 - last) % modv;
                }
                last = i as i64;
            }
        }

        if cnt > 0 && cnt % 2 == 0 {
            ans as i32
        } else {
            0
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
