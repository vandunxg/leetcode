---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [2403. Minimum Time to Kill All Monsters 🔒](https://leetcode.com/problems/minimum-time-to-kill-all-monsters)

[中文文档](/solution/2400-2499/2403.Minimum%20Time%20to%20Kill%20All%20Monsters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>power</code>, trong đó <code>power[i]</code> là sức mạnh của quái vật thứ <code>i<sup>th</sup></code>.</p>

<p>Ban đầu bạn có <code>0</code> điểm mana, và mỗi ngày số điểm mana tăng thêm <code>gain</code>, với <code>gain</code> ban đầu bằng <code>1</code>.</p>

<p>Mỗi ngày, sau khi nhận thêm <code>gain</code> mana, bạn có thể đánh bại một quái vật nếu số điểm mana của bạn lớn hơn hoặc bằng sức mạnh của quái vật đó. Khi đánh bại một quái vật:</p>

<ul>
	<li>số điểm mana của bạn được đặt lại thành <code>0</code>, và</li>
	<li>giá trị của <code>gain</code> tăng thêm <code>1</code>.</li>
</ul>

<p>Trả về <em><strong>số ngày nhỏ nhất</strong> cần để đánh bại tất cả quái vật.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> power = [3,1,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Cách tối ưu để đánh bại tất cả quái vật là:
- Ngày 1: Nhận 1 điểm mana, tổng cộng có 1 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 2<sup>nd</sup>.
- Ngày 2: Nhận 2 điểm mana, tổng cộng có 2 điểm mana.
- Ngày 3: Nhận 2 điểm mana, tổng cộng có 4 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 3<sup>rd</sup>.
- Ngày 4: Nhận 3 điểm mana, tổng cộng có 3 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 1<sup>st</sup>.
Có thể chứng minh rằng 4 là số ngày nhỏ nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> power = [1,1,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Cách tối ưu để đánh bại tất cả quái vật là:
- Ngày 1: Nhận 1 điểm mana, tổng cộng có 1 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 1<sup>st</sup>.
- Ngày 2: Nhận 2 điểm mana, tổng cộng có 2 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 2<sup>nd</sup>.
- Ngày 3: Nhận 3 điểm mana, tổng cộng có 3 điểm mana.
- Ngày 4: Nhận 3 điểm mana, tổng cộng có 6 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 3<sup>rd</sup>.
Có thể chứng minh rằng 4 là số ngày nhỏ nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> power = [1,2,4,9]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Cách tối ưu để đánh bại tất cả quái vật là:
- Ngày 1: Nhận 1 điểm mana, tổng cộng có 1 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 1.
- Ngày 2: Nhận 2 điểm mana, tổng cộng có 2 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 2.
- Ngày 3: Nhận 3 điểm mana, tổng cộng có 3 điểm mana.
- Ngày 4: Nhận 3 điểm mana, tổng cộng có 6 điểm mana.
- Ngày 5: Nhận 3 điểm mana, tổng cộng có 9 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 4.
- Ngày 6: Nhận 4 điểm mana, tổng cộng có 4 điểm mana. Dùng toàn bộ mana để đánh bại quái vật thứ 3.
Có thể chứng minh rằng 6 là số ngày nhỏ nhất cần thiết.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= power.length &lt;= 17</code></li>
	<li><code>1 &lt;= power[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + tìm kiếm ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Có $n!$ thứ tự đánh bại; $n\le 17$ nên không thể liệt kê tất cả. Lượng mana nhận được mỗi ngày bằng một cộng với số quái vật đã bị đánh bại, vì vậy đáp án tối ưu trên một tập quái vật còn sống chỉ phụ thuộc vào chính tập đó.
>
> Gọi $\textit{mask}$ là các quái vật còn sống và $dfs(\textit{mask})$ là số ngày nhỏ nhất để đánh bại chúng. Lượng mana nhận được được xác định bởi số quái vật đã chết. Thử từng bit đang bật làm quái vật tiếp theo bị đánh bại và ghi nhớ kết quả: có $2^n$ trạng thái, mỗi trạng thái có $O(n)$ chuyển trạng thái.

<!-- thinking:end -->

Ta nhận thấy số lượng quái vật không vượt quá $17$, nên có thể dùng một số nhị phân 17 bit để biểu diễn trạng thái của các quái vật. Bit thứ $i$ bằng $1$ cho biết quái vật thứ $i$ vẫn còn sống, còn bằng $0$ cho biết quái vật thứ $i$ đã bị đánh bại.

Ta xây dựng hàm $\textit{dfs}(\textit{mask})$ biểu diễn số ngày nhỏ nhất cần để đánh bại tất cả quái vật khi trạng thái hiện tại của các quái vật là $\textit{mask}$. Đáp án là $\textit{dfs}(2^n - 1)$, trong đó $n$ là số lượng quái vật.

Cách tính hàm $\textit{dfs}(\textit{mask})$ như sau:

- Nếu $\textit{mask} = 0$, nghĩa là tất cả quái vật đã bị đánh bại, trả về $0$;
- Nếu không, ta duyệt qua từng quái vật $i$. Nếu quái vật thứ $i$ vẫn còn sống, ta có thể chọn đánh bại quái vật thứ $i$, sau đó tính đệ quy $\textit{dfs}(\textit{mask} \oplus 2^i)$, rồi cập nhật đáp án thành $\textit{ans} = \min(\textit{ans}, \textit{dfs}(\textit{mask} \oplus 2^i) + \lceil \frac{x}{\textit{gain}} \rceil)$, trong đó $x$ là sức mạnh của quái vật thứ $i$, và $\textit{gain} = 1 + (n - \textit{mask}.\textit{bit\_count}())$ biểu diễn lượng mana nhận được mỗi ngày hiện tại.

Cuối cùng, ta trả về $\textit{dfs}(2^n - 1)$.

Độ phức tạp thời gian là $O(2^n \times n)$, và độ phức tạp không gian là $O(2^n)$. Ở đây, $n$ là số lượng quái vật.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, power: List[int]) -> int:
        @cache
        def dfs(mask: int) -> int:
            if mask == 0:
                return 0
            ans = inf
            gain = 1 + (n - mask.bit_count())
            for i, x in enumerate(power):
                if mask >> i & 1:
                    ans = min(ans, dfs(mask ^ (1 << i)) + (x + gain - 1) // gain)
            return ans

        n = len(power)
        return dfs((1 << n) - 1)
```

#### Java

```java
class Solution {
    private int n;
    private int[] power;
    private Long[] f;

    public long minimumTime(int[] power) {
        n = power.length;
        this.power = power;
        f = new Long[1 << n];
        return dfs((1 << n) - 1);
    }

    private long dfs(int mask) {
        if (mask == 0) {
            return 0;
        }
        if (f[mask] != null) {
            return f[mask];
        }
        f[mask] = Long.MAX_VALUE;
        int gain = 1 + (n - Integer.bitCount(mask));
        for (int i = 0; i < n; ++i) {
            if ((mask >> i & 1) == 1) {
                f[mask] = Math.min(f[mask], dfs(mask ^ 1 << i) + (power[i] + gain - 1) / gain);
            }
        }
        return f[mask];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumTime(vector<int>& power) {
        int n = power.size();
        long long f[1 << n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int mask) -> long long {
            if (mask == 0) {
                return 0;
            }
            if (f[mask] != -1) {
                return f[mask];
            }
            f[mask] = LLONG_MAX;
            int gain = 1 + (n - __builtin_popcount(mask));
            for (int i = 0; i < n; ++i) {
                if (mask >> i & 1) {
                    f[mask] = min(f[mask], dfs(mask ^ (1 << i)) + (power[i] + gain - 1) / gain);
                }
            }
            return f[mask];
        };
        return dfs((1 << n) - 1);
    }
};
```

#### Go

```go
func minimumTime(power []int) int64 {
	n := len(power)
	f := make([]int64, 1<<n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(mask int) int64
	dfs = func(mask int) int64 {
		if mask == 0 {
			return 0
		}
		if f[mask] != -1 {
			return f[mask]
		}
		f[mask] = 1e18
		gain := 1 + (n - bits.OnesCount(uint(mask)))
		for i, x := range power {
			if mask>>i&1 == 1 {
				f[mask] = min(f[mask], dfs(mask^(1<<i))+int64(x+gain-1)/int64(gain))
			}
		}
		return f[mask]
	}
	return dfs(1<<n - 1)
}
```

#### TypeScript

```ts
function minimumTime(power: number[]): number {
    const n = power.length;
    const f: number[] = Array(1 << n).fill(-1);
    const dfs = (mask: number): number => {
        if (mask === 0) {
            return 0;
        }
        if (f[mask] !== -1) {
            return f[mask];
        }
        f[mask] = Infinity;
        const gain = 1 + (n - bitCount(mask));
        for (let i = 0; i < n; ++i) {
            if ((mask >> i) & 1) {
                f[mask] = Math.min(f[mask], dfs(mask ^ (1 << i)) + Math.ceil(power[i] / gain));
            }
        }
        return f[mask];
    };
    return dfs((1 << n) - 1);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nén trạng thái + quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đã có độ phức tạp $O(2^n n)$ nhưng sử dụng đệ quy và cache. Ta có thể dùng các chuyển trạng thái tương tự để điền một bảng theo thứ tự tăng dần của $\textit{mask}$: $f[\textit{mask}]$ là số ngày nhỏ nhất sau khi đánh bại chính xác các bit đó, được chuyển từ trạng thái trước đó bằng cách bỏ đi một quái vật. Độ phức tạp tiệm cận không đổi; bit bằng $1$ lúc này biểu thị quái vật đã bị đánh bại, ngược với phương pháp 1.

<!-- thinking:end -->

Ta có thể chuyển phép tìm kiếm ghi nhớ trong Lời giải 1 thành quy hoạch động. Định nghĩa $f[\textit{mask}]$ là số ngày nhỏ nhất cần để đánh bại tất cả quái vật khi trạng thái hiện tại của các quái vật là $\textit{mask}$. Ở đây, $\textit{mask}$ là một số nhị phân $n$ bit, trong đó bit thứ $i$ bằng $1$ cho biết quái vật thứ $i$ đã bị đánh bại, còn bằng $0$ cho biết quái vật thứ $i$ vẫn còn sống. Ban đầu, $f[0] = 0$, còn các $f[\textit{mask}] = +\infty$. Đáp án là $f[2^n - 1]$.

Ta duyệt $\textit{mask}$ trong đoạn $[1, 2^n - 1]$. Với mỗi $\textit{mask}$, ta duyệt qua từng quái vật $i$. Nếu quái vật thứ $i$ đã bị đánh bại, trạng thái này có thể được chuyển từ trạng thái trước đó $\textit{mask} \oplus 2^i$, với chi phí chuyển là $(\textit{power}[i] + \textit{gain} - 1) / \textit{gain}$, trong đó $\textit{gain} = \textit{mask}.\textit{bitCount}()$.

Cuối cùng, trả về $f[2^n - 1]$.

Độ phức tạp thời gian là $O(2^n \times n)$, và độ phức tạp không gian là $O(2^n)$. Ở đây, $n$ là số lượng quái vật.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, power: List[int]) -> int:
        n = len(power)
        f = [inf] * (1 << n)
        f[0] = 0
        for mask in range(1, 1 << n):
            gain = mask.bit_count()
            for i, x in enumerate(power):
                if mask >> i & 1:
                    f[mask] = min(f[mask], f[mask ^ (1 << i)] + (x + gain - 1) // gain)
        return f[-1]
```

#### Java

```java
class Solution {
    public long minimumTime(int[] power) {
        int n = power.length;
        long[] f = new long[1 << n];
        Arrays.fill(f, Long.MAX_VALUE);
        f[0] = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            int gain = Integer.bitCount(mask);
            for (int i = 0; i < n; ++i) {
                if ((mask >> i & 1) == 1) {
                    f[mask] = Math.min(f[mask], f[mask ^ 1 << i] + (power[i] + gain - 1) / gain);
                }
            }
        }
        return f[(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumTime(vector<int>& power) {
        int n = power.size();
        long long f[1 << n];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            int gain = __builtin_popcount(mask);
            for (int i = 0; i < n; ++i) {
                if (mask >> i & 1) {
                    f[mask] = min(f[mask], f[mask ^ (1 << i)] + (power[i] + gain - 1) / gain);
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func minimumTime(power []int) int64 {
	n := len(power)
	f := make([]int64, 1<<n)
	for i := range f {
		f[i] = 1e18
	}
	f[0] = 0
	for mask := 1; mask < 1<<n; mask++ {
		gain := bits.OnesCount(uint(mask))
		for i, x := range power {
			if mask>>i&1 == 1 {
				f[mask] = min(f[mask], f[mask^(1<<i)]+int64(x+gain-1)/int64(gain))
			}
		}
	}
	return f[1<<n-1]
}
```

#### TypeScript

```ts
function minimumTime(power: number[]): number {
    const n = power.length;
    const f: number[] = Array(1 << n).fill(Infinity);
    f[0] = 0;
    for (let mask = 1; mask < 1 << n; ++mask) {
        const gain = bitCount(mask);
        for (let i = 0; i < n; ++i) {
            if ((mask >> i) & 1) {
                f[mask] = Math.min(f[mask], f[mask ^ (1 << i)] + Math.ceil(power[i] / gain));
            }
        }
    }
    return f.at(-1)!;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
