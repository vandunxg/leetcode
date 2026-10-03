---
comments: true
difficulty: Medium
rating: 1356
source: Weekly Contest 263 Q2
tags:
    - Design
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2043. Simple Bank System](https://leetcode.com/problems/simple-bank-system)

[中文文档](/solution/2000-2099/2043.Simple%20Bank%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được giao nhiệm vụ viết một chương trình cho một ngân hàng phổ biến để tự động hóa tất cả các giao dịch đến, bao gồm chuyển tiền, nạp tiền và rút tiền. Ngân hàng có <code>n</code> tài khoản được đánh số từ <code>1</code> đến <code>n</code>. Số dư ban đầu của mỗi tài khoản được lưu trong một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>balance</code>, trong đó tài khoản thứ <code>(i + 1)<sup>th</sup></code> có số dư ban đầu là <code>balance[i]</code>.</p>

<p>Thực hiện tất cả các giao dịch <strong>hợp lệ</strong>. Một giao dịch là <strong>hợp lệ</strong> khi:</p>

<ul>
	<li>Số tài khoản được cung cấp nằm trong khoảng từ <code>1</code> đến <code>n</code>, và</li>
	<li>Số tiền được rút hoặc chuyển đi <strong>nhỏ hơn hoặc bằng</strong> số dư của tài khoản.</li>
</ul>

<p>Hãy cài đặt lớp <code>Bank</code>:</p>

<ul>
	<li><code>Bank(long[] balance)</code> Khởi tạo đối tượng với mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>balance</code>.</li>
	<li><code>boolean transfer(int account1, int account2, long money)</code> Chuyển <code>money</code> đô la từ tài khoản có số <code>account1</code> sang tài khoản có số <code>account2</code>. Trả về <code>true</code> nếu giao dịch thành công, ngược lại trả về <code>false</code>.</li>
	<li><code>boolean deposit(int account, long money)</code> Nạp <code>money</code> đô la vào tài khoản có số <code>account</code>. Trả về <code>true</code> nếu giao dịch thành công, ngược lại trả về <code>false</code>.</li>
	<li><code>boolean withdraw(int account, long money)</code> Rút <code>money</code> đô la khỏi tài khoản có số <code>account</code>. Trả về <code>true</code> nếu giao dịch thành công, ngược lại trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Bank&quot;, &quot;withdraw&quot;, &quot;transfer&quot;, &quot;deposit&quot;, &quot;transfer&quot;, &quot;withdraw&quot;]
[[[10, 100, 20, 50, 30]], [3, 10], [5, 1, 20], [5, 20], [3, 4, 15], [10, 50]]
<strong>Đầu ra</strong>
[null, true, true, true, false, false]

<strong>Giải thích</strong>
Bank bank = new Bank([10, 100, 20, 50, 30]);
bank.withdraw(3, 10);    // trả về true, tài khoản 3 có số dư $20, so it is valid to withdraw $10.
                         // Tài khoản 3 còn $20 - $10 = $10.
bank.transfer(5, 1, 20); // trả về true, tài khoản 5 có số dư $30, so it is valid to transfer $20.
                         // Tài khoản 5 còn $30 - $20 = $10, and account 1 has $10 + $20 = $30.
bank.deposit(5, 20);     // trả về true, có thể nạp $20 vào tài khoản 5.
                         // Tài khoản 5 có $10 + $20 = $30.
bank.transfer(3, 4, 15); // trả về false, số dư hiện tại của tài khoản 3 là $10,
                         // nên không thể chuyển $15 từ tài khoản này.
bank.withdraw(10, 50);   // trả về false, vì tài khoản 10 không tồn tại.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == balance.length</code></li>
	<li><code>1 &lt;= n, account, account1, account2 &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= balance[i], money &lt;= 10<sup>12</sup></code></li>
	<li>Mỗi hàm <code>transfer</code>, <code>deposit</code>, <code>withdraw</code> được gọi <strong>nhiều nhất</strong> <code>10<sup>4</sup></code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Có rất nhiều tài khoản, nhưng mỗi lần gọi chỉ truy cập vào một hoặc hai số dư. Ta lưu chúng trong một mảng; vì số tài khoản được đánh số từ $1$ nên ta truy cập phần tử tại chỉ số $account-1$.
>
> Với chuyển tiền và rút tiền, ta kiểm tra số tài khoản và số dư; với nạp tiền, ta chỉ cần kiểm tra số tài khoản. Mỗi thao tác có độ phức tạp $O(1)$.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{balance}$ để lưu số dư của mỗi tài khoản. Với mỗi thao tác, ta chỉ cần thực hiện các bước kiểm tra và cập nhật cần thiết theo đề bài.

Với thao tác $\textit{transfer}$, ta cần kiểm tra số tài khoản có hợp lệ hay không và số dư có đủ hay không. Nếu các điều kiện được đáp ứng, ta thực hiện việc chuyển tiền.

Với thao tác $\textit{deposit}$, ta chỉ cần kiểm tra số tài khoản có hợp lệ hay không, sau đó thực hiện việc nạp tiền.

Với thao tác $\textit{withdraw}$, ta cần kiểm tra số tài khoản có hợp lệ hay không và số dư có đủ hay không. Nếu các điều kiện được đáp ứng, ta thực hiện việc rút tiền.

Mỗi thao tác có độ phức tạp thời gian là $O(1)$, nên độ phức tạp thời gian tổng thể là $O(q)$, trong đó $q$ là số thao tác. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Bank:
    def __init__(self, balance: List[int]):
        self.balance = balance
        self.n = len(balance)

    def transfer(self, account1: int, account2: int, money: int) -> bool:
        if account1 > self.n or account2 > self.n or self.balance[account1 - 1] < money:
            return False
        self.balance[account1 - 1] -= money
        self.balance[account2 - 1] += money
        return True

    def deposit(self, account: int, money: int) -> bool:
        if account > self.n:
            return False
        self.balance[account - 1] += money
        return True

    def withdraw(self, account: int, money: int) -> bool:
        if account > self.n or self.balance[account - 1] < money:
            return False
        self.balance[account - 1] -= money
        return True


# Your Bank object will be instantiated and called as such:
# obj = Bank(balance)
# param_1 = obj.transfer(account1,account2,money)
# param_2 = obj.deposit(account,money)
# param_3 = obj.withdraw(account,money)
```

#### Java

```java
class Bank {
    private long[] balance;
    private int n;

    public Bank(long[] balance) {
        this.balance = balance;
        this.n = balance.length;
    }

    public boolean transfer(int account1, int account2, long money) {
        if (account1 > n || account2 > n || balance[account1 - 1] < money) {
            return false;
        }
        balance[account1 - 1] -= money;
        balance[account2 - 1] += money;
        return true;
    }

    public boolean deposit(int account, long money) {
        if (account > n) {
            return false;
        }
        balance[account - 1] += money;
        return true;
    }

    public boolean withdraw(int account, long money) {
        if (account > n || balance[account - 1] < money) {
            return false;
        }
        balance[account - 1] -= money;
        return true;
    }
}

/**
 * Your Bank object will be instantiated and called as such:
 * Bank obj = new Bank(balance);
 * boolean param_1 = obj.transfer(account1,account2,money);
 * boolean param_2 = obj.deposit(account,money);
 * boolean param_3 = obj.withdraw(account,money);
 */
```

#### C++

```cpp
class Bank {
public:
    vector<long long> balance;
    int n;

    Bank(vector<long long>& balance) {
        this->balance = balance;
        n = balance.size();
    }

    bool transfer(int account1, int account2, long long money) {
        if (account1 > n || account2 > n || balance[account1 - 1] < money) return false;
        balance[account1 - 1] -= money;
        balance[account2 - 1] += money;
        return true;
    }

    bool deposit(int account, long long money) {
        if (account > n) return false;
        balance[account - 1] += money;
        return true;
    }

    bool withdraw(int account, long long money) {
        if (account > n || balance[account - 1] < money) return false;
        balance[account - 1] -= money;
        return true;
    }
};

/**
 * Your Bank object will be instantiated and called as such:
 * Bank* obj = new Bank(balance);
 * bool param_1 = obj->transfer(account1,account2,money);
 * bool param_2 = obj->deposit(account,money);
 * bool param_3 = obj->withdraw(account,money);
 */
```

#### Go

```go
type Bank struct {
	balance []int64
	n       int
}

func Constructor(balance []int64) Bank {
	return Bank{balance, len(balance)}
}

func (this *Bank) Transfer(account1 int, account2 int, money int64) bool {
	if account1 > this.n || account2 > this.n || this.balance[account1-1] < money {
		return false
	}
	this.balance[account1-1] -= money
	this.balance[account2-1] += money
	return true
}

func (this *Bank) Deposit(account int, money int64) bool {
	if account > this.n {
		return false
	}
	this.balance[account-1] += money
	return true
}

func (this *Bank) Withdraw(account int, money int64) bool {
	if account > this.n || this.balance[account-1] < money {
		return false
	}
	this.balance[account-1] -= money
	return true
}

/**
 * Your Bank object will be instantiated and called as such:
 * obj := Constructor(balance);
 * param_1 := obj.Transfer(account1,account2,money);
 * param_2 := obj.Deposit(account,money);
 * param_3 := obj.Withdraw(account,money);
 */
```

#### TypeScript

```ts
class Bank {
    balance: number[];
    constructor(balance: number[]) {
        this.balance = balance;
    }

    transfer(account1: number, account2: number, money: number): boolean {
        if (
            account1 > this.balance.length ||
            account2 > this.balance.length ||
            money > this.balance[account1 - 1]
        )
            return false;
        this.balance[account1 - 1] -= money;
        this.balance[account2 - 1] += money;
        return true;
    }

    deposit(account: number, money: number): boolean {
        if (account > this.balance.length) return false;
        this.balance[account - 1] += money;
        return true;
    }

    withdraw(account: number, money: number): boolean {
        if (account > this.balance.length || money > this.balance[account - 1]) {
            return false;
        }
        this.balance[account - 1] -= money;
        return true;
    }
}

/**
 * Your Bank object will be instantiated and called as such:
 * var obj = new Bank(balance)
 * var param_1 = obj.transfer(account1,account2,money)
 * var param_2 = obj.deposit(account,money)
 * var param_3 = obj.withdraw(account,money)
 */
```

#### Rust

```rust
struct Bank {
    balance: Vec<i64>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl Bank {
    fn new(balance: Vec<i64>) -> Self {
        Bank { balance }
    }

    fn transfer(&mut self, account1: i32, account2: i32, money: i64) -> bool {
        let (account1, account2, n) = (account1 as usize, account2 as usize, self.balance.len());
        if n < account1 || n < account2 {
            return false;
        }
        if self.balance[account1 - 1] < money {
            return false;
        }
        self.balance[account1 - 1] -= money;
        self.balance[account2 - 1] += money;
        true
    }

    fn deposit(&mut self, account: i32, money: i64) -> bool {
        let (account, n) = (account as usize, self.balance.len());
        if n < account {
            return false;
        }
        self.balance[account - 1] += money;
        true
    }

    fn withdraw(&mut self, account: i32, money: i64) -> bool {
        let (account, n) = (account as usize, self.balance.len());
        if n < account {
            return false;
        }
        if self.balance[account - 1] < money {
            return false;
        }
        self.balance[account - 1] -= money;
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
