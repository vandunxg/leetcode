---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Memoization
    - Math
    - Dynamic Programming
    - Bitmask
    - Game Theory
---

<!-- problem:start -->

# [464. Can I Win](https://leetcode.com/problems/can-i-win)

[中文文档](/solution/0400-0499/0464.Can%20I%20Win/README.md)

## Mô tả

<!-- description:start -->

<p>Trong trò chơi &quot;100 game&quot;, hai người chơi lần lượt cộng một số nguyên bất kỳ từ <code>1</code> đến <code>10</code> vào tổng tích lũy. Người đầu tiên khiến tổng <strong>đạt hoặc vượt</strong> 100 sẽ thắng.</p>

<p>Nếu thay đổi luật để người chơi <strong>không thể</strong> chọn lại số đã được chọn thì sao?</p>

<p>Ví dụ, hai người chơi lần lượt rút số từ một tập chung gồm các số từ 1 đến 15, không hoàn lại, cho đến khi tổng đạt &gt;= 100.</p>

<p>Cho hai số nguyên <code>maxChoosableInteger</code> và <code>desiredTotal</code>, trả về <code>true</code> nếu người chơi đi trước có thể đảm bảo chiến thắng; ngược lại, trả về <code>false</code>. Giả sử cả hai người chơi đều chơi <strong>tối ưu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxChoosableInteger = 10, desiredTotal = 11
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Dù người chơi thứ nhất chọn số nào, họ cũng sẽ thua.
Người chơi thứ nhất có thể chọn một số nguyên từ 1 đến 10.
Nếu người chơi thứ nhất chọn 1, người chơi thứ hai chỉ có thể chọn số nguyên từ 2 đến 10.
Người chơi thứ hai sẽ thắng nếu chọn 10, khi đó tổng bằng 11, tức là &gt;= desiredTotal.
Tương tự với mọi số khác mà người chơi thứ nhất chọn, người chơi thứ hai luôn thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxChoosableInteger = 10, desiredTotal = 0
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> maxChoosableInteger = 10, desiredTotal = 1
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= maxChoosableInteger &lt;= 20</code></li>
	<li><code>0 &lt;= desiredTotal &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lần lượt chọn các số từ $1..n$ mà không hoàn lại; ai đạt tổng mục tiêu trước sẽ thắng. Vì $n\le 20$, trạng thái có thể biểu diễn bằng tập các số đã dùng; duyệt cây trò chơi trực tiếp sẽ lặp lại nhiều trạng thái.
>
> Nếu tổng tất cả các số quá nhỏ thì không ai có thể thắng. Nếu không, $dfs(\textit{mask},s)$ thử từng $i$ chưa được chọn: người chơi hiện tại thắng nếu $s+i$ đã đạt mục tiêu hoặc đối thủ sẽ thua sau lượt chọn đó. Lưu kết quả theo $\textit{mask}$.
>
> Bit mask mã hóa gọn tập số đã chọn; có tối đa $2^{n}$ bài toán con. Kiểm tra tổng trước giúp bỏ qua lượt tìm kiếm chắc chắn thất bại.

<!-- thinking:end -->

Trước tiên, ta kiểm tra tổng của tất cả số có thể chọn có nhỏ hơn mục tiêu hay không. Nếu có, không thể thắng dù chọn thế nào, nên trả về `false` ngay.

Tiếp theo, ta xây dựng hàm `dfs(mask, s)`, trong đó `mask` biểu diễn trạng thái các số đã chọn, còn `s` là tổng tích lũy hiện tại. Hàm trả về việc người chơi hiện tại có thể thắng hay không.

Hàm `dfs(mask, s)` hoạt động như sau:

Ta duyệt từng số nguyên `i` từ `1` đến `maxChoosableInteger`. Nếu `i` chưa được chọn thì ta có thể chọn `i`. Nếu tổng tích lũy `s + i` sau khi chọn `i` lớn hơn hoặc bằng mục tiêu `desiredTotal`, hoặc sau khi người chơi chọn `i`, đối thủ sẽ rơi vào thế thua, thì người chơi hiện tại thắng và ta trả về `true`.

Nếu không có lựa chọn nào giúp người chơi hiện tại thắng, người đó thua và ta trả về `false`.

Để tránh tính toán lặp lại, ta dùng hash table `f` lưu các trạng thái đã tính, trong đó key là `mask` và value cho biết người chơi hiện tại có thể thắng hay không.

Độ phức tạp thời gian là $O(2^n)$ và độ phức tạp không gian là $O(2^n)$, trong đó $n$ là `maxChoosableInteger`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canIWin(self, maxChoosableInteger: int, desiredTotal: int) -> bool:
        @cache
        def dfs(mask: int, s: int) -> bool:
            for i in range(1, maxChoosableInteger + 1):
                if mask >> i & 1 ^ 1:
                    if s + i >= desiredTotal or not dfs(mask | 1 << i, s + i):
                        return True
            return False

        if (1 + maxChoosableInteger) * maxChoosableInteger // 2 < desiredTotal:
            return False
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private Map<Integer, Boolean> f = new HashMap<>();
    private int maxChoosableInteger;
    private int desiredTotal;

    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) {
            return false;
        }
        this.maxChoosableInteger = maxChoosableInteger;
        this.desiredTotal = desiredTotal;
        return dfs(0, 0);
    }

    private boolean dfs(int mask, int s) {
        if (f.containsKey(mask)) {
            return f.get(mask);
        }
        for (int i = 0; i < maxChoosableInteger; ++i) {
            if ((mask >> i & 1) == 0) {
                if (s + i + 1 >= desiredTotal || !dfs(mask | 1 << i, s + i + 1)) {
                    f.put(mask, true);
                    return true;
                }
            }
        }
        f.put(mask, false);
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) {
            return false;
        }
        unordered_map<int, int> f;
        function<bool(int, int)> dfs = [&](int mask, int s) {
            if (f.contains(mask)) {
                return f[mask];
            }
            for (int i = 0; i < maxChoosableInteger; ++i) {
                if (mask >> i & 1 ^ 1) {
                    if (s + i + 1 >= desiredTotal || !dfs(mask | 1 << i, s + i + 1)) {
                        return f[mask] = true;
                    }
                }
            }
            return f[mask] = false;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func canIWin(maxChoosableInteger int, desiredTotal int) bool {
	if (1+maxChoosableInteger)*maxChoosableInteger/2 < desiredTotal {
		return false
	}
	f := map[int]bool{}
	var dfs func(int, int) bool
	dfs = func(mask, s int) bool {
		if v, ok := f[mask]; ok {
			return v
		}
		for i := 1; i <= maxChoosableInteger; i++ {
			if mask>>i&1 == 0 {
				if s+i >= desiredTotal || !dfs(mask|1<<i, s+i) {
					f[mask] = true
					return true
				}
			}
		}
		f[mask] = false
		return false
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function canIWin(maxChoosableInteger: number, desiredTotal: number): boolean {
    if (((1 + maxChoosableInteger) * maxChoosableInteger) / 2 < desiredTotal) {
        return false;
    }
    const f: Record<string, boolean> = {};
    const dfs = (mask: number, s: number): boolean => {
        if (f.hasOwnProperty(mask)) {
            return f[mask];
        }
        for (let i = 1; i <= maxChoosableInteger; ++i) {
            if (((mask >> i) & 1) ^ 1) {
                if (s + i >= desiredTotal || !dfs(mask ^ (1 << i), s + i)) {
                    return (f[mask] = true);
                }
            }
        }
        return (f[mask] = false);
    };
    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
