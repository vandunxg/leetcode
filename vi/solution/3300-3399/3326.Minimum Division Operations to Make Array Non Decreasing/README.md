---
comments: true
difficulty: Medium
rating: 1864
source: Weekly Contest 420 Q3
tags:
    - Greedy
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3326. Minimum Division Operations to Make Array Non Decreasing](https://leetcode.com/problems/minimum-division-operations-to-make-array-non-decreasing)

[中文文档](/solution/3300-3399/3326.Minimum%20Division%20Operations%20to%20Make%20Array%20Non%20Decreasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Mọi <strong>ước dương</strong> của một số tự nhiên <code>x</code> <strong>nhỏ hơn nghiêm ngặt</strong> <code>x</code> được gọi là <strong>ước thực sự</strong> của <code>x</code>. Ví dụ, 2 là một <em>ước thực sự</em> của 4, còn 6 không phải là <em>ước thực sự</em> của 6.</p>

<p>Bạn được phép thực hiện một <strong>phép toán</strong> với <code>nums</code> nhiều lần tùy ý. Trong mỗi <strong>phép toán</strong>, bạn chọn <em>một</em> phần tử bất kỳ trong <code>nums</code> và chia nó cho <strong>ước thực sự</strong> <strong>lớn nhất</strong> của nó.</p>

<p>Trả về số <strong>phép toán</strong> <strong>nhỏ nhất</strong> cần thực hiện để mảng trở thành <strong>không giảm</strong>.</p>

<p>Nếu <strong>không thể</strong> biến mảng thành <em>không giảm</em> bằng bất kỳ số phép toán nào, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [25,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau một phép toán, 25 được chia cho 5 và <code>nums</code> trở thành <code>[5, 7]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,7,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một phép toán chia $x$ cho một ước thực sự. Số nguyên tố không thể giảm; số hợp thành sẽ trở thành ước nguyên tố nhỏ nhất của nó sau một phép chia như vậy. Với $n \le 10^5$ và $M \le 10^6$, ta tiền xử lý các ước nguyên tố nhỏ nhất.
>
> Nếu duyệt từ trái sang phải, ta phải xử lý các ràng buộc ở phía sau khi chúng chưa được biết. Khi duyệt từ phải sang trái, giá trị kế tiếp đã cố định, nên một phần tử bên trái lớn hơn nó phải được đổi ngay thành $\textit{lpf}[x]$.
>
> Nếu ngay cả ước nguyên tố nhỏ nhất cũng lớn hơn phần tử bên phải, mảng không thể trở thành không giảm. Mỗi chỉ số được thực hiện phép toán nhiều nhất một lần, nên chỉ cần một lượt duyệt từ phải sang trái để đếm đáp án.

<!-- thinking:end -->

Theo mô tả bài toán,

Nếu một số nguyên $x$ là số nguyên tố, ước thực sự lớn nhất của nó là $1$, nên $x / 1 = x$, nghĩa là không thể tiếp tục chia $x$.

Nếu một số nguyên $x$ không phải là số nguyên tố, giả sử ước thực sự lớn nhất của $x$ là $y$, khi đó $x / y$ chắc chắn là một số nguyên tố. Vì vậy, ta tìm ước nguyên tố nhỏ nhất $\textit{lpf}[x]$ sao cho $x \bmod \textit{lpf}[x] = 0$, khiến $x$ trở thành $\textit{lpf}[x]$, sau đó không thể tiếp tục chia.

Do đó, ta có thể tiền xử lý ước nguyên tố nhỏ nhất của mỗi số từ $1$ đến $10^6$. Sau đó, ta duyệt mảng từ phải sang trái. Nếu phần tử hiện tại lớn hơn phần tử kế tiếp, ta đổi phần tử hiện tại thành ước nguyên tố nhỏ nhất của nó. Nếu sau khi đổi mà phần tử hiện tại vẫn lớn hơn phần tử kế tiếp, nghĩa là không thể biến mảng thành không giảm, và ta trả về $-1$. Ngược lại, ta tăng số phép toán lên một. Tiếp tục duyệt cho đến khi xử lý toàn bộ mảng.

Độ phức tạp thời gian của bước tiền xử lý là $O(M \times \log \log M)$, trong đó $M = 10^6$. Độ phức tạp thời gian của bước duyệt mảng là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(M)$.

<!-- tabs:start -->

#### Python3

```python
mx = 10**6 + 1
lpf = [0] * (mx + 1)
for i in range(2, mx + 1):
    if lpf[i] == 0:
        for j in range(i, mx + 1, i):
            if lpf[j] == 0:
                lpf[j] = i


class Solution:
    def minOperations(self, nums: List[int]) -> int:
        ans = 0
        for i in range(len(nums) - 2, -1, -1):
            if nums[i] > nums[i + 1]:
                nums[i] = lpf[nums[i]]
                if nums[i] > nums[i + 1]:
                    return -1
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    private static final int MX = (int) 1e6 + 1;
    private static final int[] LPF = new int[MX + 1];
    static {
        for (int i = 2; i <= MX; ++i) {
            for (int j = i; j <= MX; j += i) {
                if (LPF[j] == 0) {
                    LPF[j] = i;
                }
            }
        }
    }
    public int minOperations(int[] nums) {
        int ans = 0;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] > nums[i + 1]) {
                nums[i] = LPF[nums[i]];
                if (nums[i] > nums[i + 1]) {
                    return -1;
                }
                ans++;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
const int MX = 1e6;
int LPF[MX + 1];

auto init = [] {
    for (int i = 2; i <= MX; i++) {
        if (LPF[i] == 0) {
            for (int j = i; j <= MX; j += i) {
                if (LPF[j] == 0) {
                    LPF[j] = i;
                }
            }
        }
    }
    return 0;
}();

class Solution {
public:
    int minOperations(vector<int>& nums) {
        int ans = 0;
        for (int i = nums.size() - 2; i >= 0; i--) {
            if (nums[i] > nums[i + 1]) {
                nums[i] = LPF[nums[i]];
                if (nums[i] > nums[i + 1]) {
                    return -1;
                }
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
const mx int = 1e6

var lpf = [mx + 1]int{}

func init() {
    for i := 2; i <= mx; i++ {
        if lpf[i] == 0 {
            for j := i; j <= mx; j += i {
                if lpf[j] == 0 {
                    lpf[j] = i
                }
            }
        }
    }
}

func minOperations(nums []int) (ans int) {
    for i := len(nums) - 2; i >= 0; i-- {
        if nums[i] > nums[i+1] {
            nums[i] = lpf[nums[i]]
            if nums[i] > nums[i+1] {
                return -1
            }
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
const mx = 10 ** 6;
const lpf = Array(mx + 1).fill(0);
for (let i = 2; i <= mx; ++i) {
    for (let j = i; j <= mx; j += i) {
        if (lpf[j] === 0) {
            lpf[j] = i;
        }
    }
}

function minOperations(nums: number[]): number {
    let ans = 0;
    for (let i = nums.length - 2; ~i; --i) {
        if (nums[i] > nums[i + 1]) {
            nums[i] = lpf[nums[i]];
            if (nums[i] > nums[i + 1]) {
                return -1;
            }
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
