---
comments: true
difficulty: Medium
rating: 1536
source: Biweekly Contest 168 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [3723. Maximize Sum of Squares of Digits](https://leetcode.com/problems/maximize-sum-of-squares-of-digits)

[Tài liệu tiếng Trung](/solution/3700-3799/3723.Maximize%20Sum%20of%20Squares%20of%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <strong>dương</strong> <code>num</code> và <code>sum</code>.</p>

<p>Một số nguyên dương <code>n</code> là <strong>tốt</strong> nếu thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Số chữ số của <code>n</code> <strong>chính xác bằng</strong> <code>num</code>.</li>
	<li>Tổng các chữ số của <code>n</code> <strong>chính xác bằng</strong> <code>sum</code>.</li>
</ul>

<p><strong>Điểm số</strong> của một số nguyên <strong>tốt</strong> <code>n</code> là tổng bình phương các chữ số của <code>n</code>.</p>

<p>Trả về một <strong>chuỗi</strong> biểu diễn số nguyên <strong>tốt</strong> <code>n</code> đạt <strong>điểm số</strong> <strong>lớn nhất</strong>. Nếu có nhiều số nguyên thỏa mãn, trả về số <strong>lớn nhất</strong>. Nếu không tồn tại số nguyên nào như vậy, trả về chuỗi rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 2, sum = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;30&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 3 số nguyên tốt: 12, 21 và 30.</p>

<ul>
	<li>Điểm số của 12 là <code>1<sup>2</sup> + 2<sup>2</sup> = 5</code>.</li>
	<li>Điểm số của 21 là <code>2<sup>2</sup> + 1<sup>2</sup> = 5</code>.</li>
	<li>Điểm số của 30 là <code>3<sup>2</sup> + 0<sup>2</sup> = 9</code>.</li>
</ul>

<p>Điểm số lớn nhất là 9, đạt được bởi số nguyên tốt 30. Do đó, đáp án là <code>&quot;30&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 2, sum = 17</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;98&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 số nguyên tốt: 89 và 98.</p>

<ul>
	<li>Điểm số của 89 là <code>8<sup>2</sup> + 9<sup>2</sup> = 145</code>.</li>
	<li>Điểm số của 98 là <code>9<sup>2</sup> + 8<sup>2</sup> = 145</code>.</li>
</ul>

<p>Điểm số lớn nhất là 145. Số nguyên tốt lớn nhất đạt được điểm số này là 98. Do đó, đáp án là <code>&quot;98&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 1, sum = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên nào có đúng 1 chữ số và tổng các chữ số bằng 10. Do đó, đáp án là <code>&quot;&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= sum &lt;= 2 * 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Với tổng chữ số cố định, tổng bình phương đạt lớn nhất khi sử dụng nhiều chữ số $9$ nhất có thể. Nếu $9\times num<sum$ thì không có đáp án; ngược lại, ta viết nhiều chữ số $9$ nhất có thể, đặt phần dư vào chữ số tiếp theo rồi thêm các số 0 để đủ độ dài $num$, cách này đồng thời tạo ra số nguyên lớn nhất trong các số thỏa mãn.

<!-- thinking:end -->

Nếu $\text{num} \times 9 < \text{sum}$ thì không tồn tại số nguyên tốt hợp lệ, vì vậy ta trả về chuỗi rỗng.

Ngược lại, ta có thể sử dụng nhiều chữ số $9$ nhất có thể để tạo số nguyên tốt, vì $9^2$ là lớn nhất và sẽ tối đa hóa điểm số. Cụ thể, ta tính số lượng chữ số $9$ có trong $\text{sum}$, ký hiệu là $k$, và phần còn lại $s = \text{sum} - 9 \times k$. Sau đó, ta tạo số nguyên tốt với $k$ chữ số đầu tiên là $9$. Nếu $s > 0$, ta thêm một chữ số $s$ vào cuối, rồi thêm các chữ số $0$ cho đến khi đủ tổng cộng $\text{num}$ chữ số.

Độ phức tạp thời gian là $O(\text{num})$ và độ phức tạp không gian là $O(\text{num})$, trong đó $\text{num}$ là số chữ số của số nguyên tốt.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumOfSquares(self, num: int, sum: int) -> str:
        if num * 9 < sum:
            return ""
        k, s = divmod(sum, 9)
        ans = "9" * k
        if s:
            ans += digits[s]
        ans += "0" * (num - len(ans))
        return ans
```

#### Java

```java
class Solution {
    public String maxSumOfSquares(int num, int sum) {
        if (num * 9 < sum) {
            return "";
        }
        int k = sum / 9;
        sum %= 9;
        StringBuilder ans = new StringBuilder("9".repeat(k));
        if (sum > 0) {
            ans.append(sum);
        }
        ans.append("0".repeat(num - ans.length()));
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maxSumOfSquares(int num, int sum) {
        if (num * 9 < sum) {
            return "";
        }
        int k = sum / 9, s = sum % 9;
        string ans(k, '9');
        if (s > 0) {
            ans += char('0' + s);
        }
        ans += string(num - ans.size(), '0');
        return ans;
    }
};
```

#### Go

```go
func maxSumOfSquares(num int, sum int) string {
	if num*9 < sum {
		return ""
	}

	k, s := sum/9, sum%9
	ans := strings.Repeat("9", k)
	if s > 0 {
		ans += string('0' + byte(s))
	}
	if len(ans) < num {
		ans += strings.Repeat("0", num-len(ans))
	}

	return ans
}
```

#### TypeScript

```ts
function maxSumOfSquares(num: number, sum: number): string {
    if (num * 9 < sum) {
        return '';
    }

    const k = Math.floor(sum / 9);
    const s = sum % 9;

    let ans = '9'.repeat(k);
    if (s > 0) {
        ans += String.fromCharCode('0'.charCodeAt(0) + s);
    }
    if (ans.length < num) {
        ans += '0'.repeat(num - ans.length);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
