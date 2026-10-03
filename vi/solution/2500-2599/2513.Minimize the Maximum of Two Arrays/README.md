---
comments: true
difficulty: Medium
rating: 2302
source: Biweekly Contest 94 Q3
tags:
    - Math
    - Binary Search
    - Number Theory
    - Inclusion-Exclusion
    - Least Common Multiple
---

<!-- problem:start -->

# [2513. Minimize the Maximum of Two Arrays](https://leetcode.com/problems/minimize-the-maximum-of-two-arrays)

[中文文档](/solution/2500-2599/2513.Minimize%20the%20Maximum%20of%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Chúng ta có hai mảng <code>arr1</code> và <code>arr2</code>, ban đầu đều rỗng. Bạn cần thêm các số nguyên dương vào chúng sao cho thỏa mãn tất cả các điều kiện sau:</p>

<ul>
	<li><code>arr1</code> chứa <code>uniqueCnt1</code> số nguyên dương <strong>phân biệt</strong>, mỗi số <strong>không chia hết</strong> cho <code>divisor1</code>.</li>
	<li><code>arr2</code> chứa <code>uniqueCnt2</code> số nguyên dương <strong>phân biệt</strong>, mỗi số <strong>không chia hết</strong> cho <code>divisor2</code>.</li>
	<li><strong>Không</strong> có số nguyên nào xuất hiện trong cả <code>arr1</code> và <code>arr2</code>.</li>
</ul>

<p>Cho <code>divisor1</code>, <code>divisor2</code>, <code>uniqueCnt1</code> và <code>uniqueCnt2</code>, hãy trả về <em>số nguyên lớn nhất <strong>nhỏ nhất có thể</strong> xuất hiện trong một trong hai mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Ta có thể phân phối 4 số tự nhiên đầu tiên vào arr1 và arr2.
arr1 = [1] và arr2 = [2,3,4].
Có thể thấy cả hai mảng đều thỏa mãn tất cả điều kiện.
Vì giá trị lớn nhất là 4, ta trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ở đây, arr1 = [1,2] và arr2 = [3] thỏa mãn tất cả điều kiện.
Vì giá trị lớn nhất là 3, ta trả về giá trị này.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong>
Hai mảng có thể là arr1 = [1,3,5,7,9,11,13,15] và arr2 = [2,6].
Có thể chứng minh rằng không thể đạt được giá trị lớn nhất nhỏ hơn mà vẫn thỏa mãn tất cả điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= divisor1, divisor2 &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= uniqueCnt1, uniqueCnt2 &lt; 10<sup>9</sup></code></li>
	<li><code>2 &lt;= uniqueCnt1 + uniqueCnt2 &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta phải chọn $\textit{uniqueCnt1}$ và $\textit{uniqueCnt2}$ số nguyên dương phân biệt cho hai mảng, lần lượt loại các bội của $\textit{divisor1}$ và $\textit{divisor2}$, đồng thời tối thiểu hóa số nguyên lớn nhất được sử dụng. Giá trị lớn nhất này có thể rất lớn, nên việc lần lượt gán các số từ $1$ là không khả thi.
>
> Tính khả thi đơn điệu theo cận trên $x$, vì vậy ta dùng tìm kiếm nhị phân trên $x$. Số lượng số nguyên trong $[1,x]$ không chia hết cho $d$ là $x-\lfloor x/d\rfloor$. Mỗi mảng cần đủ số không chia hết cho số chia tương ứng, và tổng số phần tử của hai mảng không được vượt quá số lượng số nguyên không chia hết cho $\operatorname{lcm}(\textit{divisor1},\textit{divisor2})$. $\textit{bisect\_left}$ trả về $x$ nhỏ nhất khả thi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeSet(
        self, divisor1: int, divisor2: int, uniqueCnt1: int, uniqueCnt2: int
    ) -> int:
        def f(x):
            cnt1 = x // divisor1 * (divisor1 - 1) + x % divisor1
            cnt2 = x // divisor2 * (divisor2 - 1) + x % divisor2
            cnt = x // divisor * (divisor - 1) + x % divisor
            return (
                cnt1 >= uniqueCnt1
                and cnt2 >= uniqueCnt2
                and cnt >= uniqueCnt1 + uniqueCnt2
            )

        divisor = lcm(divisor1, divisor2)
        return bisect_left(range(10**10), True, key=f)
```

#### Java

```java
class Solution {
    public int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        long divisor = lcm(divisor1, divisor2);
        long left = 1, right = 10000000000L;
        while (left < right) {
            long mid = (left + right) >> 1;
            long cnt1 = mid / divisor1 * (divisor1 - 1) + mid % divisor1;
            long cnt2 = mid / divisor2 * (divisor2 - 1) + mid % divisor2;
            long cnt = mid / divisor * (divisor - 1) + mid % divisor;
            if (cnt1 >= uniqueCnt1 && cnt2 >= uniqueCnt2 && cnt >= uniqueCnt1 + uniqueCnt2) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return (int) left;
    }

    private long lcm(int a, int b) {
        return (long) a * b / gcd(a, b);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        long left = 1, right = 1e10;
        long divisor = lcm((long) divisor1, (long) divisor2);
        while (left < right) {
            long mid = (left + right) >> 1;
            long cnt1 = mid / divisor1 * (divisor1 - 1) + mid % divisor1;
            long cnt2 = mid / divisor2 * (divisor2 - 1) + mid % divisor2;
            long cnt = mid / divisor * (divisor - 1) + mid % divisor;
            if (cnt1 >= uniqueCnt1 && cnt2 >= uniqueCnt2 && cnt >= uniqueCnt1 + uniqueCnt2) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func minimizeSet(divisor1 int, divisor2 int, uniqueCnt1 int, uniqueCnt2 int) int {
	divisor := lcm(divisor1, divisor2)
	left, right := 1, 10000000000
	for left < right {
		mid := (left + right) >> 1
		cnt1 := mid/divisor1*(divisor1-1) + mid%divisor1
		cnt2 := mid/divisor2*(divisor2-1) + mid%divisor2
		cnt := mid/divisor*(divisor-1) + mid%divisor
		if cnt1 >= uniqueCnt1 && cnt2 >= uniqueCnt2 && cnt >= uniqueCnt1+uniqueCnt2 {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}

func lcm(a, b int) int {
	return a * b / gcd(a, b)
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
