---
comments: true
difficulty: Medium
rating: 1739
source: Weekly Contest 480 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3776. Minimum Moves to Balance Circular Array](https://leetcode.com/problems/minimum-moves-to-balance-circular-array)

[中文文档](/solution/3700-3799/3776.Minimum%20Moves%20to%20Balance%20Circular%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <strong>vòng</strong> <code>balance</code> có độ dài <code>n</code>, trong đó <code>balance[i]</code> là số dư ròng của người thứ <code>i</code>.</p>

<p>Trong một lần di chuyển, một người có thể chuyển <strong>chính xác</strong> 1 đơn vị số dư cho người hàng xóm bên trái hoặc bên phải.</p>

<p>Trả về số lần di chuyển <strong>ít nhất</strong> cần thực hiện để mọi người đều có số dư <strong>không âm</strong>. Nếu không thể, trả về <code>-1</code>.</p>

<p><strong>Lưu ý</strong>: Đảm bảo rằng ban đầu có <strong>nhiều nhất</strong> 1 chỉ số có số dư <strong>âm</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [5,1,-4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi di chuyển tối ưu là:</p>

<ul>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> đến <code>i = 2</code>, khi đó <code>balance = [5, 0, -3]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> đến <code>i = 2</code>, khi đó <code>balance = [4, 0, -2]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> đến <code>i = 2</code>, khi đó <code>balance = [3, 0, -1]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> đến <code>i = 2</code>, khi đó <code>balance = [2, 0, 0]</code></li>
</ul>

<p>Vì vậy, số lần di chuyển ít nhất cần thực hiện là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [1,2,-5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi di chuyển tối ưu là:</p>

<ul>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> đến <code>i = 2</code>, khi đó <code>balance = [1, 1, -4, 2]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> đến <code>i = 2</code>, khi đó <code>balance = [1, 0, -3, 2]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 3</code> đến <code>i = 2</code>, khi đó <code>balance = [1, 0, -2, 1]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 3</code> đến <code>i = 2</code>, khi đó <code>balance = [1, 0, -1, 0]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 0</code> đến <code>i = 1</code>, khi đó <code>balance = [0, 1, -1, 0]</code></li>
	<li>Chuyển 1 đơn vị từ <code>i = 1</code> đến <code>i = 2</code>, khi đó <code>balance = [0, 0, 0, 0]</code></li>
</ul>

<p>Vì vậy, số lần di chuyển ít nhất cần thực hiện là 6.​​​</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">balance = [-3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong>Không thể làm cho mọi số dư đều không âm với <code>balance = [-3, 2]</code>, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == balance.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= balance[i] &lt;= 10<sup>9</sup></code></li>
	<li>Ban đầu có nhiều nhất một giá trị âm trong <code>balance</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất một số dư âm, và việc chuyển một đơn vị trên vòng tròn có chi phí bằng khoảng cách vòng tròn. Nếu tổng số dư âm thì không thể thực hiện; ngược lại, ta bù phần thiếu duy nhất từ các hàng xóm dương gần nhất ra ngoài, mỗi lần cộng thêm $\textit{amount}\times\textit{distance}$.

<!-- thinking:end -->

Trước hết, ta tính tổng của mảng $\textit{balance}$. Nếu tổng nhỏ hơn $0$, không thể làm cho mọi số dư đều không âm, nên ta trực tiếp trả về $-1$. Sau đó, ta tìm số dư nhỏ nhất trong mảng và chỉ số của nó. Nếu số dư nhỏ nhất lớn hơn hoặc bằng $0$, mọi số dư đã không âm, nên ta trực tiếp trả về $0$.

Tiếp theo, ta tính lượng số dư cần thiết $\textit{need}$, là đối của số dư nhỏ nhất. Sau đó, bắt đầu từ chỉ số có số dư nhỏ nhất, ta duyệt mảng sang trái và phải, lấy nhiều số dư nhất có thể từ mỗi vị trí để bù cho $\textit{need}$, đồng thời tính số lần di chuyển. Ta tiếp tục cho đến khi $\textit{need}$ trở thành $0$, rồi trả về tổng số lần di chuyển.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{balance}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, balance: List[int]) -> int:
        if sum(balance) < 0:
            return -1
        mn = min(balance)
        if mn >= 0:
            return 0
        need = -mn
        i = balance.index(mn)
        n = len(balance)
        ans = 0
        for j in range(1, n):
            a = balance[(i - j + n) % n]
            b = balance[(i + j - n) % n]
            c1 = min(a, need)
            need -= c1
            ans += c1 * j
            c2 = min(b, need)
            need -= c2
            ans += c2 * j
        return ans
```

#### Java

```java
class Solution {
    public long minMoves(int[] balance) {
        long sum = 0;
        for (int b : balance) {
            sum += b;
        }
        if (sum < 0) {
            return -1;
        }

        int n = balance.length;
        int mn = balance[0];
        int idx = 0;
        for (int i = 1; i < n; i++) {
            if (balance[i] < mn) {
                mn = balance[i];
                idx = i;
            }
        }

        if (mn >= 0) {
            return 0;
        }

        int need = -mn;
        long ans = 0;

        for (int j = 1; j < n; j++) {
            int a = balance[(idx - j + n) % n];
            int b = balance[(idx + j) % n];

            int c1 = Math.min(a, need);
            need -= c1;
            ans += (long) c1 * j;

            int c2 = Math.min(b, need);
            need -= c2;
            ans += (long) c2 * j;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minMoves(vector<int>& balance) {
        long long sum = 0;
        for (int b : balance) {
            sum += b;
        }
        if (sum < 0) {
            return -1;
        }

        int n = balance.size();
        int mn = balance[0];
        int idx = 0;
        for (int i = 1; i < n; i++) {
            if (balance[i] < mn) {
                mn = balance[i];
                idx = i;
            }
        }

        if (mn >= 0) {
            return 0;
        }

        int need = -mn;
        long long ans = 0;

        for (int j = 1; j < n; j++) {
            int a = balance[(idx - j + n) % n];
            int b = balance[(idx + j) % n];

            int c1 = min(a, need);
            need -= c1;
            ans += 1LL * c1 * j;

            int c2 = min(b, need);
            need -= c2;
            ans += 1LL * c2 * j;
        }

        return ans;
    }
};
```

#### Go

```go
func minMoves(balance []int) int64 {
	var sum int64
	for _, b := range balance {
		sum += int64(b)
	}
	if sum < 0 {
		return -1
	}

	n := len(balance)
	mn := balance[0]
	idx := 0
	for i := 1; i < n; i++ {
		if balance[i] < mn {
			mn = balance[i]
			idx = i
		}
	}

	if mn >= 0 {
		return 0
	}

	need := -mn
	var ans int64

	for j := 1; j < n; j++ {
		a := balance[(idx-j+n)%n]
		b := balance[(idx+j)%n]

		c1 := min(a, need)
		need -= c1
		ans += int64(c1) * int64(j)

		c2 := min(b, need)
		need -= c2
		ans += int64(c2) * int64(j)
	}

	return ans
}
```

#### TypeScript

```ts
function minMoves(balance: number[]): number {
    const sum = balance.reduce((a, b) => a + b, 0);
    if (sum < 0) {
        return -1;
    }

    const n = balance.length;
    let mn = balance[0],
        idx = 0;
    for (let i = 1; i < n; i++) {
        if (balance[i] < mn) {
            mn = balance[i];
            idx = i;
        }
    }

    if (mn >= 0) {
        return 0;
    }

    let need = -mn;
    let ans = 0;

    for (let j = 1; j < n; j++) {
        const a = balance[(idx - j + n) % n];
        const b = balance[(idx + j) % n];

        const c1 = Math.min(a, need);
        need -= c1;
        ans += c1 * j;

        const c2 = Math.min(b, need);
        need -= c2;
        ans += c2 * j;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
