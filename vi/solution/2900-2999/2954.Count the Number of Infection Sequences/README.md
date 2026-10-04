---
comments: true
difficulty: Hard
rating: 2644
source: Weekly Contest 374 Q4
tags:
    - Array
    - Math
    - Combinatorics
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [2954. Count the Number of Infection Sequences](https://leetcode.com/problems/count-the-number-of-infection-sequences)

[中文文档](/solution/2900-2999/2954.Count%20the%20Number%20of%20Infection%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một mảng <code>sick</code> được sắp xếp theo thứ tự tăng dần, biểu diễn vị trí của những người bị nhiễm trong một hàng gồm <code>n</code> người.</p>

<p>Ở mỗi bước, <strong>một </strong> người chưa bị nhiễm <strong>kề</strong> với một người bị nhiễm sẽ bị nhiễm. Quá trình này tiếp tục cho đến khi tất cả mọi người đều bị nhiễm.</p>

<p><strong>Chuỗi lây nhiễm</strong> là thứ tự những người chưa bị nhiễm trở thành người bị nhiễm, không bao gồm những người đã bị nhiễm từ đầu.</p>

<p>Trả về số chuỗi lây nhiễm khác nhau có thể xảy ra, lấy modulo <code>10<sup>9</sup>+7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, sick = [0,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có tổng cộng 6 chuỗi khác nhau.</p>

<ul>
	<li>Các chuỗi lây nhiễm hợp lệ là <code>[1,2,3]</code>, <code>[1,3,2]</code>, <code>[3,2,1]</code> và <code>[3,1,2]</code>.</li>
	<li><code>[2,3,1]</code> và <code>[2,1,3]</code> không phải chuỗi lây nhiễm hợp lệ vì người ở chỉ số 2 không thể bị nhiễm ngay ở bước đầu tiên.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, sick = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có tổng cộng 6 chuỗi khác nhau.</p>

<ul>
	<li>Các chuỗi lây nhiễm hợp lệ là <code>[0,2,3]</code>, <code>[2,0,3]</code> và <code>[2,3,0]</code>.</li>
	<li><code>[3,2,0]</code>, <code>[3,0,2]</code> và <code>[0,3,2]</code> không phải chuỗi lây nhiễm hợp lệ vì quá trình lây nhiễm bắt đầu từ người ở chỉ số 1, sau đó thứ tự lây nhiễm là 2 rồi 3, nên 3 không thể bị nhiễm trước 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sick.length &lt;= n - 1</code></li>
	<li><code>0 &lt;= sick[i] &lt;= n - 1</code></li>
	<li><code>sick</code> được sắp xếp theo thứ tự tăng dần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán tổ hợp + Nghịch đảo nhân + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ số bị nhiễm chia những người khỏe thành các đoạn. Thứ tự lây nhiễm cho tất cả các đoạn là multinomial $s! / \prod x_i!$. Các đoạn ở hai đầu chỉ phát triển từ một phía; một đoạn ở giữa có độ dài $x$ luôn có thể mở rộng từ một trong hai đầu, tạo ra $2^{x-1}$ cách.
>
> Ta tính trước các giai thừa, dùng nghịch đảo cho phép chia và lũy thừa nhị phân để tính lũy thừa. Vì $n \le 10^5$, cần một bước tiền xử lý tuyến tính.

<!-- thinking:end -->

Theo mô tả bài toán, những người bị nhiễm chia những người chưa bị nhiễm thành một số đoạn liên tiếp. Ta có thể dùng mảng $nums$ để ghi lại số người chưa bị nhiễm trong mỗi đoạn, tổng số người chưa bị nhiễm là $s = \sum_{i=0}^{k} nums[k]$. Ta nhận thấy số chuỗi lây nhiễm chính là số hoán vị của $s$ phần tử khác nhau, tức là $s!$.

Giả sử mỗi đoạn người chưa bị nhiễm chỉ có một cách lây nhiễm, tổng số chuỗi lây nhiễm sẽ là $\frac{s!}{\prod_{i=0}^{k} nums[k]!}$.

Tiếp theo, ta xét cách lây nhiễm trong từng đoạn người chưa bị nhiễm. Giả sử một đoạn có $x$ người chưa bị nhiễm, đoạn đó có $2^{x-1}$ cách lây nhiễm, vì mỗi lần ta có thể chọn một đầu trong hai đầu trái và phải của đoạn để lây nhiễm, tức là có hai lựa chọn trong tổng cộng $x-1$ lần lây nhiễm. Tuy nhiên, nếu đó là đoạn đầu tiên hoặc đoạn cuối cùng thì chỉ có một lựa chọn.

Tóm lại, tổng số chuỗi lây nhiễm là:

$$
\frac{s!}{\prod_{i=0}^{k} nums[k]!} \prod_{i=1}^{k-1} 2^{nums[i]-1}
$$

Cuối cùng, vì đáp án có thể rất lớn và cần lấy modulo $10^9 + 7$, ta cần tiền xử lý giai thừa và nghịch đảo nhân.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là độ dài của mảng $sick$. Không tính phần không gian dùng cho mảng tiền xử lý, độ phức tạp không gian là $O(m)$.

<!-- tabs:start -->

#### Python3

```python
mod = 10**9 + 7
mx = 10**5
fac = [1] * (mx + 1)
for i in range(2, mx + 1):
    fac[i] = fac[i - 1] * i % mod


class Solution:
    def numberOfSequence(self, n: int, sick: List[int]) -> int:
        nums = [b - a - 1 for a, b in pairwise([-1] + sick + [n])]
        ans = 1
        s = sum(nums)
        ans = fac[s]
        for x in nums:
            if x:
                ans = ans * pow(fac[x], mod - 2, mod) % mod
        for x in nums[1:-1]:
            if x > 1:
                ans = ans * pow(2, x - 1, mod) % mod
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) (1e9 + 7);
    private static final int MX = 100000;
    private static final int[] FAC = new int[MX + 1];

    static {
        FAC[0] = 1;
        for (int i = 1; i <= MX; i++) {
            FAC[i] = (int) ((long) FAC[i - 1] * i % MOD);
        }
    }

    public int numberOfSequence(int n, int[] sick) {
        int m = sick.length;
        int[] nums = new int[m + 1];
        nums[0] = sick[0];
        nums[m] = n - sick[m - 1] - 1;
        for (int i = 1; i < m; i++) {
            nums[i] = sick[i] - sick[i - 1] - 1;
        }
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        int ans = FAC[s];
        for (int x : nums) {
            if (x > 0) {
                ans = (int) ((long) ans * qpow(FAC[x], MOD - 2) % MOD);
            }
        }
        for (int i = 1; i < nums.length - 1; ++i) {
            if (nums[i] > 1) {
                ans = (int) ((long) ans * qpow(2, nums[i] - 1) % MOD);
            }
        }
        return ans;
    }

    private int qpow(long a, long n) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % MOD;
            }
            a = a * a % MOD;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
const int MX = 1e5;
const int MOD = 1e9 + 7;
int fac[MX + 1];

auto init = [] {
    fac[0] = 1;
    for (int i = 1; i <= MX; ++i) {
        fac[i] = 1LL * fac[i - 1] * i % MOD;
    }
    return 0;
}();

int qpow(long long a, long long n) {
    long long ans = 1;
    for (; n > 0; n >>= 1) {
        if (n & 1) {
            ans = (ans * a) % MOD;
        }
        a = (a * a) % MOD;
    }
    return ans;
}

class Solution {
public:
    int numberOfSequence(int n, vector<int>& sick) {
        int m = sick.size();
        vector<int> nums(m + 1);

        nums[0] = sick[0];
        nums[m] = n - sick[m - 1] - 1;
        for (int i = 1; i < m; i++) {
            nums[i] = sick[i] - sick[i - 1] - 1;
        }

        int s = accumulate(nums.begin(), nums.end(), 0);
        long long ans = fac[s];
        for (int x : nums) {
            if (x > 0) {
                ans = ans * qpow(fac[x], MOD - 2) % MOD;
            }
        }
        for (int i = 1; i < nums.size() - 1; ++i) {
            if (nums[i] > 1) {
                ans = ans * qpow(2, nums[i] - 1) % MOD;
            }
        }
        return ans;
    }
};
```

#### Go

```go
const MX = 1e5
const MOD = 1e9 + 7

var fac [MX + 1]int

func init() {
    fac[0] = 1
    for i := 1; i <= MX; i++ {
        fac[i] = fac[i-1] * i % MOD
    }
}

func qpow(a, n int) int {
    ans := 1
    for n > 0 {
        if n&1 == 1 {
            ans = (ans * a) % MOD
        }
        a = (a * a) % MOD
        n >>= 1
    }
    return ans
}

func numberOfSequence(n int, sick []int) int {
    m := len(sick)
    nums := make([]int, m+1)

    nums[0] = sick[0]
    nums[m] = n - sick[m-1] - 1
    for i := 1; i < m; i++ {
        nums[i] = sick[i] - sick[i-1] - 1
    }

    s := 0
    for _, x := range nums {
        s += x
    }
    ans := fac[s]
    for _, x := range nums {
        if x > 0 {
            ans = ans * qpow(fac[x], MOD-2) % MOD
        }
    }
    for i := 1; i < len(nums)-1; i++ {
        if nums[i] > 1 {
            ans = ans * qpow(2, nums[i]-1) % MOD
        }
    }
    return ans
}
```

#### TypeScript

```ts
const MX = 1e5;
const MOD: bigint = BigInt(1e9 + 7);
const fac: bigint[] = Array(MX + 1);

const init = (() => {
    fac[0] = 1n;
    for (let i = 1; i <= MX; ++i) {
        fac[i] = (fac[i - 1] * BigInt(i)) % MOD;
    }
    return 0;
})();

function qpow(a: bigint, n: number): bigint {
    let ans = 1n;
    for (; n > 0; n >>= 1) {
        if (n & 1) {
            ans = (ans * a) % MOD;
        }
        a = (a * a) % MOD;
    }
    return ans;
}

function numberOfSequence(n: number, sick: number[]): number {
    const m = sick.length;
    const nums: number[] = Array(m + 1);
    nums[0] = sick[0];
    nums[m] = n - sick[m - 1] - 1;
    for (let i = 1; i < m; i++) {
        nums[i] = sick[i] - sick[i - 1] - 1;
    }

    const s = nums.reduce((acc, x) => acc + x, 0);
    let ans = fac[s];
    for (let x of nums) {
        if (x > 0) {
            ans = (ans * qpow(fac[x], Number(MOD) - 2)) % MOD;
        }
    }
    for (let i = 1; i < nums.length - 1; ++i) {
        if (nums[i] > 1) {
            ans = (ans * qpow(2n, nums[i] - 1)) % MOD;
        }
    }
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
