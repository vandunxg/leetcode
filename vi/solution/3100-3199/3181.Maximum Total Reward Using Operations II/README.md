---
comments: true
difficulty: Hard
rating: 2688
source: Weekly Contest 401 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3181. Maximum Total Reward Using Operations II](https://leetcode.com/problems/maximum-total-reward-using-operations-ii)

[Tài liệu tiếng Trung](/solution/3100-3199/3181.Maximum%20Total%20Reward%20Using%20Operations%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>rewardValues</code> có độ dài <code>n</code>, biểu diễn các giá trị phần thưởng.</p>

<p>Ban đầu, tổng phần thưởng <code>x</code> của bạn là 0 và tất cả các chỉ số đều <strong>chưa được đánh dấu</strong>. Bạn được phép thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn một chỉ số <strong>chưa được đánh dấu</strong> <code>i</code> trong phạm vi <code>[0, n - 1]</code>.</li>
	<li>Nếu <code>rewardValues[i]</code> <strong>lớn hơn</strong> tổng phần thưởng hiện tại <code>x</code>, cộng <code>rewardValues[i]</code> vào <code>x</code> (tức là <code>x = x + rewardValues[i]</code>) và <strong>đánh dấu</strong> chỉ số <code>i</code>.</li>
</ul>

<p>Hãy trả về một số nguyên biểu thị <em>tổng phần thưởng</em> <strong>lớn nhất</strong> mà bạn có thể thu thập bằng cách thực hiện các thao tác một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rewardValues = [1,1,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong quá trình thực hiện, ta có thể lần lượt chọn để đánh dấu các chỉ số 0 và 2, khi đó tổng phần thưởng là 4, đây là giá trị lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rewardValues = [1,6,4,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lần lượt đánh dấu các chỉ số 0, 2 và 1. Khi đó tổng phần thưởng là 11, đây là giá trị lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rewardValues.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= rewardValues[i] &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc giống phần I nhưng miền giá trị lớn hơn, nên mảng Boolean với độ phức tạp $O(nM)$ không đáp ứng được.
>
> Phép cập nhật vẫn là “dịch $v$ bit thấp sang trái $v$ bit rồi OR trở lại”, và bitset có thể thực hiện thao tác này theo từng word.
>
> Sau khi loại bỏ phần tử trùng và sắp xếp, bắt đầu với $f=1$, áp dụng $f\mathrel{|}=(f\&((1\ll v)-1))\ll v$, rồi trả về $f.bit\_length()-1$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là liệu có thể đạt tổng phần thưởng $j$ bằng cách sử dụng $i$ giá trị phần thưởng đầu tiên hay không. Ban đầu, $f[0][0] = \textit{True}$, còn tất cả các giá trị khác là $\textit{False}$.

Xét giá trị phần thưởng thứ $i$ là $v$. Nếu không chọn nó, thì $f[i][j] = f[i - 1][j]$; nếu chọn nó, thì $f[i][j] = f[i - 1][j - v]$, trong đó $0 \leq j - v < v$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j] \vee f[i - 1][j - v]
$$

Đáp án cuối cùng là $\max\{j \mid f[n][j] = \textit{True}\}$.

Vì $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j]$ và $f[i - 1][j - v]$, ta có thể loại bỏ chiều đầu tiên và chỉ dùng mảng một chiều để chuyển trạng thái. Ngoài ra, do miền dữ liệu của bài toán này lớn, ta cần dùng thao tác bit để tối ưu hiệu quả chuyển trạng thái.

Ta định nghĩa một số nhị phân $f$ để lưu trạng thái hiện tại, trong đó bit thứ $i$ của $f$ bằng $1$ cho biết có thể đạt tổng phần thưởng $i$.

Quan sát công thức chuyển trạng thái $f[j] = f[j] \vee f[j - v]$, ta thấy điều này tương đương với việc lấy $v$ bit thấp của $f$, dịch chúng sang trái $v$ bit, rồi thực hiện phép OR với $f$ ban đầu.

Do đó, đáp án là vị trí của bit cao nhất trong $f$.

Độ phức tạp thời gian là $O(n \times M / w)$, và độ phức tạp không gian là $O(n + M / w)$. Trong đó, $n$ là độ dài của mảng `rewardValues`, $M$ là hai lần giá trị lớn nhất trong mảng `rewardValues`, còn số nguyên $w = 32$ hoặc $64$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        nums = sorted(set(rewardValues))
        f = 1
        for v in nums:
            f |= (f & ((1 << v) - 1)) << v
        return f.bit_length() - 1
```

#### Java

```java
import java.math.BigInteger;

class Solution {
    public int maxTotalReward(int[] rewardValues) {
        int[] nums = Arrays.stream(rewardValues).distinct().sorted().toArray();
        BigInteger f = BigInteger.ONE;
        for (int v : nums) {
            BigInteger mask = BigInteger.ONE.shiftLeft(v).subtract(BigInteger.ONE);
            BigInteger shifted = f.and(mask).shiftLeft(v);
            f = f.or(shifted);
        }
        return f.bitLength() - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTotalReward(vector<int>& rewardValues) {
        sort(rewardValues.begin(), rewardValues.end());
        rewardValues.erase(unique(rewardValues.begin(), rewardValues.end()), rewardValues.end());
        bitset<100000> f{1};
        for (int v : rewardValues) {
            int shift = f.size() - v;
            f |= f << shift >> (shift - v);
        }
        for (int i = rewardValues.back() * 2 - 1;; i--) {
            if (f.test(i)) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func maxTotalReward(rewardValues []int) int {
	slices.Sort(rewardValues)
	rewardValues = slices.Compact(rewardValues)
	one := big.NewInt(1)
	f := big.NewInt(1)
	p := new(big.Int)
	for _, v := range rewardValues {
		mask := p.Sub(p.Lsh(one, uint(v)), one)
		f.Or(f, p.Lsh(p.And(f, mask), uint(v)))
	}
	return f.BitLen() - 1
}
```

#### TypeScript

```ts
function maxTotalReward(rewardValues: number[]): number {
    rewardValues.sort((a, b) => a - b);
    rewardValues = [...new Set(rewardValues)];
    let f = 1n;
    for (const x of rewardValues) {
        const mask = (1n << BigInt(x)) - 1n;
        f = f | ((f & mask) << BigInt(x));
    }
    return f.toString(2).length - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
