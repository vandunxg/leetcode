---
comments: true
difficulty: Medium
rating: 1616
source: Biweekly Contest 76 Q3
tags:
    - Greedy
    - Design
    - Array
---

<!-- problem:start -->

# [2241. Design an ATM Machine](https://leetcode.com/problems/design-an-atm-machine)

[中文文档](/solution/2200-2299/2241.Design%20an%20ATM%20Machine/README.md)

## Mô tả

<!-- description:start -->

<p>Có một máy ATM lưu trữ tiền giấy thuộc <code>5</code> mệnh giá: <code>20</code>, <code>50</code>, <code>100</code>, <code>200</code> và <code>500</code> đô la. Ban đầu, máy ATM rỗng. Người dùng có thể dùng máy để nạp hoặc rút bất kỳ khoản tiền nào.</p>

<p>Khi rút tiền, máy sẽ ưu tiên sử dụng các tờ tiền có mệnh giá <strong>lớn hơn</strong>.</p>

<ul>
	<li>Ví dụ, nếu muốn rút <code>$300</code> và máy có <code>2</code> tờ tiền <code>$50</code>, <code>1</code> tờ tiền <code>$100</code> và <code>1</code> tờ tiền <code>$200</code>, máy sẽ sử dụng tờ tiền <code>$100</code> và <code>$200</code>.</li>
	<li>Tuy nhiên, nếu bạn cố rút <code>$600</code> trong khi máy có <code>3</code> tờ tiền <code>$200</code> và <code>1</code> tờ tiền <code>$500</code>, yêu cầu rút sẽ bị từ chối vì máy sẽ cố sử dụng tờ tiền <code>$500</code> trước, sau đó không thể dùng các tờ tiền còn lại để đủ <code>$100</code>. Lưu ý rằng máy <strong>không được phép</strong> sử dụng các tờ tiền <code>$200</code> thay cho tờ tiền <code>$500</code>.</li>
</ul>

<p>Hãy triển khai lớp ATM:</p>

<ul>
	<li><code>ATM()</code> khởi tạo đối tượng ATM.</li>
	<li><code>void deposit(int[] banknotesCount)</code> nạp các tờ tiền mới theo thứ tự <code>$20</code>, <code>$50</code>, <code>$100</code>, <code>$200</code> và <code>$500</code>.</li>
	<li><code>int[] withdraw(int amount)</code> trả về một mảng có độ dài <code>5</code>, biểu diễn số lượng tờ tiền sẽ đưa cho người dùng theo thứ tự <code>$20</code>, <code>$50</code>, <code>$100</code>, <code>$200</code> và <code>$500</code>, đồng thời cập nhật số lượng tờ tiền trong máy ATM sau khi rút. Trả về <code>[-1]</code> nếu không thể rút (trong trường hợp này <strong>không được</strong> rút bất kỳ tờ tiền nào).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;ATM&quot;, &quot;deposit&quot;, &quot;withdraw&quot;, &quot;deposit&quot;, &quot;withdraw&quot;, &quot;withdraw&quot;]
[[], [[0,0,1,2,1]], [600], [[0,1,0,1,1]], [600], [550]]
<strong>Đầu ra</strong>
[null, null, [0,0,1,0,1], null, [-1], [0,1,0,0,1]]

<strong>Giải thích</strong>
ATM atm = new ATM();
atm.deposit([0,0,1,2,1]); // Deposits 1 $100 banknote, 2 $200 banknotes,
                          // và 1 tờ tiền $500.
atm.withdraw(600);        // Trả về [0,0,1,0,1]. Máy sử dụng 1 tờ tiền $100
                          // và 1 tờ tiền $500. Các tờ tiền còn lại trong
                          // máy là [0,0,0,2,0].
atm.deposit([0,1,0,1,1]); // Nạp 1 tờ tiền $50, $200 và $500.
                          // Lúc này các tờ tiền trong máy là [0,1,0,3,1].
atm.withdraw(600);        // Trả về [-1]. Máy sẽ cố sử dụng tờ tiền $500
                          // trước, sau đó không thể đủ $100 còn lại,
                          // nên yêu cầu rút sẽ bị từ chối.
                          // Vì yêu cầu bị từ chối, số lượng tờ tiền
                          // trong máy không bị thay đổi.
atm.withdraw(550);        // Trả về [0,1,0,0,1]. Máy sử dụng 1 tờ tiền $50
                          // và 1 tờ tiền $500.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>banknotesCount.length == 5</code></li>
	<li><code>0 &lt;= banknotesCount[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= amount &lt;= 10<sup>9</sup></code></li>
	<li><strong>Tổng cộng</strong> có nhiều nhất <code>5000</code> lần gọi đến <code>withdraw</code> và <code>deposit</code>.</li>
	<li>Mỗi hàm <code>withdraw</code> và <code>deposit</code> được gọi ít nhất <strong>một</strong> lần.</li>
	<li>Tổng <code>banknotesCount[i]</code> trong tất cả các lần nạp không vượt quá <code>10<sup>9</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có năm mệnh giá, tối đa $5000$ lần thao tác và số tiền có thể lên tới $10^9$. Việc tìm kiếm các tổ hợp là không thể, hơn nữa máy phải ưu tiên các tờ tiền mệnh giá lớn hơn, nên ta dùng phép chia tham lam.
>
> Một mảng có độ dài $5$ lưu số tờ tiền hiện có. Thao tác nạp cộng trực tiếp vào mảng. Khi rút, ta duyệt các mệnh giá từ $500$ xuống $20$ và lấy $\min(\lfloor \textit{amount}/d_i \rfloor, \textit{cnt}[i])$ tờ tiền của mỗi mệnh giá. Nếu số tiền vẫn còn, yêu cầu thất bại và số tiền trong máy không đổi; ngược lại, ta trừ các số lượng tờ tiền đã rút.

<!-- thinking:end -->

Ta sử dụng một mảng $\textit{d}$ để lưu các mệnh giá tiền và một mảng $\textit{cnt}$ để lưu số lượng tờ tiền của mỗi mệnh giá.

Với thao tác `deposit`, ta chỉ cần cộng số lượng tờ tiền vào mệnh giá tương ứng. Độ phức tạp thời gian là $O(1)$.

Với thao tác `withdraw`, ta duyệt các tờ tiền từ mệnh giá lớn nhất đến nhỏ nhất, lấy nhiều tờ nhất có thể mà không vượt quá $\textit{amount}$. Sau đó, ta trừ tổng giá trị các tờ tiền đã rút khỏi $\textit{amount}$. Nếu cuối cùng $\textit{amount}$ vẫn lớn hơn $0$, nghĩa là không thể rút số tiền yêu cầu, và ta trả về $-1$. Ngược lại, ta trả về số lượng tờ tiền đã rút. Độ phức tạp thời gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class ATM:
    def __init__(self):
        self.d = [20, 50, 100, 200, 500]
        self.m = len(self.d)
        self.cnt = [0] * self.m

    def deposit(self, banknotesCount: List[int]) -> None:
        for i, x in enumerate(banknotesCount):
            self.cnt[i] += x

    def withdraw(self, amount: int) -> List[int]:
        ans = [0] * self.m
        for i in reversed(range(self.m)):
            ans[i] = min(amount // self.d[i], self.cnt[i])
            amount -= ans[i] * self.d[i]
        if amount > 0:
            return [-1]
        for i, x in enumerate(ans):
            self.cnt[i] -= x
        return ans


# Your ATM object will be instantiated and called as such:
# obj = ATM()
# obj.deposit(banknotesCount)
# param_2 = obj.withdraw(amount)
```

#### Java

```java
class ATM {
    private int[] d = {20, 50, 100, 200, 500};
    private int m = d.length;
    private long[] cnt = new long[5];

    public ATM() {
    }

    public void deposit(int[] banknotesCount) {
        for (int i = 0; i < banknotesCount.length; ++i) {
            cnt[i] += banknotesCount[i];
        }
    }

    public int[] withdraw(int amount) {
        int[] ans = new int[m];
        for (int i = m - 1; i >= 0; --i) {
            ans[i] = (int) Math.min(amount / d[i], cnt[i]);
            amount -= ans[i] * d[i];
        }
        if (amount > 0) {
            return new int[] {-1};
        }
        for (int i = 0; i < m; ++i) {
            cnt[i] -= ans[i];
        }
        return ans;
    }
}

/**
 * Your ATM object will be instantiated and called as such:
 * ATM obj = new ATM();
 * obj.deposit(banknotesCount);
 * int[] param_2 = obj.withdraw(amount);
 */
```

#### C++

```cpp
class ATM {
public:
    ATM() {
    }

    void deposit(vector<int> banknotesCount) {
        for (int i = 0; i < banknotesCount.size(); ++i) {
            cnt[i] += banknotesCount[i];
        }
    }

    vector<int> withdraw(int amount) {
        vector<int> ans(m);
        for (int i = m - 1; ~i; --i) {
            ans[i] = min(1ll * amount / d[i], cnt[i]);
            amount -= ans[i] * d[i];
        }
        if (amount > 0) {
            return {-1};
        }
        for (int i = 0; i < m; ++i) {
            cnt[i] -= ans[i];
        }
        return ans;
    }

private:
    static constexpr int d[5] = {20, 50, 100, 200, 500};
    static constexpr int m = size(d);
    long long cnt[m] = {0};
};

/**
 * Your ATM object will be instantiated and called as such:
 * ATM* obj = new ATM();
 * obj->deposit(banknotesCount);
 * vector<int> param_2 = obj->withdraw(amount);
 */
```

#### Go

```go
var d = [...]int{20, 50, 100, 200, 500}

const m = len(d)

type ATM [m]int

func Constructor() ATM {
	return ATM{}
}

func (this *ATM) Deposit(banknotesCount []int) {
	for i, x := range banknotesCount {
		this[i] += x
	}
}

func (this *ATM) Withdraw(amount int) []int {
	ans := make([]int, m)
	for i := m - 1; i >= 0; i-- {
		ans[i] = min(amount/d[i], this[i])
		amount -= ans[i] * d[i]
	}
	if amount > 0 {
		return []int{-1}
	}
	for i, x := range ans {
		this[i] -= x
	}
	return ans
}

/**
 * Your ATM object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Deposit(banknotesCount);
 * param_2 := obj.Withdraw(amount);
 */
```

#### TypeScript

```ts
const d: number[] = [20, 50, 100, 200, 500];
const m = d.length;

class ATM {
    private cnt: number[];

    constructor() {
        this.cnt = Array(m).fill(0);
    }

    deposit(banknotesCount: number[]): void {
        for (let i = 0; i < banknotesCount.length; ++i) {
            this.cnt[i] += banknotesCount[i];
        }
    }

    withdraw(amount: number): number[] {
        const ans: number[] = Array(m).fill(0);
        for (let i = m - 1; i >= 0; --i) {
            ans[i] = Math.min(Math.floor(amount / d[i]), this.cnt[i]);
            amount -= ans[i] * d[i];
        }
        if (amount > 0) {
            return [-1];
        }
        for (let i = 0; i < m; ++i) {
            this.cnt[i] -= ans[i];
        }
        return ans;
    }
}

/**
 * Your ATM object will be instantiated and called as such:
 * var obj = new ATM()
 * obj.deposit(banknotesCount)
 * var param_2 = obj.withdraw(amount)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
